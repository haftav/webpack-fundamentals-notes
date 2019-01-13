[Table of Contents](../README.md)

## 4. Using Plugins

### CSS with Webpack

- How can we effectively use CSS in Webpack?

Example File:
```javascript
import './footer.css';
```
- We need to set up loaders to process this new file type
```javascript
module: {
    rules: [
        {
            test: /\.css$/,
            use: [ "style-loader", "css-loader" ]
        }
    ]
}
```
- What happens if we just use css-loader?
    - It will import the CSS into the file in an array structure; technically, we could use this, but there are easier options out there.
- style-loader knows how to process the output from css-loader and apply these styles to matched selectors.
- Now you can update css files and these changes will be applied automatically (if using webpack-dev-server)

### Hot Module Replacement
- Webpack can update changes that are made incrementally without reloading the browser
- add  ```--hot``` tag at the end of your npm script

### Extracting CSS into a separate file
- Currently, Javscript is taking the CSS and inserting a style tag where needed 
- Instead, we can extract the CSS to a separate file and import it through a link tag in the index.html file. (This should probably be set up for a production env)
```javascript
module: {
    rules: [
    {
        test: /\.css$/,
        use: [MiniCssExtractPlugin.loader, "css-loader"]
    }
    ]
},
plugins: [
    new MiniCssExtractPlugin()
]
```