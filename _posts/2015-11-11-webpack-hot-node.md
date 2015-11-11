---
layout: post
title: "Using Webpack to hot reload React Components with Node.js backend"
date: 2015-11-11 00:58:29 -0700
comments: true
categories: react webpack Node.js REPL
excerpt: Recently I migrated the front-end build process of a project from Gulp to Webpack.
---

## Migration to webpack

Recently I migrated the front-end build process of a project from Gulp to Webpack, majorly because of the awesome Hot Module Replacement (HMR) feature and the easiness of ES6 support. Check out Dan Abramov's [talk](https://www.youtube.com/watch?v=xsSnOQynTHs) if you haven't heard of hot reloading.

I used to use Gulp and browserify to transform JSX code. Having tried out webpack in another little experiment, I saw the potential it could bring to our current workflow. After the migration, Gulp is still used for running automated test cases, but webpack handles all the front-end bundling process.

Pete hunt wrote this excellent [tutorial](https://github.com/petehunt/webpack-howto) if you have used tools like browserify and want to try out webpack. Dan Abramov's [minimal hot reloading boilerplate](https://github.com/gaearon/react-hot-boilerplate) and [react-transform boilerplate](https://github.com/gaearon/react-transform-boilerplate)(with error handling) are also a good point to start.

## webpack-dev-server config

Here's some config to bring up webpack-dev-server.

### webpack.config.js

``` javascript
var path = require('path');
var webpack = require('webpack');

module.exports = {
  devtool: 'eval',
  entry: [
    'webpack-dev-server/client?http://localhost:3000',
    'webpack/hot/only-dev-server',
    './public/js/app'
  ],
  output: {
    path: path.join(__dirname, 'public', 'js'),
    filename: 'bundle.js',
    publicPath: 'http://localhost:3000/js/'
  },
  resolve: {
    extensions: ['', '.js', '.jsx']
  },
  plugins: [
    // HMR
    new webpack.HotModuleReplacementPlugin(),
    new webpack.NoErrorsPlugin()
  ],
  module: {
    loaders: [
      {
        test: /\.(js|jsx)$/,
        loaders: ['react-hot', 'babel'],
        include: path.join(__dirname, 'public', 'js')
      }
    ]
  }
}
```

## webpack-server.js

``` javascript
var webpack = require('webpack');
var WebpackDevServer = require('webpack-dev-server');
var config = require('./webpack.config');

module.exports = function(){
  new WebpackDevServer(webpack(config), {
    publicPath: config.output.publicPath,
    hot: true,
    historyApiFallback: true
  }).listen(3000, 'localhost', function (err, result) {
    if (err) {
      console.log(err);
    }

    console.log('WebpackDevServer Listening at localhost:3000');
  });
};
```

[webpack-dev-server](https://webpack.github.io/docs/webpack-dev-server.html) is an Express server that uses webpack-dev-middleware to serve a webpack bundle. The *entry point* `webpack-dev-server/client?http://localhost:3000` provides the client entry point for the webpack dev server to run as inline mode. This configuration is loaded by `webpack-server.js`. The `publicPath: 'http://localhost:3000/js/'` configuration puts the bundle under `http://localhost:3000/js/bundle.js`. (if you open [http://localhost:3000/webpack-dev-server](http://localhost:3000/webpack-dev-server) you can see it). One thing to note is that `publicPath` is not only used for dev purposes, but can also be used for [async loading](https://github.com/petehunt/webpack-howto#9-async-loading).

The *entry point* `webpack/hot/only-dev-server` and the plugin `new webpack.HotModuleReplacementPlugin()` in webpack configuration, along with `hot: true` in webpack-dev-server configuration are needed to enable Hot Module Replacement on the server. Updated modules are loaded and injected them into the running app when you updates the code.

A little difference here from the webpack-dev-server tutorial is that `webpack/hot/only-dev-server` is used together with `webpack.NoErrorsPlugin()` to [disable page reload](https://github.com/webpack/webpack/issues/418) if HMR updates fails.

## Node config

### server.js

``` javascript
if (process.env.NODE_ENV !== 'production'){
	var proxy = require('proxy-middleware');
	var url = require('url');
	app.use('/js', proxy(url.parse('http://localhost:3000/js')));
}

if (process.env.NODE_ENV !== 'production'){
	// run web pack server on 3000
	// only for hot reloading (8080 doesn't hot reload)
	(require('./webpack-server'))();
}
```

Here's what we need to change in our Node.js server, which serves the front end files as well as backend services. When the client requests the js files, it proxies the request to the webpack-dev-server (that hot reloads any changes). When the client makes API calls, it still goes to the request handlers in Node.

So running `node server.js` will launch both the node server and webpack-dev-server while webpack-dev-server hot reloads your React component. To run node in production (and disable the webpack-dev-server), you just have to run: `NODE_ENV=production node server.js`.

Another approach is to use proxy settings in webpack-dev-server config to forward all requests to backend (by `proxy: { "*":"http://localhost:8080" }`), and hot reloading is available on `localhost:3000`, as described in this [blog post](http://ctheu.com/2015/05/14/using-react-hot-loader-with-a-webpack-dev-server-and-a-node-server/). As I fiddle around with it a bit, you can enjoy hot reloading on both ends if both proxies are set.

## Development With React Hot Loader

The results are nothing but satisfaction. Productivity sky-rocketed. (rough estimate: 3-4 times). React Hot Loader makes creating prototypal interfaces almost feeling like a breeze. It's like messing with some other websites using developer tools, only you can turn the results into production code with little efforts. :)

## Reflections

We should have done the migration *much much* sooner. Took me around an hour for the migration; saved dozens of hours of debugging and distractions.

The [REPL-driven](https://www.youtube.com/watch?v=D9j_Mf91M0I) development works for front end development too:

1. Read: You type in some code, the browser reads it
2. Evaluate: The browser is your HTML/CSS/Javascript interpreter
3. Print: The browser renders the web page, you see the results
4. Loop: Go to read

Live reloading shortens the feedback loop (especially in 1. Read) and hot reloading takes it to the next level because it **doesn't lose the state of the application**. This helps you stay in the [flow](https://en.wikipedia.org/wiki/Flow_(psychology) as long as possible and achieve hyperproductivity.

As brilliantly stated in this well known talk ["Inventing on Principle"](https://www.youtube.com/watch?v=PUv66718DII) by Bret Victor:

>  Creators need to be able to see what they are doing. If you are designing something embedded in time, you need to be able to control time. You need to be able to see across time, otherwise it's designing as blind

Also, the quicker you are able to spot the bug, the less hassle you will experience later in development. Another interesting talk [Why does react scale](https://www.youtube.com/watch?v=D-ioDiacTm8) by Christopher Cheddar explains how React can enable developers to quickly locate the bug by limiting who can change the states and where the states can be changed.

Next up, I will write about how we integrate Docker in the workflow.
