# react-app-from-scratch

![image](https://user-images.githubusercontent.com/63926982/185534930-a32a6add-be27-491b-8206-a48857b36fd6.png)

So you’ve been using create-react-app a.k.a CRA for a while now. It’s great and you can get straight to coding. But when do you need to eject from create-react-app and start configuring your own React application? There will be a time when we have to let go of the safety check and start venturing out on our own.

This guide will cover the most simple React configuration that I’ve personally used for almost all of my React projects. By the end of this tutorial we will have our own personal boilerplate and learn some configurations from it.

# Table of Contents

1.Why create your own configuration?

2.Configuring webpack 4

3 .Configuring Babel 7

4.Adding Prettier

5.Adding source map for better error logs

6.Setting up ESLint

7.I found errors! What do I do?

8.Adding CSS LESS processor

9.Deploying React app to Netlify

10.Conclusion

## Why create your own configuration?

There are certain reasons that make creating your own React configuration make sense. You are likely good with React and you want to learn how to use tools like webpack and Babel on your own. These build tools are powerful, and if you have some extra time, it’s always good to learn about them.

Developers are naturally curious people, so if you feel you’d like to know how things work and which part does what, then let me help you with it.

Furthermore, hiding React configuration by create-react-app is meant for developers starting to learn React, as configuration should not stand in the way of getting started. But when things get serious, of course you need more tools to integrate in your project. Think about:

+ Doing server side rendering
+ Using new ES versions
+ Adding MobX and Redux
+ Making your own configuration just for learning sake

If you look around the Internet, there are some hacks to get around CRA limitations like create-react-app rewired.
But really, why not just learn React configuration on your own? I will help you get there. Step by step.

Now that you’re convinced to learn some configuration, let’s start by initializing a React project from scratch.

Open up the command line or Git bash and create a new directory

``mkdir react-config-tutorial && cd react-config-tutorial``

Initialize NPM project by running:

```npm init -y```

Now install react

`npm install react react-dom`

## Configuring webpack 4

Our first stop will be the webpack. It’s a very popular and powerful tool for configuring not only React, but almost all front-end projects. The core function of webpack is that it takes a bunch of JavaScript files we write in our project and turns them into a single, minified file, so that it will be quick to serve. Starting from webpack 4, we aren’t required to write a configuration file at all to use it, but in this tutorial we will write one so that we can understand it better.

First, let’s do some installation

`npm install --save-dev webpack webpack-dev-server webpack-cli`

This will install:

  + webpack module 
 — which include all core webpack functionality
  + webpack-dev-server 
 — this development server automatically rerun webpack when our file is changed
  + webpack-cli 
 — enable running webpack from the command line
 
 Let’s try to run webpack by adding the following script to `package.json`
 
```
"scripts": {

 "start": "webpack-dev-server --mode development",
 
},

```

Now create an `index.html` file in your root project with the following content:

```
<!DOCTYPE html>
<html>
 <head>
 <title>My React Configuration Setup</title>
 </head>
 <body>
 <div id="root"></div>
 <script src="./dist/bundle.js"></script>
 </body>
</html>
```

Create a new directory named src and inside it, create a new `index.js` file

```
mkdir src && cd src && touch index.js
```


### Then write a React component into the file:

```
import React from "react";
import ReactDOM from "react-dom";
class Welcome extends React.Component {
  render() {
    return <h1>Hello World from React boilerplate</h1>;
  }
}
ReactDOM.render(<Welcome />, document.getElementById("root"));

```
Run the webpack by using `npm run start` … And an error will be triggered.

```
You may need an appropriate loader to handle this file type
```

## Configuring Babel 7

The React component we wrote above used the `class` syntax, which is a feature of ES6. Webpack needs Babel to process ES6 into ES5 syntaxes in order for this class to work.

Let’s install Babel into our project

```
npm install --save-dev @babel/core @babel/preset-env \@babel/preset-react babel-loader
```

## Why do we need these packages?

+ **@babel/core** is the main dependency that includes babel transform script.
+ **@babel/preset-env** is the default Babel preset used to transform ES6+ into valid ES5 code. Optionally configures browser polyfills automatically.
+ **@babel/preset-react** is used for transforming JSX and React class syntax into valid JavaScript code.
+ **babel-loader** is a webpack loader that hooks Babel into webpack. We will run Babel from webpack with this package.

To hook Babel into our webpack, we need to create a webpack configuration file. Let’s write a `webpack.config.js` file:

```

module.exports = {
  entry: './src/index.js',
  output: {
    path: __dirname + '/dist',
    publicPath: '/',
    filename: 'bundle.js'
  },
  devServer: {
    contentBase: './dist',
  },
  module: {
    rules: [
    {
      test: /\.(js|jsx)$/,
      exclude: /node_modules/,
      use: ['babel-loader']
    }
    ]
  },
};

```

This webpack config is basically saying that the `entry` point of our application is from `index.js`, so pull everything that’s needed by that file, then put the `output` of the bundling process into the dist directory, named `bundle.js.` Oh, if we’re running on `webpack-dev-server`, then tell the server to serve content from `contentBase` config, which is the same directory this config is in. For all .js or .jsx files, use `babel-loader` to transpile all of them.

In order to use Babel presets, create a new `.babelrc` file

```
touch .babelrc
```

Write the following content:

```

{
  "presets": [
    "@babel/preset-env",
    "@babel/preset-react"
  ]
}

```

Now run `npm run start` again. This time it will work.

## Adding Prettier

To further speed up development, let’s make our code formatter using Prettier. Install the dependency locally and use the — save-exact argument since Prettier introduces stylistic changes in patch releases.


```
npm install --save-dev --save-exact prettier

```

Now we need to write the `.prettierrc` configuration file:

```

{
 "semi": true,
 "singleQuote": true,
 "trailingComma": "es5"
}

```

The rules means that we want to add semicolon for the end of every statement, use a single quote whenever appropriate and put trailing commas for multi-line ES5 code like objects or arrays.

You can run Prettier from the command line with:

```
npx prettier --write "src/**/*.js"

```


Or add a new script to our `package.json` file:


```

"scripts": {
 "test": "echo \"Error: no test specified\" && exit 1",
 "start": "webpack-dev-server --mode development",
 "format": "prettier --write \"src/**/*.js\""
},

```

Now we can run Prettier using `npm run format`.

Now we can run Prettier using npm run format.

```
"editor.formatOnSave": true

```

## Adding source map for better error logs


Since webpack bundles the code, source maps are mandatory to get a reference to the original file that raised an error. For example, if you bundle three source files (`a.js`, `b.js`, and `c.js`) into one bundle (`bundler.js`) and one of the source files contains an error, the stack trace will simply point to `bundle.js`. This is problematic as you probably want to know exactly if it’s the a, b, or c file that is causing an error.

You can tell webpack to generate source maps using the devtool property of the configuration:
```
module.exports = {
  devtool: 'inline-source-map',
  // … the rest of the config
};

```

Although it will cause a slower build, it has no effect on production. 
Sourcemaps are only downloaded if you open the browser DevTools.

## Setting up ESLint

Linter is a program that checks our code for any error or warning that can cause bugs. JavaScript’s linter, ESLint, is a very flexible linting program that can be configured in many ways.

But before we get ahead, let’s install ESLint into our project:

```

npm --save-dev install eslint eslint-loader babel-eslint eslint-config-react eslint-plugin-react

```

`eslint` is the core dependency for all functionalities, while `eslint-loader` enables us to hook eslint into webpack. Now since React used ES6+ syntax, we will add `babel-eslint` — a parser that enables eslint to lint all valid ES6+ codes.
`eslint-config-react` and `eslint-plugin-react` are both used to enable **ESLint** to use pre-made rules.
