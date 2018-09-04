# Code splitting

If your bundle is getting too large, and significant chunks of your code don't need to be loaded until later (perhaps some extra libraries for page Z), you can use code splitting to separately load those parts of your app.

Webpack supports this by default using dynamic `import()`. The call will return a Promise which resolves to an object of the named exports and a `default` property with the default export.

Usage can look like this:

```js
function a () {
  import('./pageZ')
    .then(({ default: pageZ }) => {
      displayPage(pageZ);
    })
    .catch(error => {
      displayLoadingError(error);
    })
}
```

While Webpack does support this directly, you do need to add a Babel plugin for it to be parsed correctly.

Install with `npm i -D @babel/plugin-syntax-dynamic-import`.

Add the plugin to babel.config.js:
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
    }]
  ],
  // Add these lines:
  plugins: [
    '@babel/plugin-syntax-dynamic-import',
  ],
};
```

All this plugin does is tell Babel how to understand the usage of `import()`. Since `import` is a keyword, using it without the plugin is a parse error.

Back to the [index](/README.md).
