# Importing Stylesheets

A powerful feature of Webpack loaders is that you can import non-JavaScript files and have specific defined behavior for them.

We can import stylesheets and have them automatically injected into the page using [style-loader](https://webpack.js.org/loaders/style-loader/) and [css-loader](https://webpack.js.org/loaders/css-loader/), or css-loader and [MiniCssExtractPlugin](https://webpack.js.org/plugins/mini-css-extract-plugin/) to put imported styles in a separate css file.

## style-loader + css-loader

Using style loader, we can seemlessly include our stylesheets in our bundle file(s). This is the easiest way to get started, but it may not always be favorable as we can't separately cache the stylesheet for the browser, and styles won't work without JavaScript.

Install the loaders: `npm i -D style-loader css-loader`.

Update the Webpack config to add the loader rule:
```js
const path = require('path');

module.exports = {
  entry: './src/index.js',
  output: {
    filename: '[name].js',
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
      // We add a loader rule targetting .css files
      {
        test: /\.css$/,
        use: ['style-loader', 'css-loader'],
      },
    ],
  },
  devServer: {
    contentBase: path.join(__dirname, 'dist'),
  },
};
```

Now in our code we can import stylesheets like this:
```js
import './style.css';
```
Those styles will be included in the bundle and inserted in a `<style>` tag when the bundle runs.

> [More about running multiple loaders](https://webpack.js.org/configuration/module/#rule-use)

## MiniCssExtractPlugin + css-loader

Using MiniCssExtractPlugin, our stylesheets will be made into a separate .css file instead of in our bundle.

Install the loaders and plugin: `npm i -D mini-css-extract-plugin css-loader`.

Update the Webpack config to import and use the plugin and add the loader rule:
```js
const path = require('path');
const MiniCssExtractPlugin = require('mini-css-extract-plugin');

module.exports = {
  entry: './src/index.js',
  output: {
    filename: '[name].js',
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
      {
        test: /\.css$/,
        use: [MiniCssExtractPlugin.loader, 'css-loader'],
      },
    ],
  },
  plugins: [
    new MiniCssExtractPlugin(),
  ],
  devServer: {
    contentBase: path.join(__dirname, 'dist'),
  },
};
```

Now when you build, there will be a .css file in your output folder. You should add the `<link>` tag in your html to include the stylesheet.

There are a couple ways to effectively use this plugin. If you have a single bundle (no code splitting and a single entrypoint), you can just use the output filename. If you have multiple bundle files in your output, and multiple css files, it can be easier to track the files by using the ManifestPlugin and injecting the files in server side templates.

## css-modules, postcss, sass, and less

You can enable css-modules with the `modules` option for `css-loader`. Replace `'css-loader'` with:
```js
{
  loader: 'css-loader',
  options: {
    modules: true,
  },
},
```

Now you can import the class names directly into your JavaScript, giving you effectively scoped css.

```js
import styles from './style.css';`
element.className = styles.someClass;
```
```css
.someClass {
  background-color: gray;
}
```

You can compile [postcss](https://webpack.js.org/loaders/postcss-loader/), [sass](https://webpack.js.org/loaders/sass-loader/), and [less](https://webpack.js.org/loaders/less-loader/) styles by adding their respective loaders.

Back to the [index](/README.md).
