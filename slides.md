name: inverse
class: center, middle, inverse
layout: true
.topnote[ ***@tanaypratap { twitter, linkedin, github, instagram }*** ]
---

name: center
class: center, middle
layout: true
.topnote[ ***@tanaypratap { twitter, linkedin, github, instagram }*** ]
---

layout: true
.topnote[ ***@tanaypratap { twitter, linkedin, github, instagram }*** ]
---
template: inverse

# javascriptOps
### find slide at osi2019.tanaypratap.com

---
class: inverse

# Agenda

### know your instructor
### getting started with Webpack
### all hands-on coding with exercies
### imporoving your site performance using build tools 
### some more live coding, if time permits

---
class: inverse

# Instructor Intro

### works at Microsoft, in Outlook Web Team.
### teaches coding for free on [learncodingfree.org](https://learncodingfree.org)
### hosts a podcast on tech and startups: [teawithtanay](https://teawithtanay.com)
### [speaks](https://tanaypratap.com/talks) at local meetups and international conferences
### dangerously active on social media 
### find everything on [tanaypratap.com](https://tanaypratap.com)


---
template: inverse

# Why JavaScriptOps?

### story of my time at startup

### and how Microsoft was a change of pace

### story of personal projects

### did I tell you the story my Flipkart friend told me about Big Billion Days?

---
template: inverse

# things needed for this workshop

---
.left-column[
    ## Installations or Accounts needed
]

.right-column[
1. ### [node LTS](https://nodejs.org/en/) v10.16.3
2. ### [git client](https://git-scm.com/downloads) v2.20.1
3. ### [Visual Studio Code](https://code.visualstudio.com/?wt.mc_id=vscom_downloads)
4. ### [Github Account](https://github.com/)
5. ### [Azure Account](https://azure.microsoft.com/en-us/) free for most use cases
]

---

.left-column[
    ## let's get started
]
.right-column[
1. ### Download the repo 
    ```bash
    git clone https://github.com/tanaypratap/osi-workshop.git
    ```
2. ### Step into the directory 
    ```bash
    cd osi-workshop/
    ```
]
---

template: inverse

# git checkout blank-canvas

---

# Let's scaffold a simple Web App

1. Open Visual Studio Code
2. Open Terminal 
3. Create some basic folder
    ```bash 
    mkdir src dist
    ```
4. Create some empty files inside
    ```bash
     cd src 
     touch index.js
     cd ..
     cd dist
     touch index.html
     ```
---

# Edit index.js

```javascript
function component() {
  const element = document.createElement('div');

  // Lodash, currently included via a script, is required for this line to work
  element.innerHTML = _.join(['Hello', 'webpack'], ' ');

  return element;
}

document.body.appendChild(component());
```

---

# Edit index.html

## Remember : Your JS is using lodash which is needed

## Inside index.html

 ```html
 <!doctype html>
<html>
  <head>
    <title>Getting Started</title>
    <script src="https://unpkg.com/lodash@4.16.6"></script>
  </head>
  <body>
    <script src="./src/index.js"></script>
  </body>
</html>
```

---
template: center

**open index.html in browser**

**if everything works, you should see 'hello webpack' in browser**

## exercise 1
### it won't work if you were just copy pasting the code all this while. Find out what's wrong, if you can't, go to next slide for hint

# 😎

---
template: center

# 🧐
# exercise 1 
## hint

### see console for error

---
template: center

# 👻

# exercise 1 
## answer

see line 8 in index.html. it should be:

```html
<script src="../src/index.js"></script>
```

### you can commit your changes now
---

template: inverse

## leaving the exercise fun aside for a moment...
---

template: inverse

## do you see the problem with this?

---
class: inverse

# problems with this approach..

## 1. Global dependency
## 2. Need to maintain dependency order
## 3. Browser downloads dependencies irrespective of usage

---

template: inverse

## let's fix this using webpack?

### we'll see the details of what webpack is after seeing it in action

---

template: inverse

# git checkout enter-webpack

---
template: center

# install Webpack

*at the root of the project.*

Initiate a node project and install *webpack* and *webpack-cli*

```bash
npm init -y
npm install webpack --save-dev
npm install webpack-cli --save-dev
```

---
template: center

# edit package.json

```javascript
+   "private": true,
-   "main": "index.js",
```

---
 
# install lodash 
## and update index.js

1. Install lodash from npm
```bash
npm install --save lodash
```
1. Import lodash in **index.js** at the top
```javascript
import _ from 'lodash';
```

---

# change index.html

Since, we are building the app using webpack now. Our output will be written to **main.js** file.

1. **remove** 
```html
 <script src="../src/index.js"></script>
 ```
1. **add** 
```html
<script src="main.js"></script>
```
1. Also, since we are getting lodash through dependency we can **remove** this
```html
<script src="https://unpkg.com/lodash@4.16.6"></script>
```

---

# let's now run webpack

1. run the command
```bash
npx webpack
```

1. check the *dist* folder, you'll see a **main.js** file

1. run **index.html** file in browser and it should show the same output as before

---


# what just happened?  🤔

1. We stated the *dependencies* the module needs.

1. Webpack used that information to create a *dependency graph*.

1. It then used the graph to generate an *optimized bundle*.

1. This bundle **executes all JS** in correct order.

---
template: inverse

# 🤯

---

# pre-commit check

1. Everything is working

1. Add **node_modules** folder to *.gitignore*
```bash
echo node_modules/ >> .gitignore
```

1. See the next slide before committing
---

# note on committing the bundle

1. The good practice is to never commit a generated file. Especially the bundling output i.e. **main.js** will always have diffs and it is not human readable either.

1. Generate file at **deployment** with a build command

1. We'll see that later in the workshop (if time permits).

1. So, add **main.js** to *.gitgnore* as well
```bash
echo dist/main.js >> .gitignore
```

1. TBH, we should not commit anything in the dist folder. We can't do it for now. Let's go ahead. We'll see the alternative later in workshop (if time permits).

1. Commit everything else for now. 
1. Total 5 files if you're following along.

---
template: center

# 😎
## exercise 2

## Can you find where your index.js code is in the bundle?

---
template: center
# 👻

## exercise 2
## answer

Check the main.js file. 

If you open it in browser's network tab, it will prettify

Go to the end


---
template: inverse

## before we get into more details of how webpack works, 
### we will set up the config first

---

template: inverse

# git checkout configure-webpack

---
template: center

# configure webpack

 Note: As of webpack v4 config is not needed

 However, for complex configs and more control, config file is needed

---

# webpack.config.js

1. Add a file **webpack.config.js** at the root of project
```bash
touch webpack.config.js
```
2. Add this to the file. We'll discuss the details in two minutes.

```javascript
const path = require('path');

module.exports = {
  entry: './src/index.js',
  output: {
    filename: 'main.js',
    path: path.resolve(__dirname, 'dist'),
  },
};
```

---

template: center

## run the build, with the new config file
```bash
npx webpack --config webpack.config.js
```

### Check the index.html again

---

template: inverse

## before we dive into webpack config details
### a little ergonomics

---

template: inverse

# npm scripts

---

# add a build script to package.json

1. Add this to your *package.json* file, make sure you add *,* on top
```javascript
"build": "webpack --config webpack.config.js"
```
1. Now, we don't need to type npx webpack everytime. Just use the below command to run the build
```bash
npm run build
```
Note: This too is a convention. If you're writing a package, start, build, and test are common commands which everyone uses.

---
class: middle

#😎
## exercise 3  

1. Change the config so that the output file is *bundle.js* instead of *main.js*

1. Delete the old *main.js* file

1. It would be good to test the `npm run build` command.

1. See if you're getting the output in browser.

---
class: middle

# 👻
## exercise 3 
## answer

1. Change

```javascript
 output: {
    filename: 'main.js',
```
to

```javascript
 output: {
    filename: 'bundle.js',
```

1. Don't forget to update the file name to *bundle.js* in *index.html* as well

1. Update *.gitignore* as well

---

template: inverse

# git checkout webpack-loader
if you're stuck and need to see the complete code for this part

---

template: inverse

## first part concluded
We have configured a simple JS app to use webpack. However, we need to understand webpack a little more, a little better.

let's do some theory now?

---

template: inverse

# core webpack concepts 🎒

---
class: inverse, center

# core webpack concepts

## let's understand the what and why of webpack

### entry
### output
### loaders
### plugins
### mode

---

# entry

1. Point from where webpack builds it's dependency graph.
1. Like the one we saw just now.

```javascript
module.exports = {
  entry: './src/index.js'
};
```
1. There can be multiple entry points too. For multi-page Apps. 

```javascript
module.exports = {
  entry: {
    pageOne: './src/pageOne/index.js',
    pageTwo: './src/pageTwo/index.js',
    pageThree: './src/pageThree/index.js'
  }
};
```
This is useful when all the apps are having same dependencies. Say, Microsoft Office has mail and calendar app both having a lot of common dependecies.

---

# output

1. Tells webpack how to write the compiled files to use
1. Like the one we saw just now.

```javascript
module.exports = {
  output: {
    filename: 'bundle.js',
  }
};
```

Let's see some advanced use cases next
---

## output: advanced use case 1

### **substitution** in case of multiple entry point app.

```javascript
module.exports = {
  entry: {
    app: './src/app.js',
    search: './src/search.js'
  },
  output: {
    filename: '[name].js',
    path: __dirname + '/dist'
  }
};

// writes to disk: ./dist/app.js, ./dist/search.js
```
---

## output: advanced use case 2

### **publicPath** for CDN and hashes. 

Hashes are used for **cache-busting** which we'll talk about in optimizing performance. 

This is needed if you're using something like *HtmlWebpackPlugin* which we will see in the plugin section soon.
```javascript
module.exports = {
  //...
  output: {
    path: '/home/proj/cdn/assets/[hash]',
    publicPath: 'https://cdn.example.com/assets/[hash]/'
  }
};
```
---

template: inverse

# now let's try a loader
make sure that you're on the webpack-loader branch

---

# raw-loader

It loads a file and provides its contnet

1. Install the loader using npm
```bash
npm install raw-loader --save-dev
```

1. Create a *file.txt* and put some content
```bash
echo "Hi! This is Tanay" >> file.txt
```

---

# raw-loader contd..

1. Update the webpack.config.js file to use this loader

```javascript
 module: {
    rules: [
      {
        test: /\.txt$/i,
        use: 'raw-loader',
      },
    ],
  },
  ```
---

# raw-loader contd..

1. Read this in your **index.js** file

```javascript
import txt from './file.txt';

console.log(txt);
```

1. Run webpack 

1. Check console in browser to see the output

---

template: inverse

# time constraint
## we conclude the hands-on session now

---

template: inverse

# [documentation browsing](https://webpack.js.org) for more

## discussion around advanced features
## how to make your app more performant?

---

template: inverse

# AMA around the workshop

---

template: inverse

# Quiz time
## Raise hand to answer and win swag

---

template: inverse

# live demo for Azure DevOps
## [Check out docs around Azure Pipelines](https://azure.microsoft.com/en-in/services/devops/pipelines/)
## It's free for OSS projects and personal use as well

---

template: inverse

# 🙏
## thank you! 
### i hope you loved it
### feedbacks are appreciated on social media channels
#### find me on [tanaypratap.com](https://tanaypratap.com)

---
template: inverse

left blank intentionally
