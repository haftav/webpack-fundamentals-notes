[Table of Contents](../README.md)

## 1. Why Webpack

### Webpack Origins

- There are two ways Javascript can be used in the browser
    1. Adding a script tag and src attribute referencing a JS file
    2. Write your JS in your HTML
- What are the problems with these techniques?
    - Not scalable 
        - Too many scripts you're trying to load from script tags
        - Can create bottlenecks within the browser
    - Scripts can become too massive and unmaintainable
- How do we solve these problems?
    - IIFE (Immediately Invoked Function Expressions)
        - Revealing Module Pattern
            - Allows us to use data from an outside scope and return scoped information
            - Lets us encapsulate our data
```javascript
    var outerScope = 1;
    const whatever = (function(dataNowUsedInside) {
        var outerScope = 4;
        return {
            someAttribute: "youwant"
        }
    })(1)

    console.log(outerScope);
    /**
    * console log returns 1 -> no scope leakage!
    /*
```
- People started to treat each file as an IIFE (Revealing Module)
    - This allows us to concatenate; we can 'safely' combine files without concern of scopes overriding
    - This led to a number of tools that handle this concatenation (Make, Grunt, Gulp, etc.)
    - However, there are still issues with this technique
        - Every time you edit a file, everything has to be rebuilt
        - Dead code -> doesn't help tie usages across files
            - How do you know if there's code you're not using, and if so, how do you remove it?
        - Lots of IIFE's are slow
        - Can't lazy load

- These issues, in part, led to the birth of JS modules
    - How does Node work? How can you use JS when there's no DOM (meaning no script tags)?
    - CommonJS (Modules 1.0)
        - Require keyword -> lets you import pieces of other modules to use in a module (file)
        - Led to the explosion of npm modules
        - However, there are still issues
            - No browser support
            - No live bindings (issues with circular references)
            - CommonJS algorithm is somewhat slow
        - These issues led to bundlers and linkers gaining popularity
            - Browserify (static), RequireJS and SystemJS (loaders)
                - Problems:
                    - People started requiring all modules up front (terrible for performance)
                    - CommonJS is too dynamic a module format to have optimized code
- The solution to all the previous issues? ESM (EcmaScript Modules)
    - Took some inspiration from CommonJS (named exports)
    - Benefits of ESM:
        - Reuseability
        - Encapsulation
        - Organization
        - Convenience
    - Issues with ESM:
        - ESM for Node still a work in progress
        - Using modules natively in the browser is incredibly slow (pretty much unusable past 10 modules)
            - At runtime, when the import statement is hit, JS in the browser checks where that package came from, resolves it, makes sure the exports in that package are valid, repeats the process for that file, and continues the process until it finds every module that's referenced
- Every library is different: authors choose the module types they want to use
    - And this is just for JS!
        - Think about other preprocessors (CSS or HTML for example)
    - Wouldn't it be nice if there were a tech that lets us write modules, supports any module format, and can handle resources and other assets at the same time?

### Enter Webpack
 - Webpack is a module bundler
    - Lets you write any module format and compiles them **_for the browser_**
    - Supports static async bundling (you can create separate lazy-loaded bundles at build time)
    - Rich, vast ecosystem
    - Most performant way to ship Javascript today

### Configuring Webpack
- webpack.config.js
    - Like most tools used in JS apps, the webpack config file is just a CommonJS module
    - Object that contains properties that describe to Webpack what it should do with this code
- Webpack CLI
    - Almost every property is bound to the Webpack CLI parameters
    - You could use Webpack without needing a config file
- Node API