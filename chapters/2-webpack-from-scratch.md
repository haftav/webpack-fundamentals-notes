[Table of Contents](../README.md)

## 2. Webpack from Scratch

### NPM Scripts
- Within the node_modules folder there is a bin folder that holds binary executables)
- NPM allows you to run scripts that hoist these binary packages within scope
- You can run the 'webpack' command directly from the command line without a config file; by default, it will look for src/index.js as the entry point
- A script can call other scripts you've made

Example:
```javascript
    {
        "scripts": {
            "webpack": "webpack",
            "dev": "npm run webpack -- --mode development"
        }
    }
```
>Note: the first '--' means what follows will be piped on the command in front of it (in this case, webpack)

How can we debug a node script? 
- node --inspect 
    - Open chrome://inspect in Chrome to open Node devtools
    - Adding the --inspect-brk flag tells the debugger to break before code starts

What if we want to debug webpack?
- Dedicated node debugger tools help with this!

### Writing Our First Module

```javascript
//nav.js
    export default "nav";

//index.js
    import nav from "./nav";
    console.log(nav);
    //running index.js should result in 'nav' being logged to the console
```

- By default, the dist directory is where our bundled assets will go (main.js is the file that will be created if there's only a single entrypoint)
- What happens if we run "node ./dist/main.js"?
    - 'nav' will be logged to the console!
    - We've taken two modules and bundled them together, but the functionality is the same as if they were still separate

### Adding Watch Mode
- We probably (definitely) don't want to manually run a webpack script every time we change something in development.
    - We can add a --watch flag on to our script command
    - Webpack will watch for changes in the code and compile as needed
### Working with modules (ESM & CommonJS)
- Webpack outputs the dependency graph to the terminal so we can see what files are being bundled
- ESM
    - Building on the previous module example, we can have default exports and named exports (only one default export allowed per module)
```javascript
//nav.js
    export default "nav";

//footer.js
    const top = "top";
    const bottom = "bottom";

    export { top, bottom };
    //could also export top and bottom by adding export keyword in front of their declaration

//index.js
    import nav from "./nav";
    import { top, bottom } from "./footer";

    console.log(nav, top, bottom);
    //running index.js should result in 'nav top bottom' being logged to the console
```
- CommonJS
    - Webpack won't let you use CommonJS & ESM syntax in the same file; it'll throw an error
    - However, you could use ESM syntax to import things exported in CommonJS syntax:

```javascript
//button.js
    const makeButton = (buttonName) => {
        return `Button: ${buttonName}`;
    };

    module.exports = makeButton;
//index.js
    import button from "./button.js";
    //this won't throw an error
```

- Named CommonJS exports:
```javascript
//button-styles.js
    const red = "color: red";
    const blue = "color: blue";
    const makeColorStyle = (color) => `color: ${color};`; 

    exports.red = red;
    exports.blue = blue;
    exports.makeColorStyle = makeColorStyle;
```
- You should only pull in imports you're using; Webpack only bundles what you're using

- By default, when Webpack runs, it looks for the webpack.config.js file relative to the local path and requires it
- When we look at the main.js file, we see what Webpack outputs is just a giant IIFE with a modules parameter
    - The argument that gets passed in is an array of IIFE's;
        - These IIFE's are the actual modules (or files) we use
- This main.js file is known as the "webpackBootstrap" or "runtime code"
