# React Typescript Template From Scratch

- [React Typescript Template From Scratch](#react-typescript-template-from-scratch)
  - [Basic Setup Step](#basic-setup-step)
  - [Styles Setup Step](#styles-setup-step)
  - [Setup multiple environment for production and development.](#setup-multiple-environment-for-production-and-development)

## Basic Setup Step

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
9. install babel dev dependencies

```sh
yarn add -D @babel/core @babel/preset-env @babel/preset-react @babel/preset-typescript @babel/plugin-transform-runtime
```

10. create .babelrc in project root directory

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

## Styles Setup Step

1. install dev dependencies.

```sh
yarn add -D css-loader style-loader
```

2. add configuration in webpack.config.js

```js
{
  test: /\.css$/,
  use: ["style-loader", "css-loader"],
},
```

3. create `declarations.d.ts` then add declare

```ts
declare module "*.png"
```

4. add configuration for images in webpack.config.js

```js
{
  test: /\.(?:ico|gif|png|jpg|jpeg)$/i,
  type: "asset/resource",
},
```

5. add asset/inline configuration to serve inline images like svg.

```js
{
  test: /\.(woff(2)?|eot|ttf|otf|svg|)$/,
  type: "asset/inline",
},
```

## Setup multiple environment for production and development.

1. create webpack.dev.js, webpack.prod.js, webpack.common.js

webpack.dev.js

```js
const webpack = require("webpack");

module.exports = {
  mode: "development",
  devtool: "cheap-module-source-map",
  plugins: [
    new webpack.DefinePlugin({
      "process.env.name": JSON.stringify("Vishwas"),
    }),
  ],
};
```

webpack.prod.js

```js
const webpack = require("webpack");

module.exports = {
  mode: "production",
  devtool: "source-map",
  plugins: [
    new webpack.DefinePlugin({
      "process.env.name": JSON.stringify("Codevolution"),
    }),
  ],
};
```

webpack.common.js

```js
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
  plugins: [
    new HtmlWebpackPlugin({
      template: path.resolve(__dirname, "..", "./src/index.html"),
    }),
  ],
  stats: "errors-only",
};
```

webpack.config.js

```js
const { merge } = require("webpack-merge");
const commonConfig = require("./webpack.common.js");

module.exports = (envVars) => {
  const { env } = envVars;
  const envConfig = require(`./webpack.${env}.js`);
  const config = merge(commonConfig, envConfig);
  return config;
};
```

2. add scripts

```json
"scripts": {
  "start": "webpack serve --config webpack/webpack.config.js --env env=dev --open",
  "build": "webpack --config webpack/webpack.config.js --env env=prod",
},
```

3. Change App.tsx to check environment.

```ts
import "./styles.css";
import IMAGE from "./react.png";
import LOGO from "./logo.svg";

export const App = () => {
  return (
    <>
      <h1>
        React Typescript Webpack Starter Template - {process.env.NODE_ENV} {process.env.name}
      </h1>
      <img src={IMAGE} alt="React Logo" width="300" height="300" style={{ objectFit: "cover" }} />
      <img src={LOGO} alt="React Logo" width="300" height="300" style={{ objectFit: "cover" }} />
    </>
  );
};
```