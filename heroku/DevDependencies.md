# Dev dependencies in heroku deployment

By default heroku run all of the npm commands with `NODE_ENV=production`[documentation](https://devcenter.heroku.com/articles/nodejs-support#devdependencies)

The workaround, add in your `packages.json`

```
"heroku-postbuild": "npm install --dev && npm run gulp task && npm prune --production",
```
