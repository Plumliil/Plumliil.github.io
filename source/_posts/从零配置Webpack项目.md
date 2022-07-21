---
title: 从零配置Webpack项目
tags:
  - 前端篇
  - Webpack
date: 2022-06-09 16:23:42
---




#### webpack.config.js基本配置

webpack.config.js是webpack的配置文件,在此文件中对项目入口,项目的输出,loader,插件以及环境等进行简单的配置

首先来对webpack的五个核心概念进行介绍

- entry:入口,说明了项目以哪个文件为入口开始对项目进行打包,分析构建内部依赖图,
  - main:单个入口语法 可以简写为 entry:''
  - pageOne,pageTwo... 告诉我们webpack需要多个独立分离依赖图,多页面应用但是根据经验,每个 HTML 文档只使用一个入口起点。
- output:输出 通过配置此项可以规定webpack打包后资源输出到哪里以及如何命名
  - filename:用于输出文件的文件名
  - path:目标输出目录的绝对路径,通常与path模块连用
- loader:通过loader可以让webpack去处理非javascript资源(webpack本身只能处理javascript)
  - test:使用loader匹配的文件
  - use:使用多个loader,以数组的形式出现
  - loader:当处理某一类型文件只需要一种loader时可以使用
  - options:对loader进行一些配置
- plugins:插件,通过插件,可以增加webpack打包能力.插件的范围包括从打包优化到压缩,一直到重新定义变量
- mode:模式mode指示webpack执行相应的模式配置,常见模式为`development`,`production`

webpack构建工具是基于node.js平台运行,模块化默认采用commonjs

基本配置如下:

~~~javascript
const {resolve}=require('path');
module.exports={
    entry:'./src/index.js',
    output:{
        filename:'built.js',
        path:resolve(__dirname,'build')
    },
    module:{
        rules:[
            // 详细的loader配置
        ]
    },
    plugins:[
        // 详细的plugin配置
    ],
    mode:'development'
}
~~~

#### 常见的loader

##### html

- html-loader:处理html文件的img图片 (负责引入img图片,从而能被url-loader处理)

##### 样式

- style-loader:创建一个style标签,将js中的css样式资插入,添加到页面中生效
- css-loader:将css文件以字符串形式变成commonjs模加载到js中,里面内容是
- sass-loader:处理sass/scss文件
- lass-loader:处理lass文件
- postcss-loader:对css文件进行兼容性处理

##### 其他资源

- url-loader:协助打包资源文件,可以将图片通过base64方法引入到代码中,url-loader依赖file-loader,所以这两个loader都需要下载
- file-loader:对图片视频音频等资源进行打包

#### 常见plugin

- HtmlWebpackPlugin:当使用 webpack打包时，创建一个 html文件，并把webpack打包后的静态文件自动插入到这个 html文件当中。
- MiniCssExtractPlugin:将css样式提取到单独样式中,可以在插件中设置提取到的文件目录.





#### 资源打包

资源打包依赖于各种loader和plugin,通过loader和plugin的配合最终打包出需要的文件.

##### 样式资源打包

根据样式选择加载器style-loader,css-loader必选less-loader,sass-loader,

~~~javascript
module: {
    rules: [
        // less
        {
            test: /\.less$/,
            use: ['style-loader', 'css-loader', 'less-loader'],
        },
        // css
        // 样式文件和js文件一起
        {
            test: /\.css$/,
            use: ['style-loader', 'css-loader'],
        },
    ],
}
~~~

##### html文件打包 

用到了html-loader处理html文件,同时用到了插件html-webpack-plugin,部分代码如下

~~~javascript
const HtmlWebpackPlugin = require('html-webpack-plugin')
// export default
module: {
  {
    test: /\.html$/,
      // 处理html文件的img图片 (负责引入img图片,从而能被url-loader处理)
    loader: 'html-loader',
  },
},
plugins: [
  new HtmlWebpackPlugin({
    template: './src/index.html'
  })
],
~~~

##### 图片文件打包

主要用到了url-loader,但是url-loader依赖file-loader,所以这两个loader都需要下载

在options进行了配置

- limit:限制多大以下的图片进行base64打包

- name:定义图片的名字,可以调整hash值的长度

- esModule:是否按照es6模块化打包,默认为true

- outputPath:打包后的文件放置路径

~~~javascript
module:{
    rules:[
        // img
        {
            test: /\.(jpg|png|gif|jpeg)$/,
            loader: 'url-loader',
            options: {
                limit: 8 * 1024,
                name: '[hash:10].[ext]',
                // 关闭es6模块化
                esModule: false,
                outputPath:'imgs'
            }
        },
    ]
},
~~~

##### 其他资源打包

主要用到了file-loader

~~~javascript
module:{
    rules:[
        {
            exclude: /\.(html|css|jpg|jpeg|png|gif)$/,
            loader: 'file-loader',
            options: {
                name: '[hash:10].[ext]',
                outputMedia:'media'
            }
        }
    ]
},
~~~

#### devServer

开发服务器:devServer

~~~javascript
devServer: {
    // 项目路径
    static: {
        directory: resolve(__dirname, 'build'),
    },
    // 启动gzip压缩
    compress: true,
    // 端口号
    port: 3000,
    // 自动打开浏览器
    open:true
}
~~~

- static:该配置项允许配置从目录提供静态文件的选项（默认是 'public' 文件夹）。将其设置为 false 以禁用：

- compress:是否启动gzip压缩,默认为false

- port:执行的端口

- open:是否自动打开浏览器,默认为false

- [...][https://webpack.docschina.org/configuration/dev-server/#devserverstatic]

#### 开发环境配置

##### css样式提取

提取css样式时下载了`MiniCssExtractPlugin`插件来进行提取,并且将`style-loader`换成`MiniCssExtractPlugin.loader`,`style-loader`主要作用是创建style标签,将`css-loader`提取的样式放在style标签中,`MiniCssExtractPlugin.loader`作用是将css样式提取到单独样式中,可以在插件中设置提取到的文件目录.`MiniCssExtractPlugin.loader`取代`style-loader` 提取 css 样式为单独文件代码如下

~~~javascript
const {
    resolve
} = require('path');

const HtmlWebpackPlugin = require('html-webpack-plugin');
const MiniCssExtractPlugin = require('mini-css-extract-plugin');

module.exports = {
    entry: './src/js/index.js',
    output: {
        filename: 'js/built.js',
        path: resolve(__dirname, 'build')
    },
    module: {
        rules: [{
            test: /\.css$/,
            use: [
                MiniCssExtractPlugin.loader,
                'css-loader'
            ]
        }]
    },
    plugins: [
        new HtmlWebpackPlugin({
            template: './src/index.html'
        }),
        new MiniCssExtractPlugin({
            filename:'css/built.css'
        })
    ],
    mode: 'development'
}
~~~

##### css兼容性处理

主要包:postcss-loader,mini-css-extract-plugin

postcss-preset-env 帮助postcss找到package.json中browserslist里面配置

~~~shell
npm install postcss-loader postcss-preset-env -D
~~~

~~~shell
npm install mini-css-extract-plugin -D

~~~

  通过配置加载指定的css兼容性样式

  package.json中配置

   ~~~JavaScript
"browserslist": {
  开发环境设置node环境变量: process.env.NODE_ENV = development
  "development": [
    "last 1 chrome version",
    "last 1 firefox version",
    "last 1 safari version",
  ],
    浏览器兼容处理
  "production": [
    ">0.2",
    "not dead",
    "not op_mini all"
  ]
}
   ~~~







~~~javascript
const {
    resolve
} = require('path');

const HtmlWebpackPlugin = require('html-webpack-plugin');
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
// 设置nodejs环境变量
process.env.NODE_ENV = 'development';

module.exports = {
    entry: './src/js/index.js',
    output: {
        filename: 'js/built.js',
        path: resolve(__dirname, 'build')
    },
    module: {
        rules: [{
            test: /\.css$/,
            use: [
                MiniCssExtractPlugin.loader,
                'css-loader',
                {
                    loader: 'postcss-loader',
                    options: {
                        postcssOptions: {
                            ident: 'postcss',
                            plugins: () => [
                                // postcss插件
                                require('postcss-preset-env')()
                            ]
                        }
                    }
                }
            ]
        }]
    },
    plugins: [
        new HtmlWebpackPlugin({
            template: './src/index.html'
        }),
        new MiniCssExtractPlugin({
            filename: 'css/built.css'
        })
    ],
    mode: 'development'
}
~~~

##### css压缩

`optimize-css-assets-webpack-plugin`

webpack 5 以上 `css-minimizer-webpack-plugin`

~~~javascript
...
const CssMinimizerWebpackPlugin=require('css-minimizer-webpack-plugin');
...

plugins: [
        ...
        new CssMinimizerWebpackPlugin()
    ],
...
~~~

##### js语法检查

`eslint-loader` `eslint`

注意:只检查自己的编写的源代码,所引用第三方库不需要进行检查

检查规则的设置:

~~~javascript
...
    module:{
        rules:[
            {
               test:/\.js$/,
               exclude:/node_modules/,
               loader:'eslint-loader',
               options:{
                // 自动修复
                fix:true
               }
            }
        ]
    },
...
~~~

风格指南:`airbnb`

所依赖库:`eslint-config-airbnb-base` `eslint-plugin-import` `eslint`



packag.json中所需配置文件

~~~javascript
 "eslintConfig": {
  "extends": "airbnb-base"
 }
~~~



当某一行不需要进行语法检查时可以使用 `eslint-disable-next-line` 禁用下一行代码 

##### js兼容性处理

将es6及以上代码转换为es5代码



依赖包:babel-loader @babel/core @babel/preset-env

1.基本处理:@babel/preset-env

只能转换基本语法,例如promise无法处理

2.全部js兼容处理:@babel/polyfill

问题:只要解决部分兼容性问题,将所有兼容性代码引入1,代码体积太大

3.需要做兼容性处理的就按需加载 corejs

具体配置:

~~~javascript
module: {
        rules: [
            {
                test: /\.js$/,
                exclude: /node_modules/,
                loader: 'babel-loader',
                options: {
                    // 预设:指示babel做怎么样的处理
                    presets: [
                        ["@babel/preset-env", {
                            "useBuiltIns": "usage",
                            "corejs": {
                              "version": 3
                            },
                            "targets": {
                              "chrome": "60",
                              "firefox": "60",
                              "ie": "9",
                              "safari": "10",
                              "edge": "17"
                            }
                          }]
                    ]
                }
            }
        ]
    },
~~~

##### js和html代码压缩

js压缩:将development改为production UglifyJsPlugin

~~~javascript
...
plugins: [
  new htmlWebpackPlugin({
    template: './src/index.html'
            minify: {
      // 移除空格
      colllapseWhitespace: true,
      // 移除注释
      removeComments: true
    }
  })
],
mode: 'production'
...
~~~

#### 生产环境webpack配置

webpack.config.js

~~~javascript
const {
    resolve
} = require('path');
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
const CssMinimizerWebpackPlugin = require('css-minimizer-webpack-plugin'); // css压缩
const HtmlWebpackPlugin=require('html-webpack-plugin');
// 定义nodejs变量,决定使用browserslist哪个环境
process.env.NODE_ENV = 'production';
// 复用loader
const commonCssLoade = [
    MiniCssExtractPlugin.loader,
    'css-loader',
    {
        // css兼容性处理 还需在package中定义browserslist
        loader: 'postcss-loader',
        options: {
            postcssOptions: {
                ident: 'postcss',
                plugins: () => [
                    // postcss插件
                    require('postcss-preset-env')()
                ]
            }
        }
    }
]

module.exports = {
    entry: './src/js/index.js',
    output: {
        filename: 'js/built.js',
        path: resolve(__dirname, 'build')
    },
    module: {
        rules: [{
                test: /\.css$/,
                use: [...commonCssLoade]
            },
            {
                test: /\.less$/,
                use: [...commonCssLoade, 'less-loader']
            },
            /* 
                一个文件被多个loader处理,那么一定要指定loader执行先后顺序:
                    先执行 eslint 再执行 babel
            */
            {
                test: /\.js$/,
                exclude: /node_modules/,
                loader: 'eslint-loader',
                enforce:'pre',
                options: {
                    fix: true
                }
            },
            {
                test: /\.js$/,
                exclude: /node_modules/,
                loader: 'babel-loader',
                options: {
                    presets: [
                        ["@babel/preset-env", {
                            "useBuiltIns": "usage",
                            "corejs": {
                                "version": 3
                            },
                            "targets": {
                                "chrome": "60",
                                "firefox": "60",
                                "ie": "9",
                                "safari": "10",
                                "edge": "17"
                            }
                        }]
                    ]
                }
            },
            {
                test:/\.(jpg|png|gif)/,
                loader:'url-loader',
                options:{
                    limit:8*1024,
                    name:'[hash:10].[ext]',
                    outputPath:'imgs',
                    esModule:false
                }
            },
            {
                test:/\.html$/,
                loader:'html-loader',
                minify:{
                    collapseWhitespace:true,
                    removeComments:true
                }
            },
            {
                exclude:/\.(js|css|html|jpg|png|gif)/,
                loader:'file-loader',
                options:{
                    outputPath:'media'
                }
            }
        ]
    },
    plugins: [
        new MiniCssExtractPlugin({
            filename: 'css/built.css'
        }),
        new CssMinimizerWebpackPlugin(),
        new HtmlWebpackPlugin({
            template:'./src/index.html'
        })
    ],
    mode: 'production'
}
~~~



package.json

~~~javascript
"devDependencies": {
    "@babel/polyfill": "^7.12.1",
    "@babel/preset-env": "^7.17.12",
    "babel-loader": "^8.2.5",
    "core-js": "^3.22.5",
    "css-loader": "^6.7.1",
    "css-minimizer-webpack-plugin": "^3.4.1",
    "eslint": "^8.15.0",
    "eslint-config-airbnb": "^19.0.4",
    "eslint-loader": "^4.0.2",
    "eslint-plugin-import": "^2.26.0",
    "file-loader": "^6.2.0",
    "html-webpack-plugin": "^5.5.0",
    "img-loader": "^4.0.0",
    "less-loader": "^10.2.0",
    "mini-css-extract-plugin": "^2.6.0",
    "postcss-loader": "^6.2.1",
    "postcss-preset-env": "^7.5.0",
    "style-loader": "^3.3.1",
    "url-loader": "^4.1.1",
    "webpack": "^5.72.0",
    "webpack-cli": "^4.9.2",
    "webpack-dev-server": "^4.8.1"
  },
  "dependencies": {
    "html-loader": "^3.1.0"
  },
  "browserslist": {
    "development": [
      "last 1 chrome version",
      "last 1 firefox version",
      "last 1 safari version"
    ],
    "production": [
      ">0.2%",
      "not dead",
      "not op_mini all"
    ]
  },
  "eslintConfig": {
    "extends":"airbnb-base"
  }
~~~

