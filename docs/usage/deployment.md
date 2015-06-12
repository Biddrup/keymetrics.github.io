---
layout: docs
title: Deployment
description: How to use PM2.
permalink: /docs/usage/deployment/
---

PM2 embed a simple and powerful deployment system with revision tracing.
It's based on <a href="https://github.com/visionmedia/deploy">https://github.com/visionmedia/deploy</a>

A step-by-step tutorial is available here : [Deploy and Iterate faster with PM2 deploy](https://keymetrics.io/2014/06/25/ecosystem-json-deploy-and-iterate-faster/)

## Getting started with deployment

Please read the [Considerations to use PM2 deploy](#considerations)

1- Generate a sample ecosystem.json file that list processes and deployment environment

```bash
$ pm2 ecosystem
```

In the current folder a `ecosystem.json` file will be created.
It contains this:

```json
{
  "apps" : [{
    "name"      : "API",
    "script"    : "app.js",
    "env": {
      "COMMON_VARIABLE": "true"
    },
    "env_production" : {
      "NODE_ENV": "production"
    }
  },{
    "name"      : "WEB",
    "script"    : "web.js"
  }],
  "deploy" : {
    "production" : {
      "user" : "node",
      "host" : "212.83.163.1",
      "ref"  : "origin/master",
      "repo" : "git@github.com:repo.git",
      "path" : "/var/www/production",
      "post-deploy" : "pm2 startOrRestart ecosystem.json --env production"
    },
    "dev" : {
      "user" : "node",
      "host" : "212.83.163.1",
      "ref"  : "origin/master",
      "repo" : "git@github.com:repo.git",
      "path" : "/var/www/development",
      "post-deploy" : "pm2 startOrRestart ecosystem.json --env dev",
      "env"  : {
        "NODE_ENV": "dev"
      }
    }
  }
}
```

Edit the file according to your needs.

2- Be sure that you have the public ssh key on your local machine

```bash
$ ssh-keygen -t rsa
$ ssh-copy-id root@myserver.com
```

3- Now initialize the remote folder with:

```bash
$ pm2 deploy <configuration_file> <environment> setup
```

E.g:

```bash
$ pm2 deploy ecosystem.json production setup
```

This command will create all the folders on your remote server.

4- Deploy your code

```bash
$ pm2 deploy ecosystem.json production
```

Now your code will be populated, installed and started with PM2

## Deployment options

Display deploy help via `pm2 deploy help`:

```
$ pm2 deploy <configuration_file> <environment> <command>

  Commands:
    setup                run remote setup commands
    update               update deploy to the latest release
    revert [n]           revert to [n]th last deployment or 1
    curr[ent]            output current release commit
    prev[ious]           output previous release commit
    exec|run <cmd>       execute the given <cmd>
    list                 list previous deploy commits
    [ref]                deploy to [ref], the "ref" setting, or latest tag
```

## Commands

```
$ pm2 startOrRestart all.json            # Invoke restart on all apps in JSON
$ pm2 startOrReload all.json             # Invoke reload
$ pm2 startOrGracefulReload all.json     # Invoke gracefulReload
```

## Using file key for authenticating

Just add the "key" attribute with file path to the .pem key within the attributes "user", "hosts"...

```
    "production" : {
      "key"  : "/path/to/some.pem",
      "user" : "node",
      "host" : "212.83.163.1",
      "ref"  : "origin/master",
      "repo" : "git@github.com:repo.git",
      "path" : "/var/www/production",
      "post-deploy" : "pm2 startOrRestart ecosystem.json --env production"
    },
```
## Considerations

- You might want to commit your node_modules folder ([#622](https://github.com/Unitech/pm2/issues/622)) or add the `npm install` command to the `post-deploy` section: `"post-deploy" : "npm install && pm2 startOrRestart ecosystem.json --env production"`
- Verify that your remote server has the permission to git clone the repository
- You can declare specific environment variable depending on the environment you want to deploy the code to. For instance to declare variables for the production environment, just add "env_production": {} and declare that variables.
- PM2 will look by default to `ecosystem.json`. So you can skip the <configuration_file> options if it's the case
- You can embed the "apps" & "deploy" section in the package.json
- It deploys your code via ssh, you don't need any dependencies
- Process are initialized / started automatically depending on application name in `ecosystem.json`
- PM2-deploy repository is there: [pm2-deploy](https://github.com/Unitech/pm2-deploy)

## Contributing

The module is <a href="https://github.com/Unitech/pm2-deploy">https://github.com/Unitech/pm2-deploy</a>
Feel free to PR for any changes or fix.