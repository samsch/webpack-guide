# Development Only Configuration

Some extensions for Webpack are only meant for use in development or production. There are a couple ways to handle this. One option is to use separate configuration files (where both might import from a shared config file and add the specifics). The other option is to export a function from your Webpack config, and use the argv argument to build the configuration for development or production dynamically.

> [More about using multiple files](https://webpack.js.org/guides/production/)
>
> [More about alternate Webpack config export types](https://webpack.js.org/configuration/configuration-types/#exporting-a-function)

Here's what our basic Webpack config looks like with a function export:
```js
const path = require('path');

module.exports = function (env, argv) {
  // Get a boolean value to check if in production mode
  const isProduction = argv.mode === 'production';
  return {
    entry: './src/index.js',
    output: {
      filename: 'main.js',
      path: path.resolve(__dirname, 'dist')
    },
    // We don't want any sourcemaps in production, but we do in development!
    devtool: isProduction ? false : 'eval',
    module: {
      rules: [
        {
          test: /\.js$/,
          exclude: /node_modules/,
          use: {
            loader: 'babel-loader',
          },
        },
      ],
    },
    devServer: {
      contentBase: path.join(__dirname, 'dist'),
    },
  };
};
```
The only things changed from our base config are that we are exporting a function that returns are config, and that our function checks whether the mode is production to set the `devtool` option.

> This is a contrived example; devtool defaults to off for production and `'eval'` for development mode anyway.

Back to the [index](/README.md).
