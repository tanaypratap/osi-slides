name: inverse
class: center, middle, inverse
layout: true
---

name: center
class: center, middle
layout: true

---

# javascriptOps

---
layout: false

# Agenda

TBD

---
layout: false

# Who's this guy?

TBD

---
template: inverse

# Introduction

---
layout: false

# Why JavaScriptOps?

TBD

---
template: inverse

# Things needed for this workshop

---
layout: false

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
layout: false

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

# config.json

1. Add a file **config.json** at the root of project
```bash
touch config.json
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

# NPM scripts

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

template: inverse

## first part concluded
We have configured a simple JS app to use webpack. However, we need to understand webpack a little more, a little better.

let's do some theory now?

---

template: inverse

# core webpack concepts 🎒

---

template: inverse

# git checkout webpack-loader
