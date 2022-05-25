# ReactJS Skeleton App
This is a documentation for kickstarting a ReactJS project.

**References**:
- https://create-react-app.dev/
- https://www.youtube.com/watch?v=6c2NqDyxppU
- https://medium.com/analytics-vidhya/django-react-integration-37acc304e984
- Webpack:
  - https://webpack.js.org/guides/installation/
  - https://webpack.js.org/configuration/
- Babel
  - https://babeljs.io/docs/en/babel-preset-react
  - https://babeljs.io/docs/en/config-files

## How to setup a ReactJs project

- Start a new reactjs application:
  <br/>Inside `frontend` directory, run the following command:
  ```shell
  npx create-react-app my-react-app
  ```

- Navigate to react application directory
  ```shell
  cd my-react-app
  ```

- Install **Webpack**:
  Webpack takes all our javascript stuff and transpile/bundle it into one single javascript file.
  ```shell
  npm i webpack webpack-cli --save-dev
  ```
- Install **Babel**:
  This will transpile our code into javascript code that is friendly with all different web browsers. 
  Essentially, it will convert our ES6 or JSX code into javascript that will run in different browsers.
  ```shell
  npm i @babel/core babel-loader @babel/preset-env @babel/preset-react --save-dev
  ```
  
- Install addition package for **async and wait** functionality
  ```shell
  npm install @babel/plugin-proposal-class-properties
  ```
  
- Install **react router dom**:
  ```shell
  npm i react-router-dom
  ```

- **Configure babel**:
  Sets up the babel loader for us and set the presets that we will be using; environment presets, target node version,
  preset for react, plugin for async and wait, etc.
  
  Create `babel.config.json` file and add the following content.
  ```json
  {
    "presets": [
      [
        "@babel/preset-env",
        {}
      ],
      "@babel/preset-react"
    ],
    "plugins": [
      "@babel/plugin-proposal-class-properties"
    ]
  }
  ```  

**Setup a vanilla app**

- **Configure Webpack**:
  - Create `webpack.config.js` and add the following content:
    ```javascript
    const path = require("path");
    const webpack = require("webpack");

    module.exports = {
      entry:{
        main_app: "./src/main_app/index.js"
      },
      output: {
        path: path.resolve(__dirname, "../static/frontend"),
        filename: "[name]/bundle.js",
      },
      module: {
        rules: [
          {
            test: /\.js$/,
            exclude: /node_modules/,
            use: {
              loader: "babel-loader",
            },
          },
        ],
      },
      optimization: {
        // Removes unnecessary whitespaces, etc and minimizes the generated bundle files.
        minimize: true,
      },
      plugins: [
        new webpack.DefinePlugin({
          "process.env": {
            // This has effect on the react lib size
            NODE_ENV: JSON.stringify("production"),
          },
        }),
      ],
    };
    ```  
    
  - Edit `package.json` and add required scripts:
    ```json
    ...
    "scripts": {
      "dev": "webpack --mode development --watch",
      "build": "webpack --mode production"
    },
    ...
    ```

- Create an entry point js file `index.js` for the react application.
  ```text
   <project>/
      |--- src/
             |--- main_app/
                     |--- index.js    # <<--- React app entry point
  ```
  
  index.js
  ```javascript
  import {render} from "react-dom";
  import React from "react";
  
  import App from "./App";
  
  
  // Render the app
  const appDiv = document.getElementById("app");
  render(<App />, appDiv);
  ```
  
  Create your first component `scr/main_app/App.js`:
  ```javascript
  import React from "react";

  const App = () => {
    /* Main component of the app.
       This is the first component to be rendered */
  
    return (
      <div>
        <h1>Main App React Code!</h1>
        <p>This is my first react app.</p>
      </div>
    );
  };
  
  export default App;
  ```

### Run ReactJs Application 
Inside the ReactJs app directory `frontend/appsource`, run the following command:

- **Development Mode**:
  ```shell
  npm run dev
  ```
  This will create the js bundle at `frontend/static/frontend/main_app.js` that will be rendered by django html view.

- **Build for Production**:
  ```shell
  npm run build
  ```
  
## Code Formatters
https://enlear.academy/eslint-vs-prettier-57882d0fec1d
<br/>https://www.inkoop.io/blog/setup-eslint-for-react-with-prettier-pre-commit-and-vscode/

- **ESLint**: 
  ESLint is a javascript tool that checks your code for potential errors and bad code practices. It helps you enforce 
  a code standard and style guide in your codebase. You can add ESLint in any of your JavaScript Code. 
  <br/><br/>
  _References_:
  <br/>https://eslint.org/docs/user-guide/getting-started
  <br/>https://eslint.org/docs/rules/
  
  <br/>
  
  - Install eslint package:
    ```shell
    npm i eslint --save-dev
    ```
    
  - Install eslint cli globally:
    ```shell
    npm -g i eslint-cli
    ```
    
  - Generate eslint configuration file:
    ```shell
    npx eslint --init
    ```
    This will install `@eslint/config` & `eslint-plugin-react@latest` and prompt for a questionnaire. Answer the questions as follows:
    ```text
    √ How would you like to use ESLint? · To check syntax and find problems
    √ What type of modules does your project use? · JavaScript modules (import/export)
    √ Which framework does your project use? · React
    √ Does your project use TypeScript? · No
    √ Where does your code run? · browser
    √ What format do you want your config file to be in? · JSON
    The config that you've selected requires the following dependencies:
  
    eslint-plugin-react@latest
    √ Would you like to install them now with npm? · No / Yes
    ```
    Refer: https://enlear.academy/eslint-vs-prettier-57882d0fec1d#321f
  
    <br/>This will create `.eslintrc.json` file inside your react project.
    <br/><br/>
  
  - Edit `.eslintrc.json` and add the rules as per your preference. Refer the link https://eslint.org/docs/rules/.
    ```json
    {
        "env": {
            "browser": true,
            "es2021": true
        },
        "extends": [
            "eslint:recommended",
            "plugin:react/recommended"
        ],
        "parserOptions": {
            "ecmaFeatures": {
                "jsx": true
            },
            "ecmaVersion": "latest",
            "sourceType": "module"
        },
        "settings": {
            "react": {
                "version": "detect"
            }
        },
        "plugins": [
            "react"
        ],
        "rules": {
            "max-len": ["warn", {
                "code": 79,
                "ignoreUrls": true,
                "ignoreRegExpLiterals": true
            }],
            "indent": ["error", 2],
            "linebreak-style": ["error", "unix"],
            "quotes": ["error", "double"],
            "semi": ["error", "always"],
            "quote-props": ["error", "as-needed"],
            "multiline-comment-style": ["error", "bare-block"],
            "no-multiple-empty-lines": ["error", { "max": 2, "maxEOF": 0 }]
        }
    }
    ```
    
  - Create `.eslintignore` file inside react project directory as follows:
    ```ignorelang
    # Third party
    node_modules/

    # Build products
    build/
    dist/
    coverage/
    
    # Files
    webpack.config.js
    .eslintrc.json
    babel.config.json
    package-lock.json
    package.json
    ```
  - Add lint command to npm scripts:
    ```json
      ...
      "scripts": {
        "lint": "eslint"
      },
      ...
    ```
    
  - Enable ESLint live preview in PyCharm:
    - Opens settings from File > Settings
    - Go to Editor > Inspection 
    - Use search box to find Javascript and TypeScript > Code quality > ESLint
    - Enable it as with severity as Error
    <br/><br/>
    
  - **Run eslint**:
    ```shell
    eslint .
    ```
    
    Or
    
    ```shell
    npm run lint .
    ```

- **Prettier**:
  Prettier is a code formatter and it works with multiple languages. 
  
  <br/>References:
  <br/>https://prettier.io/docs/en/install.html
  <br/>https://www.inkoop.io/blog/setup-eslint-for-react-with-prettier-pre-commit-and-vscode/
  <br/><br/>

  - Navigate to react project directory:
    ```shell
    cd frontend/appsource
    ```    

  - Install prettier along with eslint-prettier plugin and config:
    ```shell
    npm i prettier eslint-config-prettier eslint-plugin-prettier --save-dev
    ```
  - Configure "eslint" to enable prettier plugin:
    Then you need to tell your ESLint about your available plugins that it should work with which is prettier in this 
    case. All you need to do is add the Prettier plugin option in your ESLint config.
    
    <br/>Config file: [.eslintrc.json](frontend/appsource/.eslintrc.json)
    ```json
    ...
    "extends": [
      ...
      "plugin:prettier/recommended"
    ],
    ...
    ```
  - Create `.prettierignore` file to let the Prettier CLI and editors know which files to not format.
    ```ignorelang
    # Third party
    node_modules/

    # Build products
    build/
    dist/
    coverage/
    
    # Files
    webpack.config.js
    .eslintrc.json
    babel.config.json
    package-lock.json
    package.json
    ```
    
  - Run prettier:
    ```shell
    # Change directory
    cd frontend/appsource
    
    # Run prettier
    npx prettier .
    
    # OR
    
    npx prettier --write .
    ```
    
### TL;DR
```shell
npm run lint .
npx prettier --write .
```
