# Using Webpack Plugins

Webpack functionality can be extended with plugins for additional tasks, such as executing find and replace on identifiers in source files, and automatically inserting script tags into html, and several features of Webpack itself are implemented as plugins, including uglification and showing progress while bundling.

General usage of plugins in Webpack is to `require()` the plugin in the Webpack config file and include it in the `plugin` key in the config. Usually plugins will have documentation which shows exactly how to use them.

Here's an example using [ManifestPlugin](https://www.npmjs.com/package/webpack-manifest-plugin).
```js
const path = require('path');
// We import our plugin at the top.
const ManifestPlugin = require('webpack-manifest-plugin');

module.exports = {
  // This is the default entrypoint, which uses the name "main", giving an entrypoint of "main.js".
  entry: './src/index.js',
  output: {
    // We'll use a hash name here to show the mapping in the manifest file.
    filename: '[hash].main.js',
    path: path.resolve(__dirname, 'dist')
  },
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
  // We add plugins in the plugins key.
  plugins: [
    // Most plugins are initiated with the `new` keyword.
    new ManifestPlugin(),
  ],
  devServer: {
    contentBase: path.join(__dirname, 'dist'),
  },
};
```

When you run Webpack with this config, an extra file (`manifest.json`) is output, which maps the entrypoint names to the actual filenames. Since we changed our output filename to `[hash].main.js`, Webpack will include a short hash of the content in the name (so the name changes when the build output changes). If we wanted to programmatically use the filename in a server side html template, we can get the filename by importing the `manifest.json` file and using the entrypoint name (in our case, the default "main" translates to an entrypoint name "main.js". If you use named entrypoints it will use the given name + ".js").

Back to the [index](/README.md).
