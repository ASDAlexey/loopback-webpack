# LoopBack + webpack build example

This is a fork of the [Getting started with LoopBack](http://docs.strongloop.com/display/LB/Getting+started+with+LoopBack) tutorial demonstrating how to build a LoopBack application with [`webpack`](https://webpack.github.io/). Specifically we handle issues relating to [`loopback-boot`](https://apidocs.strongloop.com/loopback-boot/) and associated dynamic module dependencies.

This follows the general outline of [Simon Degraeve](https://github.com/SimonDegraeve)'s [`loopback-webpack-plugin`](https://github.com/SimonDegraeve/loopback-webpack-plugin) which appears to have been abandoned and no longer working. We also draw on ideas from [`webpack-node-externals`](https://github.com/liady/webpack-node-externals).

This could be developed into a webpack plugin at some point, but as an example it's clearer to configure webpack this way.

The key features of the approach are:
* Rather than call `boot()` at runtime, we perform a `loopback-boot` *compile* at build time and store the resulting *boot instructions* as a bundled JSON resource.
* At runtime, we just call the `loopback-boot` executor to perform the boot. This avoids many problems trying to bundle the compiler and also provides much faster boot times for complex applications.
* All of the boot-time resources are specified in a [`ContextReplacementPlugin`](https://webpack.github.io/docs/list-of-plugins.html#contextreplacementplugin) by providing a single *dependency map* that resolves all of the resources specified in the boot instructions. This feature of `ContextReplacementPlugin` is currently undocumented.
* No need for browserify!
* We specify which `node_modules` dependencies will be bundled (internalized) and which will not. `loopback-boot/lib/executor` must be bundled so webpack can handle resolution of models, boot scripts, etc.
* Dynamic configuration files (such as `config.json` and `datasources.json`) are excluded from the bundle (externalized) so that they can be modified without re-building.
* [`gulp`](http://gulpjs.com) is used trivially to perform the build.

#### Installation

```bash
git clone git://github.com/zamb3zi/loopback-webpack-example
cd loopback-webpack-example
npm install
gulp
node build/main.bundle.js
```

---

[More LoopBack examples](https://github.com/strongloop/loopback-example)
