# Learning React.js
- Eve Porcello
- Started 11-16-2017

## Welcome
- Created at Facebook
- Building 2 applications: an activity counter and simple website
- We'll learn setting up a react project, core features, organizing a React project

## Setting up Chrome tools for React
- Install React developer tools on Chrome as an extension
- Also available on Firefox as an addon

## Inspecting React sites
- Airbnb uses React
- We'll look more at the Virtual DOM

# 1. What Is React.js?
## What is React?
- Used to build user interfaces
- We make reusable components that can change over time
- Facebook released in 2013
- React Native lets you create mobile applications
- Use the React docs as a supplement to this course

## Setting up Chrome tools for React
- We are going to install some more tools as chrome extensions
- Install react-detector react developer tools

## Inspecting React sites
- We visit hellosign.com
- For React tools, hit CTRL + SHIFT + J
- Tools shows props and states

## Efficient rendering with React
- DOM is the structure of HTML elements that make up a webpage
- DOM Diffing compares current rendered content to new UI contents about to be made
- It makes only minimal changes necessary
- It's faster than reading and writing to DOM directly

# 2. Intro to JSX and Babel
## Pure React
- We made a new index.js file in the 02_01/start/dist folder
```js
const title = React.createElement(
  'h1',
  {id: 'title', className: 'header'},
  'Hello World'
)

ReactDOM.render(
  title,
  document.getElementById('react-container')
)
```

- We linked it in the html:
- There's already a script for react, reactdom, and index.js
```html
  <div id='react-container'></div>
  <script src="index.js"></script>
```

### Installing a server to serve our site
- We downloaded httpstr from npm
`sudo npm install -g httpster`
- Navigate to appropriate directory
- Serve up the "dist" directory with
`httpster -d ./dist -p 3000`

### Simplifying code with destructuring
- Use ES6 destructuring to shorten your code
```js
const { createElement } = React
const { render } = ReactDOM

const title = createElement(
  'h1',
  {id: 'title', className: 'header'},
  'Hello World'
)

render(
  title,
  document.getElementById('react-container')
)
```

## Refactoring elements using JSX
- We are going to style with some CSS
```js
const { createElement } = React
const { render } = ReactDOM

const style = {
  backgroundColor: 'orange',
  color: 'white',
  fontFamily: 'verdana'
}

const title = createElement(
  'h1',
  {id: 'title', className: 'header', style: style},
  'Hello World'
)

render(
  title,
  document.getElementById('react-container')
)
```

### Converting to JSX
- We no longer use createElement
- JSX expressions use `{}`
- Notice we get rid the createElement method
- Got rid of the title variable
```js
const { render } = ReactDOM

const style = {
  backgroundColor: 'orange',
  color: 'white',
  fontFamily: 'verdana'
}

render(
  <h1 id='title'
    className='header'
    style={style}>
  Hello World
  </h1>,
  document.getElementById('react-container')
)
```

- Now we are going to nest the style object in the h1
- Notice that there's 2 sets of curly brackets
- We are passing a style object in a JSX expression
- Then you can rid of the style variable also
```js
const { render } = ReactDOM

render(
  <h1 id='title'
    className='header'
    style={{backgroundColor: 'orange', color: 'white', fontFamily: 'verdana'}}>
  Hello World
  </h1>,
  document.getElementById('react-container')
)
```
- **We have a error with the `<h1>` because it needs babel to transpile**
- See next section

## Babel inline transpiling
- It takes code unreadable by the browser and converts it to legacy code readable by all browser
- You can use the "Try it out" tab to see the code
- Supports ES2015, ES7, and so on
- Eve uses [babel-core 5.8.38](https://cdnjs.com/libraries/babel-core)
- She copies the CDN underneath the react scripts in index.html
- You also have to add the type to the script at the bottom
```js
<script type="text/babel" src="index.js"></script>
```

## Babel static transpiling with babel-cli
- Static transpiling happens offline before it makes it to the browser
- There's no processing as it builds such as with CDNs
- CDN is good for testing but not for production
- I think it only matters with big projects
- Steps:
1. Delete the babel script tag in index.html
2. Change script type as `text="text/javascript"`
3. Change src to `bundle.js`

### Generate project
- Run in terminal:
```js
npm init

// Give it a name, description, main as "index.js", author, license
```

### Manipulating folder structure and installing babel
1. Install babel:
```js
npm install babel-cli@6.18.0 --save-dev // Install for project
sudo npm install -g babel-cli // Install globably
```

2. dist folder is for the browser, contains: index.html
3. src folder is for production, contains:index.js
4. create `.babelrc` and set presets
```js
{
  "presets": ["latest", "react", "stage-0"]
}
```
4. Install babel presets with npm
```js
npm install babel-preset-react@6.16.0 --save-dev
npm install babel-preset-latest@6.16.0 --save-dev
npm install babel-preset-stage-0@6.16.0 --save-dev
```
5. Run babel command
- Babel will transpile to the bundle.js 
```js
babel ./src/index.js --out-file ./dist/bundle.js
```
6. Adjust httpster script in package.json file
```js
  "scripts": {
    "start": "httpster -d ./dist -p 3000"
  },
```
7. Run npm start and browse to `localhost:3000`

## Building with webpack
- Webpack is a module bundler that creates static files before it goes into production
- It helps automate the processes
- A `bundle.js` file is created so it is only a single HTTP request

### Installing webpack
1. Create `webpack.config.js` file in the root project folder
- Loaders help perform tasks and there are many
- devServer supports hot-loading
- Add:
```js
var ebpack = require("webpack");
module.exports = {
  entry: "./src/index.js",
  output: {
    path: "dist/assets",
    filename: "bundle.js",
    publicPath: "assets"
  },
  devServer: {
    inline: true,
    contentBase: "./dist",
    port: 3000
  },
  module: {
    loaders: [
      {
        test: /\.js$/,
        exlude: /(node_modules)/,
        loader: ["babel-loader"],
        query:  {
          presets: ["latest", "stage-0", "react"]
        }
      },
    ]
  }
}
```
2. Install webpack
```js
npm install webpack@1.13.3 --save-dev
npm install babel-loader@6.2.5 --save-dev
npm install webpack-dev-server@1.16.2 --save-dev
```
3. Run webpack
- It might not run so install webpack globally
```js
sudo npm install -g webpack@1.13.3

// An alternate way is just to run it from the bin
./node_modules/.bin/webpack
```
4. A bundle.js will be created in ./dist/assets/
5. Change script tag in html to reflect `src="/assets/bundle.js"`
6. Change the script in package.json
```js
  "scripts": {
    "start": "./node_modules/.bin/webpack-dev-server"
  },
```

## Loading JSON with webpack
- We loading react dependencies and json 
- We are importing files as modules
1. Install React and ReactDOM dependencies
```js
npm install react@15.3.2 --save-dev
npm install react-dom@15.3.2 --save-dev
```

2. Create `lib.js` in src folder
```js
import React from 'react'
import text from './titles.json'

export const hello = (
  <h1 id='title'
      className='header'
      style={{backgroundColor: 'purple', color: 'yellow'}}>
    {text.hello}
  </h1>
)

export const goodbye = (
  <h1 id='title'
      className='header'
      style={{backgroundColor: 'purple', color: 'yellow'}}>
    {text.goodbye}
  </h1>
)
```

3. Create `title.json` in the src folder
- Add JSON object code:
```js
{
  "hello": "Bonjour!",
  "goodbye": "Au Revoir"
}
```

4. Create a new render in `index.js`
```js
import React from 'react'
import { render } from 'react-dom'
import { hello, goodbye } from './lib'
render(
  <div>
    {hello}
    {goodbye}
  </div>
  document.getElementById('react-container')
)
```

5. Add another loader in `webpack.config.js` file
```js
module: {
    loaders: [
      {
        test: /\.js$/,
        exlude: /(node_modules)/,
        loader: ["babel-loader"],
        query:  {
          presets: ["latest", "stage-0", "react"]
        }
      },
      {
        test: /\.json$/,
        exlude: /(node_modules)/
        loader: "json-loader"
      }
    ]
  }
```

6. Install json-loader in npm
```js
npm install json-loader@0.5.4 --save-dev
```

7. Run npm start

## Adding CSS to webpack build
- We are going to remove our css and bundle that
1. Remove style attribute and add style className in lib.js
```js
import React from 'react'
import text from './titles.json'

export const hello = (
  <h1 id="title"
      className="hello">
      {text.hello}
  </h1>
)

export const goodbye = (
  <h1 id="title"
      className="goodbye">
      {text.goodbye}
</h1>
)
```

2. Create stylesheets folder in src folder
3. Create `hello.css` and `goodbye.scss` in stylesheets color
- Add codes:
```js
// hello.css
.hello {
  background-color: indigo;
  color: turquoise;
}

// goodbye.scss
$bg-color: turquoise;
$text-color: indigo;

.goodbye {
  background-color: $bg-color;
  color: $text-color;
}
```

4. Adding additional loaders to `webpack.config.js`
```js
  module: {
    loaders: [
      {
        test: /\.js$/,
        exclude: /(node_modules)/,
        loader: ["babel-loader"],
        query: {
          presets: ["latest", "stage-0", "react"]
        }
      },
      {
        test: /\.json$/,
        exclude: /(node_modules)/,
        loader: "json-loader"
      },
      {
        test: /\.css$/,
        loader: "style-loader!css-loader!autoprefixer-loader"
      },
      {
        test: /\.scss$/,
        loader: "style-loader!css-loader!autoprefixer-loader!sass-loader"
      }
    ]
  }
```

5. Install loaders via npm
```js
npm install autoprefixer-loader@3.2.0 --save-dev
npm install css-loader@0.25.0 --save-dev
npm install sass-loader@4.0.2 --save-dev
npm install node-sass@3.10.1 --save-dev
npm install style-loader@0.13.1 --save-dev
```

6. Import the css and scss file
```js
import React from 'react'
import text from './titles.json'
import './stylesheets/goodbye.scss'
import './stylesheets/hello.css'

export const hello = (
  <h1 id="title"
      className="hello">
      {text.hello}
  </h1>
)

export const goodbye = (
  <h1 id="title"
      className="goodbye">
      {text.goodbye}
</h1>
)
```

7. Start server with npm start

## Migrating to webpack 3
- Webpack 1 is different from Webpack 2 and 3
- Eve started a new project to show the different
1. Navigate to new project and initiate
- The `-y` flag sets default parameters
```js
npm init -y
```

2. Install webpack at version 3, babel dependencies, react
- Instead of preset latest, stage-0, you can now use babel-preset-env
```js
npm install webpack@3.1.0 --save-dev
npm install babel-core@6.25.0 --save-dev
npm install babel-loader@7.1.1 --save-dev
npm install babel-preset-env@1.6.0 --save-dev
npm install babel-preset-react@6.24.1 --save-dev
```

3. Do webpack configurations
- Remove path and public paths option
- Remove devServer since we didn't configure yet for this project
- Change `loaders` to `rules`
- Change babel loaders
  - Change brackets to quotes for the loader
  - Change latest and stage-0 to env
  - Get rid of loaders for .json, .css, and .scss (we are not using these)
```js
var webpack = require("webpack");

module.exports = {
  entry: "./src/index.js",
  output: {
    // path: "dist/assets",
    filename: "bundle.js"
    // publicPath: "assets"
  },
  // devServer: {
  //   inline: true,
  //   contentBase: "./dist",
  //   port: 3000
  // },
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /(node_modules)/,
        loader: "babel-loader", // Took out brackets
        query: {
          presets: ["env", "react"] // Removed "latest", "stage-0"
        }
      }
    ]
  }
}
```

4. Make new src folder and make `index.js` file
- Add code:
```js
const myName = "Eve";
```

5. Make a .babalrc file in the root 
- Add code:
```js
{
  "presets": ["env", "react"]
}
```

6. Run webpack
- If webpack not found, run `./node_modules/.bin/webpack`
- But you can add this to script in package.json
```js
{
  "name": "react-essential",
  "version": "1.0.0",
  "description": "A project focusing on React and related tools",
  "main": "index.js",
  "scripts": {
    "build": "./node_modules/.bin/webpack"
  },
  "author": "Eve Porcello",
  "license": "MIT",
  "devDependencies": {
    "autoprefixer-loader": "^3.2.0",
    "babel-cli": "^6.18.0",
    "babel-loader": "^6.2.5",
    "babel-preset-latest": "^6.16.0",
    "babel-preset-react": "^6.16.0",
    "babel-preset-stage-0": "^6.16.0",
    "css-loader": "^0.25.0",
    "json-loader": "^0.5.4",
    "node-sass": "^3.4.2",
    "sass-loader": "^4.0.2",
    "style-loader": "^0.13.1",
    "webpack": "^1.13.3",
    "webpack-dev-server": "^1.16.2"
  },
  "dependencies": {
    "react": "^15.3.2",
    "react-dom": "^15.3.2"
  }
}
```