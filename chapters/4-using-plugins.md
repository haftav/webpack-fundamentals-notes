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

### File Loader & URL Loader
- The following loader (url-loader) will add .jpg images to the output directory
```javascript
module: {
    rules: [
        {
            test: /\.jpe?g$/,
            use: ['url-loader']
        }
    ]
}
```
- URL Loader by default will create a large URI which is not great for your bundle
    - Note that it also base64 encodes and inlines the image (bad for SEO)
- Luckily there's an option to limit this
- Instead of just using shorthand for your loader, you can pass an object instead which allows you to specify different options
```javascript
module: {
    rules: [
        {
            test: /\.jpe?g$/,
            use: [{
                loader: 'url-loader',
                options: {
                        limit: 5000
                    }
                }]
        }
    ]
}
```
- ^ This tells url-loader to only inline the image if it's 5000 bytes or less; otherwise, it will output the image to the dist directory and return the hashed url of where that file lives
- NOTE: url-loader is calling file-loader behind the scenes (which is why both of them need to be installed)

### Implementing Presets
- It's possible that scenarios may come up where you want to test a feature or analyze something without breaking the webpack production configuration