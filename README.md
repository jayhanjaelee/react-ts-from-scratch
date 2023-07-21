# React Typescript Template From Scratch

## Steps

1. mkdir src build
2. npm init --y
3. create src/index.html
4. install react dependencies

```sh
yarn add react react-dom
```

5. install typescript dev dependencies

```sh
yarn add -D typescript @types/react @types/react-dom
```

6. create tsconfig.json
7. create src/App.tsx
8. create src/index.tsx
9. install babel dependencies

```sh
yarn add -D @babel/core @babel/preset-env @babel/preset-react @babel/preset-typescript @babel/plugin-transform-runtime
```

10. create .babelrc

```json
{
  "presets": [
    "@babel/preset-env",
    [
      "@babel/preset-react",
      {
        "runtime": "automatic"
      }
    ],
    "@babel/preset-typescript"
  ],
  "plugins": [
    [
      "@babel/plugin-transform-runtime",
      {
        "regenerator": true
      }
    ]
  ]
}
```

11. install webpack dev dependencies

```sh
yarn add -D webpack webpack-cli webpack-dev-server html-webpack-plugin
```

12. install babel-loader package to transpiling source code.

```sh
yarn add -D babel-loader
```

13. create webpack directory
14. create webpack/webpack.config.js

```javascript
const path = require("path");
const HtmlWebpackPlugin = require("html-webpack-plugin");

module.exports = {
  entry: path.resolve(__dirname, "..", "./src/index.tsx"),
  resolve: {
    extensions: [".tsx", ".ts", ".js"],
  },
  module: {
    rules: [
      {
        test: /\.(ts|js)x?$/,
        exclude: /node_modules/,
        use: [
          {
            loader: "babel-loader",
          },
        ],
      },
      {
        test: /\.css$/,
        use: ["style-loader", "css-loader"],
      },
      {
        test: /\.(?:ico|gif|png|jpg|jpeg)$/i,
        type: "asset/resource",
      },
      {
        test: /\.(woff(2)?|eot|ttf|otf|svg|)$/,
        type: "asset/inline",
      },
    ],
  },
  output: {
    path: path.resolve(__dirname, "..", "./build"),
    filename: "bundle.js",
  },
  mode: "development",
  plugins: [
    new HtmlWebpackPlugin({
      template: path.resolve(__dirname, "..", "./src/index.html"),
    }),
  ],
  stats: "errors-only",
};
```

15. add start scripts in package.json

```json
"scripts": {
  "start": "webpack serve --config webpack/webpack.config.js --open"
}
```