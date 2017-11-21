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

# 3. React Components
## Planning an ActivityCounter
- Making web app that counts the number of times an activity is done
- Can be a variety of values but project requires:
  - location
  - date
  - boolean
  - boolean

## Creating components with createClass()
- Added stylesheets folder in ./src also
- Added SkiDayCount.js in ./src/components
- Add export to the const as well
- Add code:
```js
// SkiDayCount.js
import React from 'react'
import '../stylesheets/ui.scss'

export const SkiDayCount = React.createClass({
  render() {
    return (
      <div className="ski-day-count">
        <div className="total-days">
          <span>5 days</span>
        </div>
        <div className="powder-days">
          <span>2 days</span>
        </div>
        <div className="backcountry-days">
          <span>1 hiking day</span>
        </div>
      </div>
    )
  }
}
```

- In our ./src/index.js file, add:
- Adding the window.React line helps prevent some errors
```js
import React from 'react'
import { render } from 'react-dom'
import { SkiDayCount } from './components/SkiDayCount'

window.React = React

render(
  <SkiDayCount />,
  document.getElementById('react-container')
)
```

- `React.createClass` not recommended anymore
- ES6 classes and stateless functional components are preferred now

## Adding component properties
- We are going to use properties to display data
- Think of properties as objects and each property as a key
- We are adding 4 components
```js
// index.js
import React from 'react'
import { render } from 'react-dom'
import { SkiDayCount } from './components/SkiDayCount'

window.React = React

render(
  <SkiDayCount total={50}
               powder={20}
               backcountry={10}
               goal={100} />,
  document.getElementById('react-container')
)
```
```js
// SkiDayCount.js
import React from 'react'
import '../stylesheets/ui.scss'

export const SkiDayCount = React.createClass({
  render() {
    return (
      <div className="ski-day-count">
        <div className="total-days">
          <span>{this.props.total}</span>
          <span>days</span>
        </div>
        <div className="powder-days">
          <span>{this.props.powder}</span>
          <span>days</span>
        </div>
        <div className="backcountry-days">
          <span>{this.props.backcountry}</span>
          <span>day</span>
        </div>
        <div>
          <span>{this.props.goal}</span>
        </div>
      </div>
    )
  }
}
```

## Adding component methods
- We can also add methods 
- This one calculates a percent and returns it
```js
// SkiDayCount.js
import React from 'react'
import '../stylesheets/ui.scss'

export const SkiDayCount = React.createClass({
  percentToDecimal(decimal) {
    return ((decimal *100) + '%')
  },
  calcGoalProgress(total, goal) {
    return this.percentToDecimal(total/goal)
  },
  render() {
    return (
      <div className="ski-day-count">
        <div className="total-days">
          <span>{this.props.total}</span>
          <span>days</span>
        </div>
        <div className="powder-days">
          <span>{this.props.powder}</span>
          <span>days</span>
        </div>
        <div className="backcountry-days">
          <span>{this.props.powder}</span>
          <span>day</span>
        </div>
        <div>
          <span>
            {this.calcGoalProgress(
              this.props.total,
              this.props.goal
            )}
          </span>
        </div>
      </div>
    )
  }
}
```

## Creating components with ES6 class syntax
- ES6 was released in 2015
- React has a base class called React.component
- We extend this class to create our own component
- We refactor our component:
- You do not need parentheses or commos 
```js
// SkiDayCount.js
import React from 'react'
import '../stylesheets/ui.scss'

export class SkiDayCount extends React.Component { // class is rewritten, parens removed
  percentToDecimal(decimal) {
    return ((decimal *100) + '%')
  } // Comma was removed
  calcGoalProgress(total, goal) {
    return this.percentToDecimal(total/goal)
  } // Comma was removed
  render() {
    return (
      <div className="ski-day-count">
        <div className="total-days">
          <span>{this.props.total}</span>
          <span>days</span>
        </div>
        <div className="powder-days">
          <span>{this.props.powder}</span>
          <span>days</span>
        </div>
        <div className="backcountry-days">
          <span>{this.props.backcountry}</span>
          <span>day</span>
        </div>
        <div>
          <span>
            {this.calcGoalProgress(
              this.props.total,
              this.props.goal
            )}
          </span>
        </div>
      </div>
    )
  }
}
```

- You can also use specific components if you choose
- So we can be specific in our import like:
```js
import { Component } from 'react'

export class SkiDayCount extends Component {}
```

## Creating stateless functional components
- We also have stateless functions along with React.createClass and ES6 classes
- Eve mentions that stateless functions might have a performance benefit but we not positive
- Recommends we use them whenever possible
- Functions:
1. Take in properties information and renders as JSX elements
```
const MyComponent = (props) => (
  <div>{props.tile}</div>
)
```
2. Can't access `this` inside
3. Local functions need to be declared elsewhere in component
- Now we refactor our code:
  1. Change to an arrow function and add props parameter
  2. Pull local methods and define it as own constant equal to arrow function
  3. Remove `this` keyword
  4. Remove `render` and `return`

```js
// SkiDayCount.js
import React from 'react'
import '../stylesheets/ui.scss'

const percentToDecimal = (decimal) => {
  return ((decimal *100) + '%')
}

const calcGoalProgress = (total, goal) => {
  return percentToDecimal(total/goal)
} 

export const SkiDayCount = (props) => (
  <div className="ski-day-count">
    <div className="total-days">
      <span>{props.total}</span>
      <span>days</span>
    </div>
    <div className="powder-days">
      <span>{props.powder}</span>
      <span>days</span>
    </div>
    <div className="backcountry-days">
      <span>{props.backcountry}</span>
      <span>day</span>
    </div>
    <div>
      <span>
        {calcGoalProgress(
          props.total,
          props.goal
        )}
      </span>
    </div>
  </div>
)
```

- Further refactoring by destructuring parameters:
1. Destructure parameters
2. Remove `props` from the JSX expressions
3. Remove `import React from 'react'` since we are no longer using it
```js
// SkiDayCount.js
import '../stylesheets/ui.scss'

const percentToDecimal = (decimal) => {
  return ((decimal *100) + '%')
}

const calcGoalProgress = (total, goal) => {
  return percentToDecimal(total/goal)
} 

export const SkiDayCount = ({total, powder, backcountry, goal}) => (
  <div className="ski-day-count">
    <div className="total-days">
      <span>{total}</span>
      <span>days</span>
    </div>
    <div className="powder-days">
      <span>{powder}</span>
      <span>days</span>
    </div>
    <div className="backcountry-days">
      <span>{backcountry}</span>
      <span>day</span>
    </div>
    <div>
      <span>
        {calcGoalProgress(
          total,
          goal
        )}
      </span>
    </div>
  </div>
)
```

## Adding react-icons
- React is a library, not a framework
- Libraries are intentionally small
- If you need something, you can import it
- One community is [React Icons](https://github.com/gorangajic/react-icons)

1. Install react-icons
```js
npm install --save react-icons
```
2. Import in our app
- We just want to import the icons that we need
```js
// SkiDayCount.js
import '../stylesheets/ui.scss'
import Terrain from 'react-icons/lib/md/terrain'
import SnowFlake from 'react-icons/lib/ti/weather-snow'
import Calendar from 'react-icons/lib/fa/calendar'

const percentToDecimal = (decimal) => {
  return ((decimal * 100) + '%')
}

const calcGoalProgress = (total, goal) => {
  return percentToDecimal(total/goal)
} 

export const SkiDayCount = ({total, powder, backcountry, goal}) => (
  <div className="ski-day-count">
    <div className="total-days">
      <span>{total}</span>
        <Calendar />
      <span>days</span>
    </div>
    <div className="powder-days">
      <span>{powder}</span>
        <Snowflake />
      <span>days</span>
    </div>
    <div className="backcountry-days">
      <span>{backcountry}</span>
        <Terrain />
      <span>day</span>
    </div>
    <div>
      <span>
        {calcGoalProgress(
          total,
          goal
        )}
      </span>
    </div>
  </div>
)
```

# 4. Props and State
## Composing components
- We are making a table with react components
- We changed our props and import
```js
// index.js
import React from 'react'
import { render } from 'react-dom'
import { SkiDayList } from './components/SkiDayList'

window.React = React
render(
	<SkiDayList days={
    [
      {
        resort: "Squaw Valley",
        date: new Date("1/2/2016"),
        powder: true,
        backcountry: false
      },
      {
        resort: "Kirkwood",
        date: new Date("3/28/2016"),
        powder: false,
        backcountry: false
      },
      {
        resort: "Mt. Tallac",
        date: new Date("4/2/2016"),
        powder: false,
        backcountry: true
      }
    ]
  }/>,
	document.getElementById('react-container')
)
```

## Displaying child components
- Created 2 files in `./src/components`: `SkidDayList.js` and `SkiDayRow.js`
```js
// SkiDayRow.js
import Terrain from 'react-icons/lib/md/terrain'
import SnowFlake from 'react-icons/lib/ti/weather-snow'
import Calendar from 'react-icons/lib/fa/calendar'

export const SkiDayRow = ({resort, date, powder, backcountry}) => (
  <tr>
    <td>
      {date.getMonth()+1}/{date.getDate()}/{date.getFullYear()}
    </td>
    <td>
      {resort}
    </td>
    <td>
      {(powder) ? <SnowFlake /> : null}
    </td>
    <td>
      {(backcountry) ? <Terrain /> : null}
    </td>
  </tr>
)
```

```js
// SkiDayList.js
import Terrain from 'react-icons/lib/md/terrain'
import SnowFlake from 'react-icons/lib/ti/weather-snow'
import Calendar from 'react-icons/lib/fa/calendar'
import { SkiDayRow } from './SkiDayRow'

export const SkiDayList = ({days}) => (
  <table>
    <thead>
      <tr>
        <th>Date</th>
        <th>Resort</th>
        <th>Powder</th>
        <th>Backcountry</th>
      </tr>
    </thead>
    <tbody>
      {days.map((day,i) =>
        <SkiDayRow key={i}
                   resort={day.resort}
                   date={day.date}
                   powder={day.powder}
                   backcountry={day.backcountry}/>
      )}
    </tbody>
  </table>
)
```

### Refactor to SkiDayList.js with spread operator
```js
// SkiDayList.js
    <tbody>
      {days.map((day,i) =>
        <SkiDayRow key={i}
                   {...day}/>
      )}
    </tbody>
```

## Default props
- Default values helps provide a value that doesn't have values defined
- She shows us 3 days to do it with createClass, ES6, and stateless ways

### Default porps withcreateClass method
```js
import { createClass } from 'react'
import '../stylesheets/ui.scss'
import Terrain from 'react-icons/lib/md/terrain'
import SnowFlake from 'react-icons/lib/ti/weather-snow'
import Calendar from 'react-icons/lib/fa/calendar'

export const SkiDayCount = createClass({
  getDefaultProps() {
    return {
      total: 50,
      powder: 50,
      backcountry: 15,
      goal: 100
    }
  },
  percentToDecimal(decimal) {
    return ((decimal * 100) + '%')
  },
  calcGoalProgress(total, goal) {
    return this.percentToDecimal(total/goal)
  },
  render() {
    return (
      <div className="ski-day-count">
        <div className="total-days">
          <span>{this.props.total}</span>
            <Calendar />
          <span>days</span>
        </div>
        <div className="powder-days">
          <span>{this.props.powder}</span>
            <SnowFlake />
          <span>days</span>
        </div>
        <div className="backcountry-days">
          <span>{this.props.backcountry}</span>
            <Terrain />
          <span>days</span>
        </div>
        <div>
          <span>
            {this.calcGoalProgress(
              this.props.total, 
              this.props.goal
            )}
          </span>
        </div>
      </div>
    )
  }
})

// index.js
// Don't forget to import
import { SkiDayCount } from './components/SkiDayCount-createClass'
```

### Default props with ES6 method
```js
import { Component } from 'react'
import '../stylesheets/ui.scss'
import Terrain from 'react-icons/lib/md/terrain'
import SnowFlake from 'react-icons/lib/ti/weather-snow'
import Calendar from 'react-icons/lib/fa/calendar'

export class SkiDayCount extends Component {
  percentToDecimal(decimal) {
    return ((decimal * 100) + '%')
  }
  calcGoalProgress(total, goal) {
    return this.percentToDecimal(total/goal)
  }
  render() {
    return (
      <div className="ski-day-count">
        <div className="total-days">
          <span>{this.props.total}</span>
            <Calendar />
          <span>days</span>
        </div>
        <div className="powder-days">
          <span>{this.props.powder}</span>
            <SnowFlake />
          <span>days</span>
        </div>
        <div className="backcountry-days">
          <span>{this.props.backcountry}</span>
            <Terrain />
          <span>days</span>
        </div>
        <div>
          <span>
            {this.calcGoalProgress(
              this.props.total, 
              this.props.goal
            )}
          </span>
        </div>
      </div>
    )
  }
}

SkiDayCount.defaultProps = { 
  total: 50,
  powder: 10,
  backcountry: 15,
  goal: 75
}

// index.js
// Don't forget to import
import { SkiDayCount } from './components/SkiDayCount-ES6'
```

### Default props with Stateless method
- Uses the same code as ES6 method at the very bottom
```js
// SkiDayCount.js
SkiDayCount.defaultProps = { 
  total: 50,
  powder: 10,
  backcountry: 15,
  goal: 75
}

// index.js
// Don't forget to import
import { SkiDayCount } from './components/SkiDayCount'
```

### Setting defaults directly in the function
```js
import '../stylesheets/ui.scss'
import Terrain from 'react-icons/lib/md/terrain'
import SnowFlake from 'react-icons/lib/ti/weather-snow'
import Calendar from 'react-icons/lib/fa/calendar'

const percentToDecimal = (decimal) => {
	return ((decimal * 100) + '%')
}

const calcGoalProgress = (total, goal) => {
	return percentToDecimal(total/goal)
}

export const SkiDayCount = ({total=70, powder=20, 
							backcountry=10, goal=100}) => (
		<div className="ski-day-count">
			<div className="total-days">
				<span>{total}</span>
					<Calendar />
				<span>days</span>
			</div>
			<div className="powder-days">
				<span>{powder}</span>
					<SnowFlake />
				<span>days</span>
			</div>
			<div className="backcountry-days">
				<span>{backcountry}</span>
					<Terrain />
				<span>days</span>
			</div>
			<div>
				<span>
					{calcGoalProgress(
						total, 
						goal
					)}
				</span>
			</div>
		</div>
)
```

## Validating with React.PropTypes
- This helps validates that the value is a certain type of primitive
- Again, we will show createClass, ES6, and stateless ways
- Don't forget to destructure `Proptypes` from react
- So now if you enter an incorret primitive, it will still render
- However, will give warning in console
- `isRequired` gives you a console warning if value is not supplied

### PropTypes with createClass method
```js
import { createClass, PropTypes } from 'react'
import '../stylesheets/ui.scss'
import Terrain from 'react-icons/lib/md/terrain'
import SnowFlake from 'react-icons/lib/ti/weather-snow'
import Calendar from 'react-icons/lib/fa/calendar'

export const SkiDayCount = createClass({
  propTypes: {
    total: PropTypes.number,
    powder: PropTypes.number,
    backcountry: PropTypes.number
  }
  getDefaultProps() {
    return {
      total: 50,
      powder: 50,
      backcountry: 15,
      goal: 100
    }
  },
  percentToDecimal(decimal) {
    return ((decimal * 100) + '%')
  },
  calcGoalProgress(total, goal) {
    return this.percentToDecimal(total/goal)
  },
  render() {
    return (
      <div className="ski-day-count">
        <div className="total-days">
          <span>{this.props.total}</span>
            <Calendar />
          <span>days</span>
        </div>
        <div className="powder-days">
          <span>{this.props.powder}</span>
            <SnowFlake />
          <span>days</span>
        </div>
        <div className="backcountry-days">
          <span>{this.props.backcountry}</span>
            <Terrain />
          <span>days</span>
        </div>
        <div>
          <span>
            {this.calcGoalProgress(
              this.props.total, 
              this.props.goal
            )}
          </span>
        </div>
      </div>
    )
  }
})

// index.js
// Don't forget to import
import { SkiDayCount } from './components/SkiDayCount-createClass'
```

### PropTypes with ES6 method
```js
import { Component, PropTypes } from 'react'
import '../stylesheets/ui.scss'
import Terrain from 'react-icons/lib/md/terrain'
import SnowFlake from 'react-icons/lib/ti/weather-snow'
import Calendar from 'react-icons/lib/fa/calendar'

export class SkiDayCount extends Component {
  percentToDecimal(decimal) {
    return ((decimal * 100) + '%')
  }
  calcGoalProgress(total, goal) {
    return this.percentToDecimal(total/goal)
  }
  render() {
    return (
      <div className="ski-day-count">
        <div className="total-days">
          <span>{this.props.total}</span>
            <Calendar />
          <span>days</span>
        </div>
        <div className="powder-days">
          <span>{this.props.powder}</span>
            <SnowFlake />
          <span>days</span>
        </div>
        <div className="backcountry-days">
          <span>{this.props.backcountry}</span>
            <Terrain />
          <span>days</span>
        </div>
        <div>
          <span>
            {this.calcGoalProgress(
              this.props.total, 
              this.props.goal
            )}
          </span>
        </div>
      </div>
    )
  }
}

SkiDayCount.defaultProps = { 
  total: 50,
  powder: 10,
  backcountry: 15,
  goal: 75
}

SkiDayCount.propTypes = {
  total: PropTypes.number,
  powder: PropTypes.number,
  backcountry: PropTypes.number
}

// index.js
// Don't forget to import
import { SkiDayCount } from './components/SkiDayCount-ES6'
```

### PropTypes with  Stateless method
- Uses the same code as ES6 method at the very bottom
```js
// SkiDayCount.js
SkiDayCount.propTypes = {
  total: PropTypes.number,
  powder: PropTypes.number,
  backcountry: PropTypes.number
}

// index.js
// Don't forget to import
import { SkiDayCount } from './components/SkiDayCount'
```

## Custom validation
- Helps you check for other criterias you want
- Eve added goal PropTypes:
```js
// SkiDayCount.js
SkiDayCount.propTypes = {
  total: PropTypes.number,
  powder: PropTypes.number,
  backcountry: PropTypes.number
  goal: PropTypes.number
}
```

- Eve added Proptypes in SkiDayRow.js
```js
import { PropTypes } from 'react'

SkiDayCount.propTypes = {
  resort: PropTypes.string.isRequired,
  date: PropTypes.instanceOf(Date).isRequired,
  powder: PropTypes.bool,
  backcountry: PropTypes.bool
}
```

- Eve added Proptypes in SkiDayList.js
```js
import { PropTypes } from 'react'

SkiDayList.propTypes = {
  days.PropTypes.array
}
```

### Setting up custom validations with a function
- Eve added Proptypes in SkiDayList.js
- The first check is if there an array
- The second check makes sure that it is not an empty array
```js
import { PropTypes } from 'react'

SkiDayList.propTypes = {
  days: function(props) {
    if(!Array.isArray(props.days)) {
      return new Error(
        "SkiDayList should be an array"
      )
    } else if(!props.days.length) {
      return new Error(
        "SkiDayList must have at least one record"
      )
    } else {
      return null
    }
  }
}
```

## Working with state
- State represents conditions in your application
- You could have a state for editing/save or logged in/logged out
- We should:
  1. Identify the minimal representation of app state
  2. Reduce state to as few components as possible
  3. Avoid overwriting state variables
- Now, we will create the App component to maintain the state 
1. Create a new `App.js` in `./scr/components/`
- We use `getInitialState()` to set the state with stateless functins
```js
// App.js
import { createClass } from 'react'

export const App = createClass({
  getInitialState(){
    return {
      allSkiDays: [
			{
				resort: "Squaw Valley",
				date: new Date("1/2/2016"),
				powder: true,
				backcountry: false
			},
			{
				resort: "Kirkwood",
				date: new Date("3/28/2016"),
				powder: false,
				backcountry: false
			},
			{
				resort: "Mt. Tallac",
				date: new Date("4/2/2016"),
				powder: false,
				backcountry: true
			}
		]
    }
  },
  render() {
    return (
      <div className="App">
        {this.state.allSkiDays[0]["resort"]}
      </div>
    )
  }
})
```

2. Change index to render correct components
```js
import React from 'react'
import { render } from 'react-dom'
import './stylesheets/ui.scss'
import { App } from './components/App'

window.React = React

render(
	<App />, 
	document.getElementById('react-container')
)
```
## Passing state as props
- Now we are passing down state to the child components
1. Import `SkiDayList` and `SkiDayCount` components into `App.js`
2. Change our render method
3. Create countDays() method
4. Add filters for countDays method
```js
// App.js
import { createClass } from 'react'
import { SkiDayList } from '.SkiDayList'
import { SkiDayCount } from '.SkiDayCount'

export const App = createClass({
  getInitialState(){
    return {
      allSkiDays: [
			{
				resort: "Squaw Valley",
				date: new Date("1/2/2016"),
				powder: true,
				backcountry: false
			},
			{
				resort: "Kirkwood",
				date: new Date("3/28/2016"),
				powder: false,
				backcountry: false
			},
			{
				resort: "Mt. Tallac",
				date: new Date("4/2/2016"),
				powder: false,
				backcountry: true
			}
		]
    }
  },
  countDays(filter) {
    return this.state.allSkiDays.filter(function(day) {
      if (filter) {
        return day[filter]
      } else {
        return day
      }
    }).length
  },
  render() {
    return (
      <div className="App">
        <SkiDayList days={this.state.allSkiDays} />
        <SkiDayCount total={this.countDays()}
                     powder={this.countDays("powder")}
                     backcountry={this.countDays("backcountry")}/>
      </div>
    )
  }
})
```

### Refactoring `countDays()`
```js
  countDays(filter) {
    const { allSkiDays } = this.state
    return allSkiDays.filter((day) => (filter) ? day[filter] : day).length
  },
```

## State with ES6 classes
- ES6 classes has slightly different syntax for handling state
1. Import `Component` from React
2. Remove parens on component and commas after each methods
3. We use `constructor` instead of `getInitialState()`
4. Remove `getInitialState()` moethod
### With createClass...
```js
// App.js
import { createClass } from 'react'
import { SkiDayList } from '.SkiDayList'
import { SkiDayCount } from '.SkiDayCount'

export const App = createClass({
  getInitialState(){
    return {
      allSkiDays: [
			{
				resort: "Squaw Valley",
				date: new Date("1/2/2016"),
				powder: true,
				backcountry: false
			},
			{
				resort: "Kirkwood",
				date: new Date("3/28/2016"),
				powder: false,
				backcountry: false
			},
			{
				resort: "Mt. Tallac",
				date: new Date("4/2/2016"),
				powder: false,
				backcountry: true
			}
		]
    }
  },
  countDays(filter) {
    return this.state.allSkiDays.filter(function(day) {
      if (filter) {
        return day[filter]
      } else {
        return day
      }
    }).length
  },
  render() {
    return (
      <div className="App">
        <SkiDayList days={this.state.allSkiDays} />
        <SkiDayCount total={this.countDays()}
                     powder={this.countDays("powder")}
                     backcountry={this.countDays("backcountry")}/>
      </div>
    )
  }
})
```

### To ES6 Classes
```js
// App.js
import { Component } from 'react'
import { SkiDayList } from '.SkiDayList'
import { SkiDayCount } from '.SkiDayCount'

export class App extends Component {
  constructor(props) {
    super(props)
    this.state = {
        allSkiDays: [
        {
          resort: "Squaw Valley",
          date: new Date("1/2/2016"),
          powder: true,
          backcountry: false
        },
        {
          resort: "Kirkwood",
          date: new Date("3/28/2016"),
          powder: false,
          backcountry: false
        },
        {
          resort: "Mt. Tallac",
          date: new Date("4/2/2016"),
          powder: false,
          backcountry: true
        }
      ]
    }
  }
  countDays(filter) {
    return this.state.allSkiDays.filter(function(day) {
      if (filter) {
        return day[filter]
      } else {
        return day
      }
    }).length
  }
  render() {
    return (
      <div className="App">
        <SkiDayList days={this.state.allSkiDays} />
        <SkiDayCount total={this.countDays()}
                     powder={this.countDays("powder")}
                     backcountry={this.countDays("backcountry")}/>
      </div>
    )
  }
}
```