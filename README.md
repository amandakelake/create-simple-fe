# 一、快速搭建基于ES6的热更新环境

## 一、Usage

```
▶ git clone git@github.com:amandakelake/create-simple-fe.git
▶ cd create-simple-fe
▶ yarn / npm install
▶ yarn dev / npm run dev
```


## 二、搭建过程
### 1、初始化+Webpack打包
```
npm init -y
touch src/index.js
yarn add webpack webpack-cli -D
touch webpack.dev.config.js
```

```js
// webpack.dev.config.js文件
module.exports = {
  entry: "./src/index.js",
  output: {
    path: __dirname,
    filename: "./release/bundle.js"
  }
}
```

`package.json`文件加一条script
```
"dev": "webpack --config ./webpack.dev.config.js --mode development"
```

### 2、自动打包插入js文件，热更新
```
yarn add webpack-dev-server html-webpack-plugin -D
touch index.html
```

```js
const path = require('path')
const HtmlWebpackPlugin = require('html-webpack-plugin')

module.exports = {
  entry: "./src/index.js",
  output: {
    path: __dirname,
    filename: "./release/bundle.js"
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: './index.html'
    })
  ],
  devServer: {
    // 根目录
    contentBase: path.join(__dirname, './release'),
    // 自动打开浏览器
    open: true,
    port: 9124
  }
}
```

`package.json`文件改script
```
"dev": "webpack-dev-server --config ./webpack.dev.config.js --mode development"
```

### 3、babel编译ES6代码
此处因为babel升级到7的原因,所以babel-loader@7要指定版本（具体到时候看提示修改即可，不必担心）
```
yarn add -D babel-core babel-loader@7 babel-polyfill babel-preset-es2015 babel-preset-latest

touch .babelrc
```

```js
// .babelrc文件
{
  "presets": ["es2015", "latest"],
  "plugins": []
}
```

```js
const path = require('path')
const HtmlWebpackPlugin = require('html-webpack-plugin')

module.exports = {
  entry: "./src/index.js",
  output: {
    path: __dirname,
    filename: "./release/bundle.js"
  },
  plugins: [
    // bundle.js文件会被自动插入到模板文件中
    new HtmlWebpackPlugin({
      template: './index.html'
    })
  ],
  devServer: {
    // 根目录
    contentBase: path.join(__dirname, './release'),
    // 自动打开浏览器
    open: true,
    port: 9124
  },
  module: {
    rules: [{
      test: /\.js?$/,
      exclude: /(node_modules)/,
      loader: 'babel-loader'
    }]
  }
}
```
