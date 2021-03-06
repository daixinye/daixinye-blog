---
layout: post
title:  "React：基本环境搭建简明步骤"
date:   2017-04-14 22:34:00 +0800
tags: react
---
超简的步骤整理。待完善。

- Babel
- ESLint
- webpack

## Babel

`babel`是一个多用途的JavaScript编译器，用它的目的有二：一是支持ES6语法，二是支持`React`的一些特性（JSX）语法等。

### 安装

在这里我们是跟`webpack`一起用的，所以需要的是`babel-core`核心模块和`babel-lodaer`，如果需要单独使用`babel`则安装`babel-cli`。

    $ npm i babel-core babel-loader --save-dev

接下来安装babel presets

    $ npm i babel-preset-es2015 babel-preset-react --save-dev
    
### 配置

在根目录下新建一个`.babelrc`文件，并加入以下配置

    {
        "presets": ["es2015", "react"]
    }

## ESLint

ESLint是一个代码检查工具，用于检查和统一代码规范。

### 安装

    $ npm i eslint eslint-loader --save-dev

### 配置

    $ eslint --init

接下来根据你的需求进行选择即可，非常方便。最终可选择生成一个`.eslint.json`文件。我的配置如下：

    {
        "env": {
            "browser": true,
            "commonjs": true,
            "es6": true,
            "node": true
        },
        "extends": "eslint:recommended",
        "parserOptions": {
            "ecmaFeatures": {
                "experimentalObjectRestSpread": true,
                "jsx": true
            },
            "sourceType": "module"
        },
        "plugins": [
            "react"
        ],
        "rules": {
            "linebreak-style": [
                "error",
                "unix"
            ],
            "quotes": [
                "error",
                "single"
            ],
            "semi": [
                "error",
                "always"
            ]
        }
    }

更多的配置可参照[ESLint](http://eslint.org)

## webpack

`webpack`是当前非常流行的模块打包工具，上述的`Babel`和`ESLint`都可以作为`loader`在`webpack`中被使用。

注意：本文使用的是`webpack 2`，`webpack 1`和`webpack 2`在配置上有诸多不同之处，请务必留心。

[链接：webpack 1和2的区别](https://webpack.js.org/guides/migrating/)

### 安装

    $ npm i webpack webpack-dev-server --save-dev
    
### 插件

用于自动生成HTML页面，并引入正确的JavaScript文件依赖。

    $ npm i html-webpack-plugin --save-dev

### loader

处理CSS文件需要用到的两个loader。

    $ npm i css-loader style-loader

### 配置

在根目录下新建一个`app`目录，同时再创建一个`webpack.config.js`文件。

    var path = require('path');
    var HtmlWebpackPlugin = require('html-webpack-plugin');
    
    var ROOT_PATH = path.resolve(__dirname);
    var APP_PATH = path.resolve(ROOT_PATH, 'app');
    var BUILD_PATH = path.resolve(ROOT_PATH, 'build');
    
    module.exports = {
        entry: {
            app: path.resolve(APP_PATH, 'app.jsx')
        },
        output: {
            path: BUILD_PATH,
            filename: 'bundle.js'
        },
        devtool: 'eval-source-map',
        devServer: {
            historyApiFallback: true,
            hot: true,
            inline: true,
        },
        module: {
            rules: [{
                test: /\.jsx?$/,
                enforce: 'pre',
                loaders: ['eslint-loader'],
                include: APP_PATH
            }, {
                test: /\.css$/,
                loaders: ['style-loader', 'css-loader']
            }, {
                test: /\.jsx?$/,
                loader: ['babel-loader'],
                include: APP_PATH
            }],
        },
        plugins: [
            new HtmlWebpackPlugin({
                title: 'react-dev'
            })
        ],
        resolve: {
            extensions: ['.js', '.jsx']
        }
    };

## 最后

添加两条命令到`package.json`里。

    "scripts": {
        "build":"webpack",
        "dev":"webpack-dev-server --hot"
    }

通过在Terminal中输入 `npm run build` 或 `npm run dev` 执行。