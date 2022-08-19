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

+ # webpack module — which include all core webpack functionality
+ # webpack-dev-server — this development server automatically rerun webpack when our file is changed
+ # webpack-cli — enable running webpack from the command line
