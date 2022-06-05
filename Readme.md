# Tutorial ReactApp with EWF

	This tutorial is divided in three parts. The first one describe how to 
	configure React with WebPack and Babel using a simple React App..

	The second part build a simple EWF REST API.

	The last part we build a simple React Sample to consume the EWF REST API.

## Create a React application from scracth by configuring Webpack and Babel.

### Why do we need a Module Bundler?

Module bundlers are tools frontend developers used to bundle JavaScript modules into single JavaScript files that can be executed in the browsers. This is required because module systems ([ESM](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules) or [CommonJs](https://requirejs.org/docs/commonjs.html)) are still not supported by the browsers.  Examples of module blunders are: [Webpack](https://webpack.js.org/), [Browserfy](https://browserify.org/),[rollup](https://rollupjs.org/guide/en/),  [parcel](https://parceljs.org/), etc

To learn more about [JavaScript Modules and Browser Support]( https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules)


So a module bundler takes file of different types (js, Html, images, CSS) and packages them in groups of smaller files. It also manages dependencies and import them in the correct order. In this tutorial, we will use [webpack](https://webpack.js.org/), since it's one of the most used ones.

### Why do we need Babel?

With React we will be using the latest version of JavaScript code. Most browsers
don't understand the latest version of Javascript, which is why we need a transpiler, a tool that converts one type of source code to another type of source code. [Babel](https://babeljs.io/) is a transpiler, that converts ES5 and ES6 code to code that browsers understand.

Note: you will need to have Node.js and NPM installed.

```
	$>node --version
	$>v18.0.0

	$>npm --version
	$>8.6.0
``` 

If you don't have it install it with : 
* https://github.com/nvm-sh/nvm 
* https://github.com/coreybutler/nvm-windows

## Setup React App.

First things first, let's create our project folder, where we will have our React application.

```
mkdir react_app

cd react_app

```

### Initialize the Project

We initialize our project by running [npm init](https://docs.npmjs.com/cli/v8/commands/npm-init#examples). This will create a `package.json` file to keep track of our dependencies, development dependencies, metadata, and more.

```
npm init -y
```
The -y parameter generates an empty npm project without going through an interactive process. 

## Installing Required Dependencies.

### Installing React and ReactDOM
Before we can write some code, we need to install [React](https://reactjs.org/) and ReactDOM 


``` 
npm install react react-dom
```

[ReactDOM](https://en.reactjs.org/docs/react-dom.html) is a package that's an entry point to the DOM and server renderers for React.


### Installing Webpack

Lets now install webpack, as a development dependency since we only need for development. We also need to install as development dependencies [webpack-cli](https://www.npmjs.com/package/webpack-cli) (to run webpack commands) and [webpack-dev-server](https://www.npmjs.com/package/webpack-dev-server) (to run React locally with live reload)

```
	npm install webpack webpack-cli webpack-dev-server --save-dev
```

### Installing Babel/core
We will install the package [@babel/core](https://babeljs.io/docs/en/babel-core) the code transpiler as a dev dependency. 

```
npm install @babel/core --save-dev
```

### Installing Babel/loader
We need to install the package [babel-loader](https://webpack.js.org/loaders/babel-loader/) which is a webpack loader that will help webpack to use babel transpiler. Loads ES2015+ code and transpiles to ES5 using Babel

There are different types of [loaders](https://webpack.js.org/loaders/), webpack use loaders to preprocess files. 

```
npm install babel-loader --save-dev
```

### Installing Babel/preset-react
This package [@babel/preset-react ](https://babeljs.io/docs/en/babel-preset-react) contains presets with multiple plugins for React. In Babel, a [preset](https://babeljs.io/docs/en/presets/) is a set of plugins that support language features.

```
npm install @babel/preset-react --save-dev
```

### Installing @babel/preset-env
The package [@babel/preset-env](https://babeljs.io/docs/en/babel-preset-env) is a preset that allows us to use the latest JavaScript without needing to micromanage which syntax transforms (and optionally, browser polyfills) are needed by your target environment(s). This both makes your life easier and JavaScript bundles smaller!

```
npm install @babel/preset-env --save-dev
```


### Installing HTMLWebPack plugin
Finally we need to install the package [HTMLWebpackPlugin](https://webpack.js.org/plugins/html-webpack-plugin/). It simplifies creation of HTML files to serve your webpack bundles. This is especially useful for webpack bundles that include a hash in the filename which changes every compilation.

```
npm install html-webpack-plugin --save-dev
```

Now that we have installed all our dependencies we will create our simple React application.

## Create an HTML and JavaScript file.
Create a new source folder `src`

```
mkdir src
cd src
```
Now, let´s create `index.js` and `index.html` file under `src` folder.


```
touch index.hml
```

Add the following code.

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>React Application</title>
</head>
<body>
    <div id="root"></div>
</body>
</html>
```

```
touch index.js
```

Add the following code.

```
import React from "react"
import { createRoot } from "react-dom/client"

const container = document.getElementById("root")
const root = createRoot(container)
root.render(<h1>React EWF Integration!!</h1>)
```

Now we need to configure webpack

#### Add WebPack Configuration file.

We will create a file named `webpack.config.js` to put all our configuration. Creat the file `webpack.config.js` at the root folder of the project.

```
touch webpack.config.js
```

### WebPack Entry Point
First we need to define the [entry point](https://webpack.js.org/concepts/entry-points/). The [entry point](https://webpack.js.org/concepts/#entry) is basically where our application begins.
In our simple example it's the file called `index.js` inside the directory `src`.

We use the built in Node.js [path](https://nodejs.org/api/path.html) module to handle file paths.

```
const path = require('path');

module.exports = {
  entry: path.join(__dirname, "src", "index.js"),
}
```

### Configure WebPack output path for the bundled file
We now need to configure webpack to create a final bundle. We need to provide the name and location where it will be generated. This is done using the [output](https://webpack.js.org/concepts/output/) key.


```
  output: {
    path:path.resolve(__dirname, "dist"),
    filename: 'bundle.js'
  }
```
Here a bundled file named `bundle.js` is generated in a `dist` folder in the root of the project.


### Extending our simple example.

To make this simple project more interesting we will extend our project with a new component App.

Create a new file App.js inside the `src` directory.

```
touch App.js
```

Add the following code.

```
import React from "react"

export default function App(){
    return (
        <h1>React EWF Integration!!</h1>
    )
}
```

Now we need to update our `index.js` file as follow to use our new React component `App`


`index.js`

```
import React from "react"
import { createRoot } from "react-dom/client"
import App from "./App" //(1)

const container = document.getElementById("root")
const root = createRoot(container)
root.render(<App/>) //(2)
```

As you can see we have modified our `index.js` file to use our component App, in the point (1) we import the component App with

`import App from "./App"`


And then when we call the render method in point (2) we call our React component.

`root.render(<App/>)`


### HTML file for Bundled file.
Once the bundled Javascript file `bundled.js` was generated, we want to load it into our HTML file `index.html` using a `script` tag. To do that we will use the `HTMLWebpackPlugin ` that we installed previously. So now we are going to use this [plugin](https://webpack.js.org/concepts/plugins/).

First we need to add the plugin to our webpack configuration file.


```
const HtmlWebpackPlugin = require('html-webpack-plugin');
```

Then we register the HtmlWebpackPlugin as a plugin in our configuration file as follow

```

plugins: [
	new HtmlWebpackPlugin({
      template: path.join(__dirname, "src", "index.html"),
    })
]
```

This will tell webpack to use a file `index.html` from the `src` directory and inject the bundled file `bundled.js` and finally move that HTML file to the output directory `dist`

The generated file `dist\index.html` will look like this

```
<!DOCTYPE html>
<html>
  <head>
  	...
  </head>
  <body>
  	...
    <script src="bundle.js"></script>
  </body>
</html>
```

## Configuring Babel Loader
Now we need to configure webpack to transpile JavaScript files. We use [modules](https://webpack.js.org/concepts/modules) with [rules](https://webpack.js.org/configuration/module/#rule) to let webpack know how the module is created.


```
 module: {
    rules: [
      {
        test: /\.?js$/,
        exclude: /node_modules/,
        use: {
          loader: "babel-loader",
        }
      }
    ]
  }
```

`module`: is an object
`rules`: is an array of objects where each object is a rule.

In this rule, we tell webpack to use `babel-loader` to transpile files that end with `.js` excluding files from the `/node_modules/`.

Now we will extend our rule to use the predefined configurations to transpile different type of Javascript to browsers, using the presets we already installed.
With `@babel/preset-env` transpile ES2015+ syntax and `@babel/preset-react` for transpiling react code



```
 module: {
    rules: [
      {
        test: /\.?js$/,
        exclude: /node_modules/,
        use: {
          loader: "babel-loader",
        	options: {
                    presets: ['@babel/preset-env', '@babel/preset-react']
            }
         }
      }
    ]
  }
```

## Build and Run and Distribute
Finally we need to configure a few scripts to build and run our application in the `package.json`.


```

"scripts": {
    "dev": "webpack-dev-server --mode development --open --hot"
     "build": "webpack --mode production"
}
```
The npm `dev` script will use the `webpack-dev-server` to run a live reload server locally compiling our code in development mode since we are using `--mode development`. It will open the application in a new tab with the `--open` and using hot module replacement with `--hot`

The npm `build` will create a production ready bundle of assets that can be deployed to servers.

### Run the application
```
npm run dev
```

### Build

```
npm run build
```

In the next part of the tutorial we will create an EWF Rest API.


