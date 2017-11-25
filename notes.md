# Learning React.js
- Eve Porcello
- Started 11-16-2017 Ended 11-25-2017

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

# 5. Using the React Router
## Incorporating the router
- React Router is a community built application to navigate between different components
- We want to be able to navigate between SkiDayList and SkiDayCount components
1. Install react-router in npm
```js
npm install react-router@3.0.0 --save
```

2. Render the router instead of app
- The `/` represents the home route and `*` represents the wild card route
- `history` tracks changes on the address bar
```js
// ./src/index.js
import React from 'react'
import { render } from 'react-dom'
import './stylesheets/ui.scss'
import { App } from './components/App'
import { Router, Route, hashHistory } from 'react-router'

window.React = React

render(
	<Router history={hashHistory}>
		<Route path="/" component={App}/>
		<Route path="*" component={Whoops404}/>
	</Router>,
	document.getElementById('react-container')
)
```

3. Create new file for `Whoops404.js` and import component in `index.js`
- We don't need parens when you are just rendering JSX elements only
```js
// ./src/components/Whoops404.js
export const Whoops404 = () => 
	<div>
		<h1>Whoops, route not found</h1>
  </div>
  
// ./src/index.js
import React from 'react'
import { render } from 'react-dom'
import './stylesheets/ui.scss'
import { App } from './components/App'
import { Whoops404 } from './components/Whoops404'
import { Router, Route, hashHistory } from 'react-router'

window.React = React

render(
	<Router history={hashHistory}>
		<Route path="/" component={App}/>
		<Route path="*" component={Whoops404}/>
	</Router>,
	document.getElementById('react-container')
)
```

## Setting up routes
- We set up the home and the wild card routes
- Now we want to separate SkiDayList and SkiDayCount components separately
- We also want to add a component to let you add new ski days
1. Create AddDayForm component
- We are just going to render an `h1` for now
```js
// ./src/components/AddDayForm.js
export const AddDayForm = () => (
	<h1>Add A Day</h1>
)
```

2. Add routes for our different components
- We now add the list-days and add-day routes which is linked inside the App component
```js
// ./index.js
import React from 'react'
import { render } from 'react-dom'
import './stylesheets/ui.scss'
import { App } from './components/App'
import { Whoops404 } from './components/Whoops404'
import { Router, Route, hashHistory } from 'react-router'

window.React = React

render(
	<Router history={hashHistory}>
		<Route path="/" component={App}/>
		<Route path="list-days" component={App} />
		<Route path="add-day" component={App} />
		<Route path="*" component={Whoops404} />
	</Router>,
	document.getElementById('react-container')
)
```

3. Set up the route under the render() method
- The logic will display depending on the address route we are on
- The logic has 2 ternary operators so look carefully
- Don't forget to import AddDayForm
```js
// ./src/components/App.js
import { Component } from 'react'
import { SkiDayList } from './SkiDayList'
import { SkiDayCount } from './SkiDayCount'
import { AddDayForm } from './AddDayForm'

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
		const { allSkiDays } = this.state
		return allSkiDays.filter(
			(day) => (filter) ? day[filter] : day).length
	}
	render() {
		return (
			<div className="app">
        {(this.props.location.pathname === "/") ?
          <SkiDayCount total={this.countDays()}
                powder={this.countDays(
                    "powder"
                  )}
                backcountry={this.countDays(
                    "backcountry"
                  )}/> :
        (this.props.location.pathname === "/add-day") ?
          <AddDayForm /> :
          <SkiDayList days={this.state.allSkiDays}/>				 
			}
			</div>
		)
	}
}
```

## Navigating with the link component
- Now we are making a navigation for users so they can click between links
1. Create a new component called `Menu.js`
- We are linking them to the routes we created
- We are adding icons also
- `activeClassName` will show which link is active
- We import `Link` component from 'react-router'
```js
// ./src/components/Menu.js
import { Link } from 'react-router'
import HomeIcon from 'react-icons/lib/fa/home'
import AddDayIcon from 'react-icons/lib/fa/calendar-plus-o'
import ListDaysIcon from 'react-icons/lib/fa/table'

export const Menu = () => 
	<nav className="menu">
		<Link to="/" activeClassName="selected">
			<HomeIcon />
		</Link>
		<Link to="/add-day" activeClassName="selected">
			<AddDayIcon />
		</Link>
		<Link to="/list-days" activeClassName="selected">
			<ListDaysIcon />
		</Link>
	</nav>
```

2. We need to render our `Menu` in `App.js`
```js
import { Component } from 'react'
import { SkiDayList } from './SkiDayList'
import { SkiDayCount } from './SkiDayCount'
import { AddDayForm } from './AddDayForm'
import { Menu } from './Menu'

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
		const { allSkiDays } = this.state
		return allSkiDays.filter(
			(day) => (filter) ? day[filter] : day).length
	}
	render() {
		return (
			<div className="app">
			<Menu />
			{(this.props.location.pathname === "/") ?
			  <SkiDayCount total={this.countDays()}
							 powder={this.countDays(
							 		"powder"
							 	)}
							 backcountry={this.countDays(
							 		"backcountry"
							 	)}/> :
			 (this.props.location.pathname === "/add-day") ?
			 	<AddDayForm /> :
			 	<SkiDayList days={this.state.allSkiDays}/>				 
			}
					
			</div>
		)
	}
}
```

## Using route parameters
- We are going to add filters for powder or backcountry
1. Change SkiDayList Component
- We need to use brackets since it's not just JSX elements
- `colSpan` is for how much column space it takes
- We added `filteredDays` component for the filter feature
```js
// ./src/components/SkiDayList.js
import Terrain from 'react-icons/lib/md/terrain'
import SnowFlake from 'react-icons/lib/ti/weather-snow'
import Calendar from 'react-icons/lib/fa/calendar'
import { SkiDayRow } from './SkiDayRow'
import { PropTypes } from 'react'
import { Link } from 'react-router'

export const SkiDayList = ({days, filter}) => {
  const filteredDays = (!filter || 
  		!filter.match(/powder|backcountry/))?
  		days:
  		days.filter(day => day[filter])

  return (
  	<div className="ski-day-list">
	<table>
		<thead>
			<tr>
				<th>Date</th>
				<th>Resort</th>
				<th>Powder</th>
				<th>Backcountry</th>
			</tr>
			<tr>
				<td colSpan={4}>
					<Link to="/list-days">
						All Days
					</Link>
					<Link to="/list-days/powder">
						Powder Days
					</Link>
					<Link to="/list-days/backcountry">
						Backcountry Days
					</Link>
				</td>
			</tr>
		</thead>
		<tbody>
			{filteredDays.map((day, i) =>
				<SkiDayRow key={i}
						   {...day}/>	
				)}
		</tbody>

	</table>
	</div>
)
}  

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

2. Fix render in App.js
```js
// ./src/components/App.js
import { Component } from 'react'
import { SkiDayList } from './SkiDayList'
import { SkiDayCount } from './SkiDayCount'
import { AddDayForm } from './AddDayForm'
import { Menu } from './Menu'

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
		const { allSkiDays } = this.state
		return allSkiDays.filter(
			(day) => (filter) ? day[filter] : day).length
	}
	render() {
		return (
			<div className="app">
			<Menu />
			{(this.props.location.pathname === "/") ?
			  <SkiDayCount total={this.countDays()}
							 powder={this.countDays(
							 		"powder"
							 	)}
							 backcountry={this.countDays(
							 		"backcountry"
							 	)}/> :
			 (this.props.location.pathname === "/add-day") ?
			 	<AddDayForm /> :
			 	<SkiDayList days={this.state.allSkiDays}
			 				filter={this.props.params.filter}/>				 
			}
					
			</div>
		)
	}
}
```
3. Add another route in index.js
```js
import React from 'react'
import { render } from 'react-dom'
import './stylesheets/ui.scss'
import { App } from './components/App'
import { Whoops404 } from './components/Whoops404'
import { Router, Route, hashHistory } from 'react-router'

window.React = React

render(
	<Router history={hashHistory}>
		<Route path="/" component={App}/>
		<Route path="list-days" component={App}>
			<Route path=":filter" component={App} />
		</Route>
		<Route path="add-day" component={App} />
		<Route path="*" component={Whoops404}/>
	</Router>,
	document.getElementById('react-container')
)
```

## Nesting routes
- We are going to learn how to nest routes
- Eve is making Rock Appreciation Society
- Routes come from routes.js file
1. We make left and right side navigation
```js
// ./src/components/index.js
import MainMenu from './ui/MainMenu'

export const Left = ({ children }) => 
	<div className="page">
		<MainMenu className="main-menu"/>
		{children}
	</div>

export const Right = ({ children }) => 
	<div className="page">
		{children}
		<MainMenu className="main-menu"/>
	</div>

export const Whoops404 = ({ location }) =>
    <div>
        <h1>Whoops, resource not found</h1>
        <p>Could not find {location.pathname}</p>
    </div>
```

2. We add routes to handle children
```js
// ./src/components/routes.js
import React from 'react'
import { Router, Route, hashHistory } from 'react-router'
import Home from './components/ui/Home'
import About from './components/ui/About'
import MemberList from './components/ui/MemberList'
import  { Left, Right, Whoops404  } from './components'

const routes = (
    <Router history={hashHistory}>
        <Route path="/" component={Home} />
        <Route path="/" component={Left}>
        	<Route path="about" component={About} />
        	<Route path="members" component={MemberList} />
        </Route>
        <Route path="*" component={Whoops404} />
    </Router>
)

export default routes
```

# 6. Forms and Refs
## Creating a form component
- We are now going to add a form to add-day route
- This allows us to add resort, date, powder, and backcountry to our list
1. We make a class component
2. Make a form with labels and input
  - We use `htmlFor` refers to the id of the input 
  - We wrap checkboxes with divs to appear nicer
3. Populate data from default props
  - We also destructure our props at the top
  - Use `defaultValue` to set default strings
  - Use `defaultChecked` to set default checkboxes
```js
import { PropTypes, Component } from 'react'

export class AddDayForm extends Component {
	render() {

		const { resort, date, powder, backcountry } = this.props 

		return (
			<form className="add-day-form">

				<label htmlFor="resort">Resort Name</label>
				<input id="resort" 
					   type="text" 
					   required 
					   defaultValue={resort}/>

				<label htmlFor="date">Date</label>
				<input id="date" 
					   type="date" 
					   required 
					   defaultValue={date}/>

				<div>
					<input id="powder" 
						   type="checkbox" 
						   defaultChecked={powder}	/>
					<label htmlFor="powder">Powder Day</label>
				</div>

				<div>	
					<input id="backcountry" 
						   type="checkbox"
						   defaultChecked={backcountry} />
					<label htmlFor="backcountry">
						Backcountry Day
					</label>
				</div>
			</form>
		)
	}
}

AddDayForm.defaultProps = {
	resort: "Kirkwood",
	date: "2017-02-12",
	powder: true,
	backcountry: false
}


AddDayForm.propTypes = {
	resort: PropTypes.string.isRequired,
	date: PropTypes.string.isRequired,
	powder: PropTypes.bool.isRequired,
	backcountry: PropTypes.bool.isRequired
}
```

## Using refs in class components
- Props are how children components interacts with parent components
- Use `ref` for parent components to pull values from children components
1. Add `ref` so we can get values from children components
2. Add a button to add values
3. Capture values with `submit()` function
  - We use `e.preventDefault()` to prevent default behavior of the form
  - Default is clearing out the form
  - Use `.value` to grab values of strings
  - Use `.checked` to grab values of checkboxes
4. Add `onSubmit` to add a trigger
5. Add a constructor for `submit()`
  - We need to bind our custom method in the constructor
  - This is necessary since we are using an ES6's class component
```js
import { PropTypes, Component } from 'react'

export class AddDayForm extends Component {
	
	constructor(props) {
		super(props)
		this.submit = this.submit.bind(this)
	}

	submit(e) {
		e.preventDefault()
		console.log('resort', this.refs.resort.value)
		console.log('date', this.refs.date.value)
		console.log('powder', this.refs.powder.checked)
		console.log('backcountry', this.refs.backcountry.checked)

	}

	render() {

		const { resort, date, powder, backcountry } = this.props 

		return (
			<form onSubmit={this.submit} className="add-day-form">

				<label htmlFor="resort">Resort Name</label>
				<input id="resort" 
					   type="text" 
					   required 
					   defaultValue={resort}
					   ref="resort"/>

				<label htmlFor="date">Date</label>
				<input id="date" 
					   type="date" 
					   required 
					   defaultValue={date}
					   ref="date"/>

				<div>
					<input id="powder" 
						   type="checkbox" 
						   defaultChecked={powder}	
						   ref="powder"/>
					<label htmlFor="powder">Powder Day</label>
				</div>

				<div>	
					<input id="backcountry" 
						   type="checkbox"
						   defaultChecked={backcountry} 
						   ref="backcountry"/>
					<label htmlFor="backcountry">
						Backcountry Day
					</label>
				</div>
				<button>Add Day</button>
			</form>
		)
	}
}

AddDayForm.defaultProps = {
	resort: "Kirkwood",
	date: "2017-02-12",
	powder: true,
	backcountry: false
}


AddDayForm.propTypes = {
	resort: PropTypes.string.isRequired,
	date: PropTypes.string.isRequired,
	powder: PropTypes.bool.isRequired,
	backcountry: PropTypes.bool.isRequired
}
```

## Using refs in stateless components
- With stateless functional components, you don't have access to `this` keyword
- We can use callback functions as `ref` to capture the values from each field
1. Write an stateless function and put elements inside
  - Put the `submit` function and `return()` element inside the stateless function
  - Put the arguments of the destructured elements in the parameters
2. Clear instances of `this`
  - Remove `this` from `onSubmit`
3. Changing the refs inside each input
  - We do the same for all 4 inputs
  - Essentially we are making a variable that refers to the `ref`
  - The variable then holds the value which we pull in the `submit` function
4. Replace the `this.refs` in the submit function
5. Make our underscore values available in our function
6. Remove the export class

```js
import { PropTypes, Component } from 'react'

export const AddDayForm = ({ resort, 
							 date, 
							 powder, 
							 backcountry }) => {
	
	let _resort, _date, _powder, _backcountry
	
	const submit = (e) => {
		e.preventDefault()
		console.log('resort', _resort.value)
		console.log('date', _date.value)
		console.log('powder', _powder.checked)
		console.log('backcountry', _backcountry.checked)

	}

	return (
		<form onSubmit={submit} className="add-day-form">

			<label htmlFor="resort">Resort Name</label>
			<input id="resort" 
				   type="text" 
				   required 
				   defaultValue={resort}
				   ref={input => _resort = input}/>

			<label htmlFor="date">Date</label>
			<input id="date" 
				   type="date" 
				   required 
				   defaultValue={date}
				   ref={input => _date = input}/>

			<div>
				<input id="powder" 
					   type="checkbox" 
					   defaultChecked={powder}	
					   ref="powder"
					   ref={input => _powder = input}/>
				<label htmlFor="powder">Powder Day</label>
			</div>

			<div>	
				<input id="backcountry" 
					   type="checkbox"
					   defaultChecked={backcountry} 
					   ref="backcountry"
					   ref={input => _backcountry = input}/>
				<label htmlFor="backcountry">
					Backcountry Day
				</label>
			</div>
			<button>Add Day</button>
		</form>
	)
}

AddDayForm.defaultProps = {
	resort: "Kirkwood",
	date: "2017-02-12",
	powder: true,
	backcountry: false
}


AddDayForm.propTypes = {
	resort: PropTypes.string.isRequired,
	date: PropTypes.string.isRequired,
	powder: PropTypes.bool.isRequired,
	backcountry: PropTypes.bool.isRequired
}
```

## Two-way function binding
- We now need to pass the values up to the parent
- We also need to render existing values and new days users input
1. We will display a string for date
  - We removed the date formate from the `td`
  - We changed the default prop type to string
```js
// SkiDayRow.js
import Terrain from 'react-icons/lib/md/terrain'
import SnowFlake from 'react-icons/lib/ti/weather-snow'
import Calendar from 'react-icons/lib/fa/calendar'
import { PropTypes } from 'react'

export const SkiDayRow = ({resort, date, 
							powder, backcountry}) => (
	<tr>
		<td>
			{date}
		</td>
		<td>
			{resort}
		</td>
		<td>
			{(powder) ? <SnowFlake/> : null}
		</td>
		<td>
			{(backcountry) ? <Terrain /> : null}
		</td>
	</tr>						

)

SkiDayRow.propTypes = {
	resort: PropTypes.string.isRequired,
	date: PropTypes.string.isRequired,
	powder: PropTypes.bool,
	backcountry: PropTypes.bool
}
```
2. Make new object with values that user inputs
  - We made a new `onNewDay` object to store the user values
  - We also reset the values to blank strings and false
  - So users can add new values
  - We also need to add `onNewDay` component to our arguments
  - Removed `Component` from the import since we no longer use it
```js
// AddDayForm.js
import { PropTypes } from 'react'

export const AddDayForm = ({ resort, 
							 date, 
							 powder, 
							 backcountry,
							 onNewDay }) => {
	
	let _resort, _date, _powder, _backcountry
	
	const submit = (e) => {
		e.preventDefault()
		onNewDay({
			resort: _resort.value,
			date: _date.value,
			powder: _powder.checked,
			backcountry: _backcountry.checked
		})
		_resort.value = ''
		_date.value = ''
		_powder.checked = false
		_backcountry.checked = false

	}

	return (
		<form onSubmit={submit} className="add-day-form">

			<label htmlFor="resort">Resort Name</label>
			<input id="resort" 
				   type="text" 
				   required 
				   defaultValue={resort}
				   ref={input => _resort = input}/>

			<label htmlFor="date">Date</label>
			<input id="date" 
				   type="date" 
				   required 
				   defaultValue={date}
				   ref={input => _date = input}/>

			<div>
				<input id="powder" 
					   type="checkbox" 
					   defaultChecked={powder}	
					   ref="powder"
					   ref={input => _powder = input}/>
				<label htmlFor="powder">Powder Day</label>
			</div>

			<div>	
				<input id="backcountry" 
					   type="checkbox"
					   defaultChecked={backcountry} 
					   ref="backcountry"
					   ref={input => _backcountry = input}/>
				<label htmlFor="backcountry">
					Backcountry Day
				</label>
			</div>
			<button>Add Day</button>
		</form>
	)
}

AddDayForm.defaultProps = {
	resort: "Kirkwood",
	date: "2017-02-12",
	powder: true,
	backcountry: false
}


AddDayForm.propTypes = {
	resort: PropTypes.string.isRequired,
	date: PropTypes.string.isRequired,
	powder: PropTypes.bool.isRequired,
	backcountry: PropTypes.bool.isRequired
}
```

3. We change the state of our list and adding component to add new days
  - Removed all the ski days only to 1 item
  - We changed it so it's just a string
  - Added `onNewDay` component to our `AddDayForm` component
  - Added `addDay` method to add new days and reset state
  - Eve use spread operator to spread existing data and then add on user added days
  - Bind the `addDay` function to the constructor
```js
// App.js
import { Component } from 'react'
import { SkiDayList } from './SkiDayList'
import { SkiDayCount } from './SkiDayCount'
import { AddDayForm } from './AddDayForm'
import { Menu } from './Menu'

export class App extends Component {
	constructor(props) {
		super(props)
		this.state = {
			allSkiDays: [
			{
				resort: "Squaw Valley",
				date: "2016-01-02",
				powder: true,
				backcountry: false
			}
		]
		}
		this.addDay = this.addDay.bind(this)
	}

	addDay(newDay) {
		this.setState({
			allSkiDays: [
				...this.state.allSkiDays,
				newDay
			]
		})
	}

	countDays(filter) {
		const { allSkiDays } = this.state
		return allSkiDays.filter(
			(day) => (filter) ? day[filter] : day).length
	}

	render() {
		return (
			<div className="app">
			<Menu />
			{(this.props.location.pathname === "/") ?
			  <SkiDayCount total={this.countDays()}
							 powder={this.countDays(
							 		"powder"
							 	)}
							 backcountry={this.countDays(
							 		"backcountry"
							 	)}/> :
			 (this.props.location.pathname === "/add-day") ?
			 	<AddDayForm onNewDay={this.addDay}/> :
			 	<SkiDayList days={this.state.allSkiDays}
			 				filter={this.props.params.filter}/>				 
			}
					
			</div>
		)
	}
}
```

## Adding an autocomplete component
- We are going to add an autocomplete component and an image
1. Created `Autocomplete` class to hold our components
  - Don't to forget export `Component`
  - We have a data list with all the Tahoe resorts
  - We add `datalist` which gives it the autocomplete function
2. Grab user-input value and use set/get
  - We grab the value of the input with `ref`
  - I'm still not clear about the set and get yet
  - I think it grabs our choice in the autocomplete and sets it as the ref
  - It also updates that field to our choice as well
```js
// AddDayForm.js
import { PropTypes, Component } from 'react'

const tahoeResorts = [
	"Alpine Meadows",
	"Boreal",
	"Diamond Peak",
	"Donner Ski Ranch", 
	"Heavenly", 
	"Homewood",
	"Kirkwood",
	"Mt. Rose", 
	"Northstar",
	"Squaw Valley",
	"Sugar Bowl"
]

class Autocomplete extends Component {
	
	get value() {
		return this.refs.inputResort.value
	}

	set value(inputValue) {
		this.refs.inputResort.value = inputValue
	}

	render() {
		return (
			<div>
				<input ref="inputResort"
					   type="text" 
					   list="tahoe-resorts" />
				<datalist id="tahoe-resorts">
					{this.props.options.map(
						(opt, i) => 
						<option key={i}>{opt}</option>)}
				</datalist>
			</div>
		)
	}
}

export const AddDayForm = ({ resort, 
							 date, 
							 powder, 
							 backcountry,
							 onNewDay }) => {
	
	let _resort, _date, _powder, _backcountry
	
	const submit = (e) => {
		e.preventDefault()
		onNewDay({
			resort: _resort.value,
			date: _date.value,
			powder: _powder.checked,
			backcountry: _backcountry.checked
		})
		_resort.value = ''
		_date.value = ''
		_powder.checked = false
		_backcountry.checked = false

	}

	return (
		<form onSubmit={submit} className="add-day-form">

			<label htmlFor="resort">Resort Name</label>
			<Autocomplete options={tahoeResorts}
				   		  ref={input => _resort = input}/>

			<label htmlFor="date">Date</label>
			<input id="date" 
				   type="date" 
				   required 
				   defaultValue={date}
				   ref={input => _date = input}/>

			<div>
				<input id="powder" 
					   type="checkbox" 
					   defaultChecked={powder}	
					   ref="powder"
					   ref={input => _powder = input}/>
				<label htmlFor="powder">Powder Day</label>
			</div>

			<div>	
				<input id="backcountry" 
					   type="checkbox"
					   defaultChecked={backcountry} 
					   ref="backcountry"
					   ref={input => _backcountry = input}/>
				<label htmlFor="backcountry">
					Backcountry Day
				</label>
			</div>
			<button>Add Day</button>
		</form>
	)
}

AddDayForm.defaultProps = {
	resort: "Kirkwood",
	date: "2017-02-12",
	powder: true,
	backcountry: false
}


AddDayForm.propTypes = {
	resort: PropTypes.string.isRequired,
	date: PropTypes.string.isRequired,
	powder: PropTypes.bool.isRequired,
	backcountry: PropTypes.bool.isRequired
}
```

3. We then set the `render()` to return `Autocomplete` component
```js
// AddDayForm.js
import { PropTypes, Component } from 'react'

const tahoeResorts = [
	"Alpine Meadows",
	"Boreal",
	"Diamond Peak",
	"Donner Ski Ranch", 
	"Heavenly", 
	"Homewood",
	"Kirkwood",
	"Mt. Rose", 
	"Northstar",
	"Squaw Valley",
	"Sugar Bowl"
]

class Autocomplete extends Component {
	
	get value() {
		return this.refs.inputResort.value
	}

	set value(inputValue) {
		this.refs.inputResort.value = inputValue
	}

	render() {
		return (
			<div>
				<input ref="inputResort"
					   type="text" 
					   list="tahoe-resorts" />
				<datalist id="tahoe-resorts">
					{this.props.options.map(
						(opt, i) => 
						<option key={i}>{opt}</option>)}
				</datalist>
			</div>
		)
	}
}

export const AddDayForm = ({ resort, 
							 date, 
							 powder, 
							 backcountry,
							 onNewDay }) => {
	
	let _resort, _date, _powder, _backcountry
	
	const submit = (e) => {
		e.preventDefault()
		onNewDay({
			resort: _resort.value,
			date: _date.value,
			powder: _powder.checked,
			backcountry: _backcountry.checked
		})
		_resort.value = ''
		_date.value = ''
		_powder.checked = false
		_backcountry.checked = false

	}

	return (
		<form onSubmit={submit} className="add-day-form">

			<label htmlFor="resort">Resort Name</label>
			<Autocomplete options={tahoeResorts}
				   		  ref={input => _resort = input}/>

			<label htmlFor="date">Date</label>
			<input id="date" 
				   type="date" 
				   required 
				   defaultValue={date}
				   ref={input => _date = input}/>

			<div>
				<input id="powder" 
					   type="checkbox" 
					   defaultChecked={powder}	
					   ref="powder"
					   ref={input => _powder = input}/>
				<label htmlFor="powder">Powder Day</label>
			</div>

			<div>	
				<input id="backcountry" 
					   type="checkbox"
					   defaultChecked={backcountry} 
					   ref="backcountry"
					   ref={input => _backcountry = input}/>
				<label htmlFor="backcountry">
					Backcountry Day
				</label>
			</div>
			<button>Add Day</button>
		</form>
	)
}

AddDayForm.defaultProps = {
	resort: "Kirkwood",
	date: "2017-02-12",
	powder: true,
	backcountry: false
}


AddDayForm.propTypes = {
	resort: PropTypes.string.isRequired,
	date: PropTypes.string.isRequired,
	powder: PropTypes.bool.isRequired,
	backcountry: PropTypes.bool.isRequired
}
```

4.. We also add an image
  - In `index.scss`, you can link the image with 
  - Don't forget to import in `index.js` with `import './stylesheets/index.scss'`
```css
// index.scss
div.app {
 background-image:url('http://localhost:3000/assets/img/rowdy.jpg');
 background-size: cover;
 background-position: center;
}
```

# 7. The Component Lifecycle
## Challenge: Building the Member component
- We will be building a member profile in `Member.js`
- We are building each piece at a time
1. Render a name in the `<h1>`
  - Set up the properties by destructure our props
2. Render a react-icon
3. Populate an image based on a thumbnail image
4. `<a>` tag that has email
5. Add a property that makes the person an admin
6. Don't forget to import component in `index.js`
```js
// Member.js
import FaShield from 'react-icons/lib/fa/shield' 
import { Component } from 'react'

class Member extends Component {
  render() {
    const { name, thumbnail, email, admin, makeAdmin } = this.props
      return (
          <div className="member">
              <h1>{name} {(admin) ? <FaShield /> : null}</h1>
              <a onClick={makeAdmin}>Make Admin</a>
              <img src={thumbnail} alt="profile picture" />
              <p><a href={`mailto:${email}`}>{email}</a></p>
          </div>
      )
  }
}

export default Member
```

## Challenge: Building the MemberList Component
- Now we are rendering 3 members from `MemberList.js`
- Don't forget to render and import in `index.js`
1. Destructure members from state and loop over them
  - The spread operator is to go through all the members, regardless of how many are in state
  - Notice we have hard-coded members here
```js
// MemberList.js
import { Component } from 'react'
import fetch from 'isomorphic-fetch'
import Member from './Member'

class MemberList extends Component {
    constructor(props) {
        super(props)
        this.state = {
            members: [
            {
                name: "Joe Wilson",
                email: "joe.wilson@example.com",
                thumbnail: "https://randomuser.me/api/portraits/men/53.jpg"
            },
            {
                name: "Stacy Gardner",
                email: "stacy.gardner@example.com",
                thumbnail: "https://randomuser.me/api/portraits/women/74.jpg"
            },
            {
                name: "Billy Young",
                email: "billy.young@example.com",
                thumbnail: "https://randomuser.me/api/portraits/men/34.jpg"
            }
          ]
        }
    }

    render() {
    	const { members } = this.state
        return (
            <div className="member-list">
                <h1>Society Members</h1>
                {members.map(
                	(data, i) => 
                		<Member key={i} 
                				    onClick={email => console.log(email)}
                            {...data} />
                	 )}
            </div>
        )
    }
}

export default MemberList
```

## Understanding the mounting lifecycle
- We can execute codes during different points of the component's lifetime
- We are grabbing data from [randomuser.me](https://randomuser.me/)
1. We set members to an empty array and add loading state false
2. We add fetch to grab data from the api
3. Add `componentDidMount()`, which fires when `render()` is fired
4. Add fetch method
  - From the results, we set the state with the members that comes back from the API
5. Add a loading and render feature
  - Shows loading when it loads data and show how many members
  - We also load the API property keys to render random users
  - Will error 0 members if they're is no data
```js
// MemberList.js
import { Component } from 'react'
import fetch from 'isomorphic-fetch'
import Member from './Member'

class MemberList extends Component {
    constructor(props) {
        super(props)
        this.state = {
            members: [],
            loading: false
        }
    }

    componentDidMount() {
        this.setState({loading: true})
        fetch('https://api.randomuser.me/?nat=US&results=12')
            .then(response => response.json())
            .then(json => json.results)
            .then(members => this.setState({
                members,
                loading: false
            }))
    }

    render() {
        const { members, loading  } = this.state
        return (
            <div className="member-list">
            	<h1>Society Members</h1>

                {(loading) ?
                    <span>loading...</span> :
                    <span>{members.length} members</span>
                }

                {(members.length) ?
                    members.map(
                    (member, i) => 
                        <Member key={i} 
                                name={member.name.first + ' ' + member.name.last}
                                email={member.email}
                                thumbnail={member.picture.thumbnail} />
                    ):
                    <span>Currently 0 Members</span>
                }
            </div>
        )    
   }     
}

export default MemberList
```

6. We add a gray background before component mounts
```js
// Member.js
import FaShield from 'react-icons/lib/fa/shield' 
import { Component } from 'react'

class Member extends Component {

	componentWillMount() {
		this.style = {
			backgroundColor: 'gray'
		}
	}

	render() {

		const { name, thumbnail, email, admin, makeAdmin } = this.props
		return (
			<div className="member">
				<h1>{name} {(admin) ? <FaShield /> : null}</h1>
				<a onClick={makeAdmin}>Make Admin</a>
				<img src={thumbnail} alt="profile picture" />
				<p><a href={`mailto:${email}`}>{email}</a></p>

			</div>
		)
	}
}

export default Member
```

## Understanding the updating lifecycle
- We will be able to make and remove admins
1. Render routes
  - Our pages should be working now
```js
import React from 'react'
import { render } from 'react-dom'
import routes from './routes'

window.React = React

render(
	routes, 
	document.getElementById('react-container'))
```
2. Create administrators state
3. Create `makeAdmin()` and `removeAdmin()`
  - Don't forget to bind the method
  - Didn't understand why Eve used `email` as an argument
4. Add `admin` property to the render method
  - The `.some()` checks to see if there is match in the members array
5. Add `makeAdmin()` and `removeAdmin()` to the render method
```js
// MemberList.js
import { Component } from 'react'
import fetch from 'isomorphic-fetch'
import Member from './Member'

class MemberList extends Component {

    constructor(props) {
        super(props)
        this.state = {
            members: [],
            loading: false,
            administrators: []
        }
        this.makeAdmin = this.makeAdmin.bind(this)
        this.removeAdmin = this.removeAdmin.bind(this)
    }

        componentWillUpdate(nextProps) {
        this.style = { backgroundColor: (nextProps.admin) ? 'green' : 'purple' }
    }

    componentDidUpdate(prevProps) {
       console.log(`${prevProps.name} updated`, prevProps.admin, this.props.admin)
    }

    componentDidMount() {
        this.setState({loading: true})
        fetch('https://api.randomuser.me/?nat=US&results=12')
            .then(response => response.json())
            .then(json => json.results)
            .then(members => this.setState({
                members,
                loading: false
            }))
    }

    makeAdmin(email) {
        const administrators = [
            ...this.state.administrators,
            email
        ]
        this.setState({administrators})
    }

    removeAdmin(email) {
        const administrators = this.state.administrators.filter(
            adminEmail => adminEmail !== email
        )
        this.setState({administrators})
    }


    render() {
    	const { administrators, members, loading } = this.state
        return (
            <div className="member-list">
                <h1>Society Members</h1>

                {(loading) ?
                    <span>loading...</span> :
                    <span>{members.length} members</span>
                }

                {(members.length) ?
                   members.map(
                	(member, i) => 
                		<Member key={i} 
                                admin={administrators.some(
                                    adminEmail => adminEmail === member.email
                                    )}
                                name={member.name.first + ' ' + member.name.last} 
                                email={member.email}
                                thumbnail={member.picture.thumbnail}
                                makeAdmin={this.makeAdmin}
                                removeAdmin={this.removeAdmin}/>
                	 ):
                   <span>Currently 0 Members </span>
               }
            </div>
        )
    }
}

export default MemberList
```

6. Add button to be dynamic about a person's admin status
  - Button displays add/remove admin depending on status
7. Add lifecycle method `componentWillUpdate()`
  - Once component updates, it will turn green
8. Add `shouldComponentUpdate()`
  - Change background to green only to the person being made admin
  - She also used `nextProps` in this life cycle
  - The logic is hard to follow so need to look at it a couple times
9. Add `nextProps` argument
  - Will make admins green and undo admins to purple
  - But it will keep the purple background after removing admin
```js
// Member.js
import FaShield from 'react-icons/lib/fa/shield' 
import { Component } from 'react'

class Member extends Component {

  componentWillMount() {
  	this.style = {
  		backgroundColor: 'gray'
  	}
  }	

  componentWillUpdate(nextProps) {
    this.style = {
      backgroundColor: (nextProps.admin) ? 'green' : 'purple'
    }
  }

  shouldComponentUpdate(nextProps) {
    return this.props.admin !== nextProps.admin
  }

  render() {

	const { name, thumbnail, email, admin, 
          makeAdmin, removeAdmin } = this.props
    return (
        <div className="member" style={this.style}>
        	<h1>{name} {(admin) ? <FaShield /> : null}</h1>
          {(admin) ? 
            <a onClick={() => removeAdmin(email)}>Remove Admin</a> :
            <a onClick={() => makeAdmin(email)}>Make Admin</a>
          }

        	
        	<img src={thumbnail} alt="profile picture" />
        	<p><a href={`mailto:${email}`}>{email}</a></p>

        </div>
    )
}
}

export default Member
```

# Conclusion
## Next steps
- [Redux](https://redux.js.org/) is a state management solution that helps you manage ReactJS
- [React Native](https://facebook.github.io/react-native/) lets you build mobile apps using React and JavaScript
- Data management systems:
  - [Relay](https://facebook.github.io/relay/)
  - [Falcor](https://netflix.github.io/falcor/)
  - [GraphQL](http://graphql.org/)