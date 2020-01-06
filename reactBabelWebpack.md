Set-up project with Babel, React and Webpack
============================================

Install required packages using `npm`:

```sh
npm init -y
npm install react react-dom --save
npm install webpack webpack-dev-server @babel/core @babel/preset-env @babel/preset-react \
    @babel/plugin-proposal-decorators @babel/plugin-proposal-class-properties babel-loader --save-dev
```

Webpack configuration in `webpack.config.js` assuming that `app/index.js` is the entry point to the application:

```js
var path = require("path");

var DIST_DIR = path.resolve(__dirname, "dist");
var SRC_DIR = path.resolve(__dirname, "src");
var config = {
    entry: SRC_DIR + "/app/index.js",
    output: {
        path: DIST_DIR + "/app",
        filename: "bundle.js",
        publicPath: "/app"
    },
    module: {
        rules : [
            {
                test: /\.js$/,
                include: SRC_DIR,
                loader: "babel-loader",
                query: {
                    presets: [
                        "@babel/preset-env",
                        "@babel/preset-react"
                    ],
                    plugins: [
                        ["@babel/plugin-proposal-decorators", {"legacy": true}],
                        ["@babel/plugin-proposal-class-properties", {"loose": true}]
                    ]
                }
            }
        ]
    }
};
module.exports = config;
```

Configuration of `npm` scripts in `package.json`:

```js
{
  // ...
  "scripts": {
    "start": "npm run build",
    "build": "webpack -d && cp src/index.html dist/ && webpack-dev-server --content-base src/ --inline --hot",
    "build:prod": "webpack -p && cp src/index.html dist/"
  },
  //...
}
```

