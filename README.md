# @hackr/multi-babel
![Build Status](https://travis-ci.com/athackr/webpack-multi-babel-plugin.svg?branch=master)

Add this plugin to generate an alternative babel compilation for your existing bundles. It's a fork of [`prateekbh/babel-esm-plugin`](https://github.com/prateekbh/babel-esm-plugin)
Works with Webpack4 and Babel7

```
yarn add -D @hackr/multi-babel
```

### Note
This plugin only works when you're already using `babel-preset-env`.

Also, there is an expectation that your `babel-preset-env` is configured in the shape:
```
{
  use: {
    loader: 'babel-loader',
    options: {
      "presets": [["@babel/preset-env", {
        "targets": {
          "browsers": ["last 2 versions", "safari >= 7"]
        }
        ....
      }]]
    },
  },
}
```

## Options
```js
new MultiBabelPlugin({
  filename: 'es6/[name].js',
  chunkFilename: 'es6/[id].js',
  excludedPlugins: [...],
  additionalPlugins: [...],
  beforeStartExecution: function(plugins, babelConfig) {}
});
```
1. `filename`: Output name of es6 bundles. (default: '[name].es6.js')
2. `chunkFilename`: Output name of es6 chunks. (default: '[id].es6.js')
3. `excludedPlugins`: List of plugins you want to exclude from generating es6 bundles.
4. `additionalPlugins`: List of plugins you want to add while generating es6 bundles.
5. `beforeStartExecution`: A callback function which passes all plugins and the new babel config, to a function where the user can modify them before starting the `ESM` build.

## How to use
```js
  const MultiBabelPlugin = require('@hackr/multi-babel');

  module.exports = {
    entry: {
      index: './index.js',
      home: './index2.js'
    },
    output: {
      filename: "[name].js"
    },
    module: {
      rules: [
        {
          test: /\.js$/,
          exclude: /(node_modules|bower_components)/,
          use: {
            loader: 'babel-loader',
            options: {
              "presets": [["@babel/preset-env", {
                "targets": {
                  "browsers": ["last 2 versions", "safari >= 7"]
                }
              }]]
            },
          },
        }
      ]
    },
    plugins: [
      new MultiBabelPlugin()
    ]
  }
```
