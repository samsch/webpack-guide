# Getting started with Webpack

Webpack is a powerful tool for bundling browser code. In the last several years it has grown into the defacto standard bundler.

On my blog, you can find a decent getting started guide, which takes you from scratch to a basic setup with babel-loader for compiling modern JavaScript syntax.

- [Getting Started with Webpack v4](https://samsch.org/2018/09/02/how-to-setup-webpack-v4)

The content covered in that post covers parts of Webpack that apply to almost any configuration. Frequently you want to do more than just bundling a single entrypoint and supporting older browsers.

These are some basics which cover general Webpack configuration topics.

- [Development only configuration options](/Development%20Only%20Configuration.md)
- [Using Webpack plugins](/Using%20Webpack%20Plugins.md)

These next guides cover more specific topics, but in a more succinct format where instead of walking through the setup, they show you the finished configuration and explain what was added and what it does.

- [Code splitting (Separate a tree of your application)](/Code%20splitting.md)
- [Importing stylesheets](/Importing%20Stylesheets.md)
- *Importing static assets* Coming soon!
- *Transforming non-JS code into JS (such as JSX, Typescript, Flow, and JavaScript proposal features)* Coming soon!
- *Create a map (manifest) of output files for programmatic consumption* Coming soon!
