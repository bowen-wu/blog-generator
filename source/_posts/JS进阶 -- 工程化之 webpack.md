---
title: JS进阶 -- 工程化之 webpack
date: 2018-02-27 09:09:03
tags: JavaScript 进阶
---
## [代码链接](https://github.com/bowen-wu/webpack-demo)
## 概述
工程化包括了自动化 + 模块化 + 性能优化，前端语言较多并且更新频率较快。
```
CSS ==> Less ==> Sass ==> Scss ==> Stylus（结合Less + Sass）
JS ==> coffee ==> ES6 ==> babel ==> TypeScript ==> Elm
HTML ==> Jade ==> Pug ==> slim
```
当我们面对这么多语言的时候，Webpack 应运而生，Webpack 是一个现代 JavaScript 应用程序的静态模块打包器(module bundler)，它主要为我们解决了工程化的问题。在本篇文章中将要介绍 SCSS + babel + Webpack

## Sass | Scss
Scss 是 CSS 的预处理语言 | 扩展，` Sass makes CSS fun again ` 这是Scss 要做的事情。Scss添加了嵌套规则、变量、混入、选择器继承，还能和命令行工具 | Web框架插件将其转换为格式良好的标准 CSS

#### Sass 与 Scss 区别
Sass 使用换行 + 缩进进行工作
```
body
    background: red
```
Scss 使用分号 + 花括号进行工作
```
body{
    background: red;
}
```
#### 安装 Scss
Scss 是有 Ruby 社区创作出来的，所以要使用 ` gem ` 命令，但是 ` node ` 将 Scss 重写了一遍，所以可以使用 ` node-sass `
```
npm install -g node-sass  // 安装
npm uninstall node-sass -g    // 卸载

解决问题 4 连 
npm update
npm install
nodejs node_modules/node-sass/scripts/install.js
npm rebuild node-sass

解决问题 7 连 查看 node-sass 是否存在
npm -v
node -v
node -p process.versions
node -p process.platform
node -p process.arch
node -p "require('node-sass').info"
npm ls node-sass

解决问题 3 连 重新生成 node_modules + package.json
npm -rf node_modules
rm package.json
rm package-lock.json
npm install

npm cache clean  // 清除缓存
npm cache clean --force  // 清除缓存
npm install --unsafe-perm
npm config set sass_binary_site http://npm.taobao.org/mirrors/node-sass/    // 切换淘宝源
npm i -g node-sass --save_binary_path=/home/wubowen/Downloads/Linux-x64-48-binding.node   // 通过 .node 包安装
npm install -g cnpm --registry=https://registry.npm.taobao.org  // 安装 cnpm
cnpm install -g node-sass  // cnpm 安装

运行配置文件
. ~/.bash_profile
. ~/.npmrc
source ~/.profile
source .npmrc
```
安装成功之后便可以使用 node-sass 了
```
node-sass [options] <input> [output]
示例
node-sass src/style.scss dist/style.css
node-sass src/style.scss dist/style.css -w  // Watch a directory or file
```
## babel
Babel is a JavaScript compiler。它是一款可以将 JS 语法编译为 ES5（IE能读）的编译器

#### 安装 + 使用
```
npm install --save-dev babel-cli babel-preset-env
touch .babelrc
编辑 .babelrc
{
  "presets": ["env"]
}
npm init   // 创建 package.json
npm install --save-dev babel-cli
package.json 多出一行
{
  "devDependencies": {
+   "babel-cli": "^6.0.0"
  }
}

在 package.json 中 + 
  {
    "name": "my-project",
    "version": "1.0.0",
+   "scripts": {
+     "build": "babel src -d lib"
+   },
    "devDependencies": {
      "babel-cli": "^6.0.0"
    }
  }

npm run build   // 运行 package.json 中的 "build": "babel src -d lib" 这句话
./node_modules/.bin/babel src -d lib    // 将 src 中的文件翻译到 lib 文件中

之后 index.html 引用 dist 中的文件

自动监控
./node_modules/.bin/babel path1 -d path2 --watch
```

## watch
watch 命令行工具可以监控文件的改动
```
npm i -g watch-cli
watch -p 'path1' -c 'cp path1 path2'  // 只要 path1 中的文件改动就会将 path1 中的文件复制到 path2 中
```
watch 也可以用于 img
```
watch -p 'src/img/**/*' -c 'cp -r src/img dist/img'
```

## 前端目录规范
src ==> source 目录 ==> 未经翻译的代码
dist ==> distribution 目录 ==> 待发布的代码
node_modules ==> 第三方包
vendors ==> 第三方代码（jQuery）

## Webpack
通过之前的介绍，我们使用
 scss ==> node-sass ==> css
ES6 JS ==> babel ==> ES5（IE 使用）
HTML + img ==> watch ==> 复制
我们通过这么多的命令才能实现我们的自动化，那么为什么不使用一种工具，具有以上三者的作用呢，所以 Webpack 应运而生

#### Webpack 核心
###### 入口（entry）
entry 指示 webpack 使用哪个模块来作为构建其内部依赖图的开始，进入进口后，webpack 会找出有哪些模块和库是入口直接或间接依赖的，每个依赖项随即被处理，最后输出到 bundles 的文件中
通过在 webpack 配置中配置 ` entry ` 属性来指定起点
```
webpack.config.js
module.exports = {
    entry: './path/to/my/entry/file.js'
}
```

###### 出口（output）
output 属性告诉 webpack 在哪里输出它所创建的 bundles，以及如何命名这些文件
```
webpack.config.js
module.exports = {
    entry: './path/to/my/entry/file.js',
    output: {
        path: path.resolve( __dirname, 'dist' )
        filename: 'my-first-webpack-bundle.js'
    }
}
```

###### loader
loader 让 webpack 能够去处理非 JavaScript 文件（webpack 自身只能理解 JavaScript）。webpack loader 将所有类型的文件，转换为应用程序的依赖图可以直接引用的模块。**多数 loader 可以通过选项（` options `）自定义**
loader 实现的目标
- 识别出应该被对应的 loader 进行转换的那些文件（使用 ` text ` 属性）
- 转换这些文件，使其能被添加到依赖图中（使用 ` use ` 属性）
```
webpack.config.js
module.exports = {
    entry: './path/to/my/entry/file.js',
    output: {
        path: path.resolve( __dirname, 'dist' )
        filename: 'my-first-webpack-bundle.js'
    },
    module: {
        rules: [
            {
                // 遇到匹配的文件，使用 loader 进行转换之后再进行打包
                text: /\.txt$/,        // 匹配的文件
                use: 'raw-loader'    // 使用的 loader
                use: {
                    loader: 'babel-loader',
                    options: {
                        presets: [ '@babel/present-env' ]
                    }
                }
            }
        ]
    }
}
```

###### 插件（plugins）
loader 被用于转换某些类型的模块，而插件则用于执行范围更广的任务。插件的范围包括：从打包优化 + 压缩，一直到重新定义环境中的变量
想使用一个插件，需要 ` require() ` 插件，然后把它添加到 ` plugins ` 数组中，多数插件可以通过选项 ` options ` 自定义
```
webpack.config.js
const HtmlWebpackPlugin = require('html-webpack-plugin'); // 通过 npm 安装
const webpack = require('webpack'); // 用于访问内置插件
const path = require('path');

const config = {
  entry: './path/to/my/entry/file.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'my-first-webpack.bundle.js'
  },
  module: {
    rules: [
      { test: /\.txt$/, use: 'raw-loader' }
    ]
  },
  plugins: [
    new webpack.optimize.UglifyJsPlugin(),
    new HtmlWebpackPlugin({template: './src/index.html'})
  ]
};

module.exports = config;
```

#### webpack 使用
**注意：` npm i 找不到的module `**
```
mkdir webpack-demo
cd webpack-demo
npm init    // 初始化
一路回车
ls   ==> package.json
npm install --save-dev webpack  // package.json 中多了依赖 webpack
touch webpack.config.js   // 创建配置文件并编辑
vi webpack.config.js
    ****************
    const path = require('path');  
    module.exports = {
      entry: './src/js/',
      output: {
        filename: 'bundle.js',
        path: path.resolve(__dirname, 'dist/js/')
      }
    };
    *****************

mkdir src src/css src/js 
touch src/css/main.scss src/js/index.js src/js/main.js src/index.html  

    ****************
    index.html 中引入 css，不引 scss
    <link rel="stylesheet" href="./css/main.css">
    ***************
npx webpack  // 将 src 中的 JS 文件转换 + 复制到 dist/js/
```

#### 使用 [babel-loder](https://github.com/babel/babel-loader/tree/7.x)
```
index.js + main.js

    ***********************
    let a = 1
    console.log(a)
    
    let b = 1
    console.log(b)
    **********************

npm install --save-dev babel-loader babel-core babel-preset-env webpack  
// 使用 webpack 3.x babel-loader 7.x | babel 6.x，注意 github 点击[链接](https://github.com/babel/babel-loader/tree/7.x)
vi webpack.config.js

    **************************
    output: {
        filename: 'bundle.js',
        path: path.resolve(__dirname, 'dist/js/')
    },
    + module: {
    +   rules: [
    +     {
    +       test: /\.js$/,    // 如果文件是 .js 结尾
    +       exclude: /(node_modules|bower_components)/,
    +       use: {             // 使用 babel-loader 转换
    +         loader: 'babel-loader',
    +       options: {       // 选项，参数 env
    +           presets: ['env']
    +         }
    +       }
    +     }
    +   ]
    + }
    ***************************

npx webpack
```

#### 模块化
```
touch src/js/app.js
// app.js 调用 main.js + index.js
vi app.js

    ***************************
    import a from './main'
    import b from './index'
    
    a()
    console.log('app')
    b()
    **************************

index.js

    ********************************
    let b = 1
    function fn() {
        console.log(b)
    }
    
    export default fn
    *******************************

main.js

    **********************************
    export default function(){
        let a =1
        console.log(a)
    }
    ***********************************

webpack.config.js

    ********************************
    module.exports = {
    -    entry: './src/js/',
    +    entry: './src/js/app.js',
    }
    ********************************

rm -rf dist
npx webpack  // 看终端 log 便可以看到，webpack 将三个 js 文件都转换并复制到了 dist/js 中
```

#### 使用 [scss-loader](https://github.com/webpack-contrib/sass-loader)
```
npm install sass-loader node-sass webpack --save-dev
vi webpack.config.js

    ********************************
    module: {
        rules: [
            {
                test: /\.js$/,
                exclude: /(node_modules|bower_components)/,
                use: {
                    loader: 'babel-loader',
                    options: {
                        presets: ['env']
                    }
                }
        -      }
        +      },
        +    {
        +        test: /\.scss$/,   // 1. 发现后缀 .scss 文件
        +        use: [
        +            {
        +                loader: "style-loader" // 4. 使用 style-loader 将 JS 字符串 ==> <style> 标签（creates style nodes from JS strings）
        +            }, {
        +                loader: "css-loader" // 3. 使用 css-loader 将 css ==> JS 字符串（translates CSS into CommonJS）
        +            }, {
        +                loader: "sass-loader" // 2. 使用 sass-loader 将 sass ==> css (compiles Sass to CSS）
        +            }
        +        ]
        +    }
        ]
    }
    *********************************

vi app.js

    **************************
    + import '../css/main.scss'
    **************************

npx webpack
```
#### 使用 [postcss-loader](https://github.com/postcss/postcss-loader)
postcss-loader 能够使得 CSS 自动添加前缀，从而使我们的代码更加强健
```
npm i -D postcss-loader
touch postcss.config.js
vi postcss.config.js

    *****************************
    module.exports = {
      //parser: 'sugarss',
      plugins: {
        'postcss-import': {},
       // 'postcss-cssnext': {},
       // 'cssnano': {}
      }
    }
    *******************************

vi webpack.config.js

    ************************
    ......
            {
                test: /\.scss$/,
                use: [
                    {
                        loader: "style-loader" // creates style nodes from JS strings
                    }, {
                        loader: "css-loader", // translates CSS into CommonJS
    +                  options: { importLoaders: 1 }
    +              }, {
    +                  loader: 'postcss-loader',
    +                  options: {                     // 如果没有 options 则不会添加后缀，先尝试不添加 options，如果 postcss-loader 不工作，再添加 options
    +                      plugins: () => [
    +                          require('autoprefixer')
    +                      ]
    +                  }
                    },{
                        loader: "sass-loader" // compiles Sass to CSS
                    }
                ]
            }
    ......
    ***********************

npx webpack
```

#### webpack 总结
- webpack 将前端思想完全转变，将 CSS 写入 JS 中，使用 JS 生成 <style> 标签，这样使得 JS 执行的时候才能加载 CSS，这是一个弊端，可以使用 Cache Control 缓解，即当我们第二次访问的时候，会很快，因为一个 JS 包含了所有问题，可以使用混杂模式，既有传统 CSS，又有 webpack CSS，在 <head> 标签中加入 <style> 基础样式，使用 JS 添加动态样式
- webpack 就是做一系列自动化的流程，例如，通过 babe-loader 将 ES6 转化为 ES5，通过 scss-loader 将 scss 转化为 css，通过 postcss-loader 添加 css 前缀。webpack 是一种基于预编译的模块化方案，把一切内容包括 JS 文件和静态文件都视为模块（module）
