[Table of Contents](../README.md)

## 1. Webpack from Scratch

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

