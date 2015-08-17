---
layout: docs
title: Watch & Restart
description: Restart process on file change
permalink: /docs/usage/watch-and-restart/
---

## Watch & Restart

PM2 can automatically restart your app when a file changes in the current directory or its subdirectories:

```bash
$ pm2 start app.js --watch
```

If `--watch` is enabled, stopping it won't stop watching:
- `pm2 stop 0` will not stop watching
- `pm2 stop --watch 0` will stop watching

Restart toggle the `watch` parameter when triggered.

To watch specific paths, please use a JS/JSON app declaration, `watch` can take a string or an array of paths. Default is `true`:

```json
{
  "watch": ["server", "client"],
  "ignore_watch" : ["node_modules", "client/img"],
  "watch_options": {
    "followSymlinks": false
  }
}
```

As specified in the [Schema](#a988):
- `watch` can be a boolean, an array of paths or a string representing a path. Default to `false`
- `ignore_watch` can be an array of paths or a string, it'll be interpreted by [chokidar](https://github.com/paulmillr/chokidar#path-filtering) as a glob or a regular expression.
- `watch_options` is an object that will replace options given to chokidar. Please refer to [chokidar documentation](https://github.com/paulmillr/chokidar#api) for the definition.

PM2 is giving chokidar these Default options:

```
var watch_options = {
  persistent    : true,
  ignoreInitial : true
}
```

When working with NFS devices you'll need to set `usePolling: true` as stated in [this chokidar issue](https://github.com/paulmillr/chokidar/issues/242).
