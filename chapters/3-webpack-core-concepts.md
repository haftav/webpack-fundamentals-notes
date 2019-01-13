[Table of Contents](../README.md)

## 3. Webpack Core Concepts

### Entry

- Conceptually, the entry point is the first file to 'kick-off' your app.
- Webpack uses this as the starting point
- We can define this using an entry property in the config

### Output

- After webpack creates a bundle from our dependency graph, it needs a name for the output file and a place to put it
- The output property tells webpack _where_ and _how_ to distribute bundles

### Loaders & Rules

- How do we want to treat files that aren't JS?
    - We can define rules (rulesets) to treat files that match a certain filetype
    - These rules allow us to transform files before they're added to the dependency graph, based on the loaders provided
    - Loaders are javascript modules that take the source file and return it in a modified state
        - Loaders tell webpack *how* to interpret and translate files
    - rulesets need (at minumum) two properties:
        - test
            - A regex expression that can be used to match certain filetype extensions
        - use
            - The loader or loaders to apply when a filetype is matched in the test property
    - This testing of rulesets is a per-file process; it's not something that just happens in bulk
    - Rule options:
    ```javacript
        {
            test: regex,
            use:(Array|String|Function),
            include: RegExp[],
            exclude: RegExp[],
            issuer: (RegExp|String)[],
            enforce: "pre"|"post"
        }
    ```
    - "include" and "exclude" allow us to specify which files to include or exclude for this ruleset
    - the "use" property could also be a chain of loaders
    - "enforce" allows us to control whether a rule is run before or after other rules

- Chaining Loaders
    - Loaders are just functions that take a source and return a new source
    - Loaders will always execute from right to left

### Plugins

- What is a plugin in Webpack?
    - Instance of an object with an 'apply' property (somewhere in the prototype chain)
    - Allows us to hook into the entire compilation lifecycle
    ```javascript
        module.exports = {
            //...
            plugins: [
                new YourExamplePlugin(),
                //other plugins you'd want to include can follow
            ]
        }
    ```
- 80% of webpack is made up of its own plugin system; it's an event-driven architecture
- Plugins can add additional functionality to bundles by giving access to the powerful CompilerAPI.

### Webpack Config

- Can also be a function that returns an object
- We can pass variables to the webpack config:

```javascript
    //package.json example script:
    "prod": "npm run webpack -- --env.mode production"
    //webpack.config.js
    const HtmlWebpackPlugin = require('html-webpack-plugin');

    module.exports = (env) => {
        return {
            mode: env.mode,
            output: {
                filename: "bundle.js"
            },
            plugins: [ 
                new HtmlWebpackPlugin()
            ]
        }
    }
```

- HtmlWebpackPlugin injects whatever output assets are created into a new index.html file

### Setting Up a Local Development Server
- Run webpack-dev-server instead of just webpack
- Under the hood, a bundle is created in memory, which is then served up to express

### Splitting Environment Config Files
- webpack-merge -> similar functionality to Object.assign, first argument specifies initial object, second argument specifies new properties
```javascript
    const webpack = require("webpack");
    const HtmlWebpackPlugin = require("html-webpack-plugin");
    const webpackMerge = require("webpack-merge");

    const modeConfig = env => require(`./build-utils/webpack.${env}`)(env);

    module.exports = ({ mode, presets } = { mode: "production", presets: [] }) => {
    return webpackMerge(
        {
        mode,
        output: {
            filename: "bundle.js"
            },
        plugins: [new HtmlWebpackPlugin(), new webpack.ProgressPlugin()]
        },
        modeConfig(mode)
    );
    };
```
- Example string interpolation in webpack:
```javascript
    output: {
        filename: "[chunkhash].js"
    }
```
- Prod features like hashing probably shouldn't be used in dev mode