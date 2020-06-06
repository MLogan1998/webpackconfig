## Webpack Config:

 1. `npm init -y`
 
 1. In package.json
    1.   `"main": "src/javascripts/main.js",`
    
    
    1.    
       ```js
           "scripts": {
             "start": "webpack-dev-server --mode development --open",
              "build": "webpack --mode production --module-bind js=babel-loader",
              "deploy": "npm run build && firebase deploy"
              }, 
          ```
1. `npm install @babel/core @babel/preset-env babel-loader css-loader eslint eslint-config-airbnb-base eslint-loader eslint-plugin-import file-loader html-loader html-webpack-plugin mini-css-extract-plugin node-sass sass-loader webpack webpack-cli webpack-dev-server --save-dev`

1. Create `.gitignore`
     
      ```
       .vscode/
       .DS_Store
       package-lock.json
       node_modules/
       dist/
       build/
      .firebase/
      ```
      
1. Create `.babelrc`
     ```
      {
        "presets": [
            "@babel/preset-env"
         ]
      }
     ```
1. Create `webpack.config.js`
```js
const webpack = require('webpack');
const HtmlWebPackPlugin = require("html-webpack-plugin");
const MiniCssExtractPlugin = require("mini-css-extract-plugin");
module.exports = {
  entry: './src/javascripts/main.js',
  devtool: "eval-source-map",
  module: {
    rules: [
      {
        enforce: "pre",
        test: /\.js$/,
        exclude: /node_modules/,
        loader: "eslint-loader",
	options: {
          formatter: require('eslint/lib/cli-engine/formatters/stylish')
        }
      },
      {
        test: /\.js$/,
        exclude: /node_modules/,
        use: {
          loader: "babel-loader"
        }
      },
      {
        test: /\.html$/,
        use: [
          {
            loader: "html-loader",
            options: { minimize: true }
          }
        ]
      },
      {
        test: /\.scss$/,
        use: [
          MiniCssExtractPlugin.loader,
            { loader: 'css-loader', options: { sourceMap: true, importLoaders: 1 } },
            { loader: 'sass-loader', options: { sourceMap: true } },
        ],
      },
      {
        test: /\.(png|svg|jpg|gif)$/,
        use: ['file-loader']
      },
      {
        test: /\.(woff|woff2|eot|ttf|otf)$/,
        use: ['file-loader']
      }
    ]
  },
  plugins: [
    new HtmlWebPackPlugin({
      template: "./src/index.html",
      filename: "./index.html"
    }),
    new MiniCssExtractPlugin({
      filename: "[name].css",
      chunkFilename: "[id].css"
    }),
    new webpack.ProvidePlugin({
      $: "jquery",
      jQuery: "jquery"
    })
  ],
  output: {
		path: __dirname + "/build",
		filename: "bundle.js"
	}
};
```
1. Create `.eslintignore`
```
webpack.config.js
node_modules
build
```
1. Create `.eslintrc`
```
{
  "parserOptions": {
    "ecmaVersion": 9,
    "sourceType": "module"
  },
  "extends": "airbnb-base",
  "globals": {
    "document": true,
    "window": true,
    "$": true,
    "XMLHttpRequest": true,
    "allowTemplateLiterals": true
  },
  "rules": {
    "no-console": [1, { "allow": ["error"] }],
    "no-debugger": 1,
    "class-methods-use-this": 0,
    "linebreak-style": 0,
    "max-len": [1,200,2]
  }
}
```
1. `npm install axios bootstrap firebase jquery popper.js @fortawesome/fontawesome-free --save`
1. Create `/src` directory.
   1. Create `/styles` and `/javascripts` directories in `/src`
   1. Create `index.html` in `/src`
   1. Create `main.scss` in `/src/styles/`
   1. Create `main.js` in `/src/javascripts/`. IMPORT `import '../styles/main.scss';`
