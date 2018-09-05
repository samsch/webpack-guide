# Basic Webpack Config With React JSX Support

Install packages:
`npm i -D @babel/core @babel/plugin-syntax-dynamic-import @babel/preset-env @babel/preset-react babel-loader webpack webpack-cli webpack-dev-server`

You'll also install React in your regular dependencies: `npm i react react-dom`.

> Also don't forget to install the Polyfill `npm i @babel/polyfill` and import it in your entrypoint `import '@babel/polyfill';`.

`webpack.config.js`:
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
        // Add in the 'x?' here to support both .js and .jsx files.
        test: /\.jsx?$/,
        exclude: /node_modules/,
        use: {
          loader: 'babel-loader',
        },
      },
    ],
  },
  devServer: {
    contentBase: path.join(__dirname, 'dist'),
    // Uncomment to allow connections from local network.
    // host: '0.0.0.0',
    
    // Enables self-signed tls cert for development.
    // https: true,
    
    // Can make all 404 requests fall back to index.html. Useful when doing client-side routing.
    // historyApiFallback: true,
    
    // You can proxy requests for non-bundled files to another server.
    // proxy: {
    //   // All requests which start with `/api`.
    //   '/api': {
    //     target: 'http://localhost:3000',
    //     changeOrigin: true,
    //   },
    // },
  },
};
```

`babel.config.js`:
```js
module.exports = {
  presets: [
    ['@babel/preset-env', {
      useBuiltIns: 'entry',
      modules: false,
      targets: [
        'last 2 firefox versions',
        'last 2 chrome versions',
        'last 2 edge versions',
        'last 2 ios versions',
      ],
    }],
    // Add the React preset here
    '@babel/preset-react',
  ],
  plugins: [
    '@babel/plugin-syntax-dynamic-import',
  ],
};
```

Now we can use JSX in our JavaScript files and it will be properly compiled to JavaScript.
