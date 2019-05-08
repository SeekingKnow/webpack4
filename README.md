# hello-webpack 4

webpack4 各种语法 入门讲解

- [【慕课网 Webpack4.0】从基础到实战 手把手带你掌握新版Webpack4.0完整](https://coding.imooc.com/class/316.html)

看视频整理要点笔记：

----

**目录**
- [第1章 为什么会出现webpack?](#第1章-为什么会出现webpack)
- [第2章 webpack究竟是什么？](#第2章-webpack究竟是什么)
    - [2-3 Webpack 的正确安装方式](#2-3-Webpack-的正确安装方式)
- [第3章 Webpack 的核心概念](#第3章-Webpack-的核心概念)
    - [3-1 什么是loader](#3-1-什么是loader)
    - [3-2 使用 Loader 打包静态资源（图片篇）](#3-2-使用-Loader-打包静态资源图片篇)
    - [3-3 使用 loader 打包静态资源（样式篇 - 上）【css、scss、私有前缀】](#3-3-使用-loader-打包静态资源样式篇---上)
    - [3-4 使用 loader 打包静态资源（样式篇 - 下）【样式中是sass、样式的局部作用域、字体文件】](#3-4-使用-loader-打包静态资源样式篇-下)
    - [3-5 使用 ```plugins``` 让打包更便捷【```HtmlWebpackPlugin```、```CleanWebpackPlugin```】](#3-5-使用-plugins-让打包更便捷)
    - [3-6 Entry 与 Output 的基础配置](#3-6-Entry-与-Output-的基础配置)
    - [3-7 SourceMap 的配置](#3-7-SourceMap-的配置)
    - [3-8 使用 ```WebpackDevServer``` 提升开发效率](#3-8-使用-WebpackDevServer-提升开发效率)
    - [3-9 ```Hot Module Replacement``` 模块热更新](#3-9-Hot-Module-Replacement-模块热更新)
    - [3-11 使用 ```Babel``` 处理 ES6 语法](#3-11-使用-Babel-处理-ES6-语法)
    - [3-13 Webpack 实现对React框架代码的打包](#3-13-Webpack-实现对React框架代码的打包)
    - [本章总结 最佳配置](#第3章-Webpack-的核心概念总结-最佳配置)
- [第4章 Webpack 的高级概念](##第4章-webpack-的高级概念)
    - [4-1 Tree Shaking 概念详解](#4-1-tree-shaking-概念详解)
    - [4-2 ```Development``` 和 ```Production``` 模式的区分打包](#4-2-development-和-production-模式的区分打包)
    - [4-3 ```Code Splitting``` 代码分割](#4-3-Code-Splitting-代码分割)
    - [4-5 ```Split Chunks Plugin``` 配置参数详解](#4-5-split-chunks-plugin-配置参数详解)
    - [4-6 ```SplitChunksPlugin``` 参数详解](#4-6splitchunksplugin-参数详解)
    - [4-7 ```Lazy Loading``` 懒加载，```Chunk``` 是什么？](##4-7-lazy-loading-懒加载chunk-是什么)
    - [4-8 ```Bundle Analysis``` 打包分析，```Prefetching``` 预取，```Preloading``` 预加载](#4-8-bundle-analysis-打包分析prefetching-预取preloading-预加载)
    - [4-9 Css 文件的代码分割，打包独立的css文件](#4-9-css-文件的代码分割打包独立的css文件)
    - [4-10 Webpack 与浏览器缓存（Caching）](#4-10-webpack-与浏览器缓存caching)
    - [4-11 Shimming 垫片 的作用](#4-11-shimming-垫片-的作用)
    - [4-12 环境变量的使用方法](#4-12-环境变量的使用方法)
- [第5章 Webpack 实战配置案例讲解](#)
    - [5-1 Library 的打包]()

----

## 第1章 为什么会出现webpack?

- 发展到现在，前端项目越来越复杂，业务逻辑越来越多，已经不是仅仅几个 html js css 文件能处理得了的。即使处理的了，可维护性也很差。

## 第2章 webpack究竟是什么？

- 2-2 webpack 是什么？
    - 模块打包工具
    - 打包命令： 
        - ```npx webpack index.js```  (局部安装)
        - ```webpack input.js output.js``` (全局安装)
    
- ### 2-3 Webpack 的正确安装方式
    - 环境依赖 node.js , npm
    - 项目初始化：```npm init -y```
        - 要想用webpack管理项目，要先让项目符合node的规范
        - 生成 package.json
    - 安装命令
        - ```npm i webpack webpack-cli -D```                  (简写)
        - ```npm install webpack webpack-cli --save-dev```    (全写)
        - 全局安装 存在的问题
            - ```npm install webpack webpack-cli -g```
            - ```npm uninstall webpack webpack-cli -g```      (卸载全局安装)
            - 非常不推荐全局安装 webpack
            - 原因：假如我有两个项目，一个是webpack4打包的，另一个是webpack3打包的。由于我是全局安装webpack4的，那么webpack3打包的项目是运行不起来的
        - webpack-cli 的作用
            - webpack-cli 使得 webpack index.js 或者 npx webpack 这样的命令能够在命令行中运行
    - 安装后检查是否安装成功
        - ```npx webpack -v```    检查局部安装
        - ```webpack -v```        检查全局安装
    - Webpack 文件上传github仓库
        - 一般情况下，都会把项目中的 node_modules文件夹 删除
         - ```npm install``` 可以自动把项目依赖包下载好
- 2-4 使用webpack的配置文件
    - ```npx webpack index.js```
        - 这条命令，实际上是使用webpack的默认配置，且入口文件是index.js
    - webpack.config.js 配置文件
        - 当配置文件 配置好后，打包命令为：npx webpack
        ```js
            const path = require('path')

            module.exports = {
                mode: 'production', // development
                entry: {
                    main: './src/index.js'
                },
                // entry: './src/index.js', 简写
                output: {
                    filename: 'bundle.js',
                    path: path.resolve(__dirname, 'dist')
                }
            }
        ```
    - 目录结构
        ```
        webpack-demo
        +  |- node_modules
        +  |- /dist               // 打包出口
        +  |- /src                // 源代码(入口)
               |- index.js
           |- webpack.config.js   // 配置文件
           |- package.json
           |- index.html
        ```
    - npm run bundle 打包命令
        - 问题：如何自定义打包命令的名称呢？例如把打包命令改成```npm run bundle```，而不是```npx webpack```
        - 在 package.json 文件中
            ```js
            ...
            "scripts": {
                "bundle": "webpack"
            }
            ...
            ```
            npm script 中的'webpack'会优先到本地(局部)的node_modules中查找webpack模块，如果没有才会去全局中查找
    - 手动指定配置文件
        - ```npx webpack --config config_file_name.js```

## 第3章 Webpack 的核心概念
- ### 3-1 什么是loader
    - 什么是 loader ?
        - 由于webpack原来只能打包js文件，但是如果要打包css或jpg/png的文件时，webpack就会不支持了。这时候就要借助loader来解决这个问题。
    - loader三步曲
        - 1. js引入一个非js文件，如：.jpg .png .txt .vue excel表格文件等
        - 2. 配置好对应的 ```webpack.config.js``` loader
        - 3. 在webpack中 安装好对应的loader ```npm i file-loader -D```
    - ```webpack.config.js``` 配置loader
        ```js
        const path = require('path')

        module.exports = {
            mode: 'development',
            entry: {
                main: './src/index.js'
            },
            module:{
                rules: [{
                    test: /\.jpg$/,
                    use: {
                        loader: 'file-loader'
                    }
                }]
            }
            output:{
                filename: 'bundle.js',
                path: path.resolve(__dirname, 'dist')
            }
        }
        ```
- ### 3-2 使用 Loader 打包静态资源(图片篇)
    - file-loader
        ```js
        const path = require('path')

        module.exports = {
            mode: 'development',
            entry: {
                main: './src/index.js'
            },
            module: {
                rules: [{
                    test: /\.(jpg|png|gif)$/,
                    use: {
                        loader: 'file-loader',
                        options: {
                            name: '[name].[hash].[ext]',
                            // [name] 这种语法为 placeholder 占位符
                            outputPath: 'img/'
                        }
                    }
                }]
            },
            output: {
                filename: 'bundle.js',
                path: path.resolve(__dirname, 'dist')
            }
        }
        ```
    - url-loader
        - url-loader能做一切 file-loader所能做的一切
            - url-loader 和 file-loader 非常像，只不过url-loader 多了一个limit的配置项
        - 通过 url-loader 打包完后，你会发现，图片文件并没有被打包到dist目录下
            - 其实，url-loader 默认会把图片文件转成 base64 字符串，写入了 bundle.js 文件中
                - 但是这样打包又引入了一些问题。
                    - 优点：图片打包到js里面，只要js加载完成，页面就显示出来了，不用再去额外的请求图片的地址了，省了一次http请求
                    - 缺点：如果图片本身特别大，打包生成的js文件也就会特别大，那么你加载这个js文件的时间就会很长。所以在一开始，很长的时间里面，页面上什么东西都显示不出来
                    - 最佳的使用方式：如果一个图片很小，只有几kb，那么把这些图片打包到js里，是非常好的选择，没必要让几kb的图片 再去发一次http请求，很浪费时间。反之，如果图片很大的话，就把图片打包到dist目录下。
                        - 实现：```limit: 2048``` 限制2048个字节(2kb), 大于2048则打包成图片 放入dist目录，否则打包成base64
        ```js
        const path = require('path')

        module.exports = {
            mode: 'development',
            entry:{
                main: './src/index.js'
            },
            module: {
                relus:[{
                    test: /\.(jpg|png|gif)$/,
                    use: {
                        loader: 'url-loader',
                        option: {
                            name: '[name].[hash].[ext]',
                            outputPath: 'img/',
                            limit: 2048     // 限制2048个字节
                        }
                    }
                }]
            },
            output: {
                filename: 'bundle.js',
                path: path.resolve(__dirname, 'dist')
            }
        }
        ```
- ### 3-3 使用 loader 打包静态资源(样式篇 - 上)
    - 1.处理css文件
        - ```npm i style-loader css-loader -D``` 安装css-loader
        - webpack.config.js
            ```js
            const path = require('path')

            module.exports = {
                mode: 'development',
                entry: {
                    main: './src/index.js'
                },
                module: {
                    rules: [{
                        test: /\.(jpg|png|gif)$/,
                        use: {
                            loader: 'file-loader',
                            options: {
                                name: '[name].[hash].[ext]',
                                outputPath: 'img/'
                            }
                        }
                    },{
                        test: /\.css$/,
                        use: ['style-loader','css-loader']
                    }]
                },
                output: {
                    filename: 'bundle.js',
                    path: path.resolve(__dirname, 'dist')
                }
            }
            ```
    - 2.处理scss文件
        - ```npm i sass-loader node-sass -D``` 安装sass-loader
        - webpack.config.js
            ```js
            const path = require('path')

            module.exports = {
                mode: 'development',
                entry: {
                    main: './src/index.js'
                },
                module: {
                    rules: [{
                        test: /\.(jpg|png|gif)$/,
                        use: {
                            loader: 'file-loader',
                            options: {
                                name: '[name].[hash].[ext]',
                                outputPath: 'img/'
                            }
                        }
                    },{
                        test: /\.scss$/,
                        use: ['style-loader','css-loader','sass-loader']
                    }]
                },
                output: {
                    filename: 'bundle.js',
                    path: path.resolve(__dirname, 'dist')
                }
            }
            ```
    - 3.postcss-loader自动添加 浏览器私有前缀
        - ```npm i -D postcss-loader``` 安装 postcss-loader
        - ```npm i -D autoprefixer``` 安装自动添加 浏览器私有前缀 插件
        - 新建 ```postcss.config.js``` 配置文件，放在 ```webpack.config.js``` 同级目录下
            ```js
            // postcss.config.js
            module.exports = {
                plugins: [
                    require('autoprefixer')
                ]
            }
            ```
            
            ```js
            // webpack.config.js
            const path = require('path')

            module.exports = {
                mode: 'development',
                entry: {
                    main: './src/index.js'
                },
                module: {
                    rules: [{
                        test: /\.(jpg|png|gif)$/,
                        use: {
                            loader: 'file-loader',
                            options: {
                                name: '[name].[hash].[ext]',
                                outputPath: 'img/'
                            }
                        }
                    },{
                        test: /\.scss$/,
                        use: [
                            'style-loader',
                            'css-loader',
                            'sass-loader',
                            'postcss-loader'
                        ]
                    }]
                },
                output: {
                    filename: 'bundle.js',
                    path: path.resolve(__dirname, 'dist')
                }
            }
            ```

- ### 3-4 使用 loader 打包静态资源(样式篇-下)
    - ### 1.样式loader 配置项
        - 现在有一个问题：先看下面项目文件
            ```js
            // index.js
            import './index.scss'

            // ...other js code
            ```
            ```scss
            // index.scss
            @import './avatar.scss';

            body{
                .avatar{ width: 150px }
            }
            ```
            ```scss
            // avatar.scss
            .avatar{ color: red }
            ```
            ```js
            // webpack.config.js
            const path = require('path')

            module.exports = {
                mode: 'development',
                entry: './src/index.js',
                module: {
                    rules: [{
                        test: /\.scss$/,
                        use: [
                            'style-loader',
                            'css-loader',
                            'sass-loader',
                            'postcss-loader'
                        ]
                    }]
                },
                output: {
                    filename: 'bundle.js',
                    path: path.resolve(__dirname, 'dist')
                }
            }
            ```
        - 问题：
            - 1.在入口文件index.js中，引入的.scss文件，webpack在处理这类文件的时候都会依次```由后向前```去走 'postcss-loader',  'sass-loader', 'css-loader', 'style-loader'
            - 2.但是问题来了，在sass文件中，@import其他 .scss 文件。当webpack处理文件由 'postcss-loader',  'sass-loader' 走到'css-loader', 的时候，遇到了 ```@import './avatar.scss';``` 它就不知道该如何解析这个@import进来的scss文件了，然后他就会直接把它按照css文件处理了，而不会解析里面scss语法
            - 所以，解决方案：给 ```css-loader``` 添加配置项 ```importLoaders``` 
            ```js
            // webpack.config.js
            const path = require('path')

            module.exports = {
                mode: 'development',
                entry: './src/index.js',
                module: {
                    rules: [{
                        test: /\.scss$/,
                        use: [
                            'style-loader',
                            {
                                loader: 'css-loader',
                                options: {
                                    importLoaders: 2
                                    // 在css文件中遇到 @import，会往前执行2个loader
                                }
                            },
                            'sass-loader',
                            'postcss-loader'
                        ]
                    }]
                },
                output: {
                    filename: 'bundle.js',
                    path: path.resolve(__dirname, 'dist')
                }
            }
            ```
    - ### 2.样式的模块化 （样式的局部作用域）
        - 过去存在的问题：
            - 在一个页面中引入 index.css
            ```css
                .avator{ width: 150px }
            ```
            这种情况下，对于整个页面来说 .avator 的 **样式的作用域是全局的** （对页面中所有的.avator class都有影响）。但是如果是多人开发，或组件式开发的项目，这样就很容易引起 **样式冲突** 的问题。

            那么，有没有什么办法能实现 **样式的局部作用域** 呢？
        - #### 样式的局部作用域
            - **思路：** 
                - ```modules: true```   开启css模块化
                - ```let style = require('./css/index.scss')```     引入的scss赋值给变量 style
                - ```img.classList.add(style.avator)```     添加class时用 style.avator
            - 这样就能实现， index.scss 只对 其中指定的class有效 (**即使class同名**)
        
            ```js
            // webpack.config.js
            const path = require('path')

            module.exports = {
                mode: 'development',
                entry: './src/index.js',
                module: {
                    rules: [{
                        test: /\.scss$/,
                        use: [
                            'style-loader',
                            {
                                loader: 'css-loader',
                                options: {
                                    importLoaders: 2,
                                    modules: true   // 开启css模块化
                                }
                            },
                            'sass-loader',
                            'postcss-loader'
                        ]
                    }]
                },
                output: {
                    filename: 'bundle.js',
                    path: path.resolve(__dirname, 'dist')
                }
            }
            ```

            ```js
            // index.js
            let img_src = require('./img/webpack.png')
            let style = require('./css/index.scss')     // 引入的scss赋值给变量 style
            let createAvator = require('./js/avator.js')
            createAvator()

            let dom = document.getElementById('dom')

            let img = new Image()
            img.src = `./dist/${img_src}`
            img.classList.add(style.avator)     // 添加class时用 style.avator

            dom.append(img)
            ```

            ```js
            // ./js/avator.js
            let img_src = require('../img/webpack.png')

            function createAvator(){
                let dom = document.getElementById('dom')

                let img = new Image()
                img.src = `./dist/${img_src}`
                img.classList.add('avator')

                dom.append(img)
            }

            module.exports = createAvator
            ```
    - #### 3.打包字体文件
        - 思路：通过 ```file-loader``` 把字体文件打包到```dist```目录下
        - 工程文件如下：
        ```
        项目目录
        +  |- node_modules
        +  |- /src
        +     |- /css
                 |- index.scss
        +     |- /fonts
                 |- iconfont.eot
                 |- iconfont.svg
                 |- iconfont.ttf
                 |- iconfont.woff
              |- index.js
           |- index.html
           |- package.json
           |- postcss.config.js
           |- webpack.config.js           
        ```
        ```js
        // webpack.config.js
        const path = require('path')

        module.exports = {
            mode: 'development',
            entry: {
                main: './src/index.js'
            },
            module: {
                rules: [{
                    test: /\.scss$/,
                    use: [
                        'style-loader',
                        {
                            loader: 'css-loader',
                            options: {
                                importLoaders: 2
                            }
                        },
                        'sass-loader',
                        'postcss-loader'
                    ]
                },{
                    test: /\.(eot|svg|ttf|woff|woff2)$/,
                    use: {
                        loader: 'file-loader',
                        // 此处用 'file-loader' 仅仅是利用file-loader能吧对应的文件移动到dist目录下的特性而已
                        options: {
                            outputPath: 'fonts/'
                        }
                    }
                }]
            },
            output: {
                filename: 'bundle.js',
                path: path.resolve(__dirname, 'dist')
            }
        }
        ```
        ```scss
        // index.scss
        @font-face {font-family: "iconfont";
            src: url('../fonts/iconfont.eot?...')
            src: url('../fonts/iconfont.eot?...')
            url('../fonts/iconfont.woff?...')
            url('../fonts/iconfont.ttf?...')
            url('../fonts/iconfont.svg?...')
        }
        
        .iconfont {
            font-family: "iconfont" !important;
            font-size: 16px;
            font-style: normal;
            -webkit-font-smoothing: antialiased;
            -moz-osx-font-smoothing: grayscale;
        }
        
        .icon-edit-tools:before { content: "\e615"; }
        
        .icon-calendar:before { content: "\e613"; }
        ```
        ```js
        // index.js
        require('./css/index.scss')

        let dom = document.getElementById('dom')

        dom.innerHTML = `
            <div class="iconfont icon-edit-tools"></div>
            <div class="iconfont icon-calendar"></div>
        `
        ```
        ```js
        //postcss.config.js
        module.exports = {
            plugins: [
                require('autoprefixer')
            ]
        }
        ```

- ### 3-5 使用 plugins 让打包更便捷
    - ```plugins```的作用
        - ```plugins```可以做webpack运行到某个时刻的时候，帮你做一些事情
    - #### 1. ```HtmlWebpackPlugin```
        - 之前存在的问题：在项目中，```index.html```文件总是我们手动创建并修改的，如果每次打包都需要这样的手动操作 就会显得很麻烦。那么，我们能不能通过webpack自动生成```index.html```文件呢？可以的！
        - 1.```npm i -D html-webpack-plugin``` 安装插件
        - 2.使用
            ```js
            // webpack.config.js
            const HtmlWebpackPlugin = require('html-webpack-plugin')
            const path = require('path')

            module.exports = {
                entry: 'index.js',
                output: {
                    path: path.resolve(__dirname, 'dist'),
                    filename: 'bundle.js'
                },
                plugins: [new HtmlWebpackPlugin()]
            }
            ```
            打包后，它会在```dist```目录下自动生成 ```index.html```
            ```html
            // dist/index.html
            <!DOCTYPE html>
            <html>
            <head>
                <meta charset="UTF-8">
                <title>webpack App</title>
            </head>
            <body>
                <script src="bundle.js"></script>
            </body>
            </html>
            ```
        - 3.**```HtmlWebpackPlugin``` 会在打包结束后，自动生成一个html文件，并把打包生成的js自动引入到这个html文件中**
        - ##### 4. ```HtmlWebpackPlugin``` 使用模板
            ```js
            // webpack.config.js
            const HtmlWebpackPlugin = require('html-webpack-plugin')
            const path = require('path')

            module.exports = {
                entry: 'index.js',
                output: {
                    path: path.resolve(__dirname, 'dist'),
                    filename: 'bundle.js'
                },
                plugins: [new HtmlWebpackPlugin({
                    template: 'src/index.html'
                })]
            }
            ```
            ```html
            // src/index.html
            <!DOCTYPE html>
            <html>
            <head>
                <meta charset="UTF-8">
                <title>html 模板</title>
            </head>
            <body>
                <div id="root"></div>
            </body>
            </html>
            ```
            - 打包后会生成如下html
            ```html
            // dist/index.html
            <!DOCTYPE html>
            <html>
            <head>
                <meta charset="UTF-8">
                <title>html 模板</title>
            </head>
            <body>
                <div id="root"></div>
                <script src="bundle.js"></script>
            </body>
            </html>
            ```
    - #### 2.```CleanWebpackPlugin```
        - 先看存在的问题：
            - 在开发过程中，假如我需要把打包输出的 ```bundle.js``` 改名为 ```dist.js```，但是我又不想要去手动删除打包输出的 ```dist``` 目录，那么有没有什么办法能帮我自动完成这个操作的呢？特别是在要删除的东西不是一个两个的时候（批量手动操作实在太麻烦了）。解决方法如下
        - 1.安装 ```npm i -D clean-webpack-plugin```
        - 2.使用
            ```js
            // webpack.config.js
            const HtmlWebpackPlugin = require('html-webpack-plugin')
            const CleanWebpackPlugin = require('clean-webpack-plugin')
            const path = require('path')

            module.exports = {
                entry: 'index.js',
                output: {
                    path: path.resolve(__dirname, 'dist'),
                    filename: 'bundle.js'
                },
                plugins: [
                    new HtmlWebpackPlugin({
                    template: 'src/index.html'
                    }),
                    new CleanWebpackPlugin(['dist']) // 在打包前，它会自动删除 dist 目录下的所有文件
                ]
            }
            ```
- ### 3-6 Entry 与 Output 的基础配置
    - 1.输出命名
        ```js
        // webpack.config.js
        const path = require('path')

        module.exports = {
            mode: 'development',
            entry: {
                main: './src/index.js'  // 这里对象的键名，表示打包输出文件名为main.js
            },
            output: {
                filename: 'bundle.js',  // 如果没定义输出文件名，默认为main.js
                path： path.resolve(__dirname, 'dist')
            }
        }
        ```
    - 2.打包多个文件
        ```js
        // webpack.config.js
        const path = require('path')

        module.exports = {
            mode: 'development',
            entry: {
                main: './src/index.js'
                sub: './src/sub.js'
            },
            output: {
                filename: '[name].js',  // 这里支持 placeholder占位符 各种写法
                path： path.resolve(__dirname, 'dist')
            }
        }
        ```
        输出结果：在 dist 目录下会生成 ```main.js``` 和 ```sbu.js```
        #### 原理：output 中 ```filename: '[name].js'``` [name] 会自动根据 entry入口文件 的键名去生成对应的 ```main.js``` 和 ```sbu.js```
    - 3.```publicPath```
        - 问题：如果我们把 index.html 文件给到后端作为入口文件，而其他的静态资源我们上传到 cdn 上， 如```<script src="http://cdn.com.cn/main.js"></script>```，我们希望webpack能自动帮我们在 index.html 中插入的 js文件 自动插入cdn地址，我们该怎么办呢？
        - 解决方法：
            ```js
            // webpack.config.js
            const path = require('path')

            module.exports = {
                mode: 'development',
                entry: {
                    main: './src/index.js'
                    sub: './src/sub.js'
                },
                output: {
                    publicPath: 'http://cdn.com.cn',    // 在这里配置
                    filename: '[name].js',
                    path： path.resolve(__dirname, 'dist')
                }
            }
            ```
            打包结果如下：
            ```html
            // dist/index.html
            <!DOCTYPE html>
            <html>
            <head>
                <meta charset="UTF-8">
            </head>
            <body>
                <script src="http://cdn.com.cn/main.js"></script>
                <script src="http://cdn.com.cn/sub.js"></script>
            </body>
            </html>
            ```
- ### 3-7 SourceMap 的配置
    - #### 最佳配置：
        - 开发环境
            - 优点：提示错误比较全，打包速度比较快
            ```js
            // webpack.config.js
            module.exports = {
                mode: 'development',
                devtool: 'cheap-module-eval-source-map'
                ...
            }
            ```
        - 生产环境
            - 生成环境中，可以不用 ```devtool```，或者配置如下
            ```js
            // webpack.config.js
            module.exports = {
                mode: 'production',
                devtool: 'cheap-module-source-map'
                ...
            }
            ```
    - 什么是 ```SourceMap``` ?
        - 问题再现：
            - 当你在开发过程中，```src/index.js``` 文件中的第1行 ```consele.log(hello world)``` 写错了，你却不知道的时候 执行了打包。
            ```js
            // webpack.config.js
            module.exports = {
                mode: 'development',
                devtool: 'none'         // 关闭 SourceMap
                ...
            }
            ```
            如果在 ```SourceMap``` 没开启的情况下打包，且入口文件 ```src/index.js``` 内有错误，那么它在报错的时候，就会说
             - 在 ```dist/bundle.js``` 文件中第96行发现错误。
             - 但是问题来了，我并不希望你告诉我 ```dist/bundle.js``` 哪里错了，而是希望你能告诉我源码中哪里错了，如 ```src/index.js``` 文件中的第1行 ```consele.log(hello world)``` 写错了。那我们该怎么办呢？
        - #### SourceMap 定义
            - ```SourceMap``` 它是一个映射关系，当它知道在 ```dist/bundle.js``` 文件中第96行发现错误，实际上就对应着 如 ```src/index.js``` 入口文件中的第1行 ```consele.log(hello world)``` 发现错误。
            - 一言以蔽之，```SourceMap``` 就是把输出后的js，与源码做映射。
        - #### SourceMap 用法
            - 1.开启 SourceMap
                ```js
                // webpack.config.js
                module.exports = {
                    mode: 'development',
                    devtool: 'source-map'   // 开启 SourceMap
                    ...
                }
                ```
            - 2.查找源码错误
                - 开启 ```SourceMap``` 后，重新打包，如果浏览器 ```console面板``` 提示错误，点击错误提示就会 **直接跳转到错误的源文件处了**。
        - 文档解析：
            - [文档地址](https://webpack.js.org/configuration/devtool#devtool)
            - ```inline-source-map```
                - 在 webpack.config.js 中 ```devtool: 'xxx'``` 的所有配置参数中，凡是没带 ```inline-``` 开头的，都会在 ```dist``` 目录下生成 ```bundle.js.map``` 映射文件
                - 而凡是带了 ```inline-``` 开头的，```.map``` 映射文件的内容都会被 以```base64```方式 写入输出的 ```bundle.js``` 文件中
            - ```cheap-source-map```
                - 这是 "cheap(低开销)" 的 SourceMap ,因为它没有生成列映射，只是映射行数。
                - 默认情况下，不带 ```cheap-``` 的 SourceMap 默认既映射行数，也映射列数，但是打包过程比较久，比较耗费性能。
            - ```cheap-module-source-map```
                - 存在的问题：
                    - 而且 ```cheap-``` 只映射 **业务代码** (即entry的入口文件) 的映射关系，不会去管其它的模块错误。
                - 当 ```devtool: 'cheap-module-source-map'``` 时，这时候的映射关系不仅映射 **业务代码** ，还映射业务代码引入的其他 文件或模块 ，或第三方 module 的错误。
            - ```eval```
                - 当 ```devtool: 'eval'``` 时
                    - ```eval``` 是打包速度最快的一种方式，也能映射报错位置。
                    - ```eval``` 既不生成 ```bundle.js.map``` 映射文件，也不会像 ```inline-source-map``` 中以 ```base64``` 方式吧 ```映射关系``` 写入在 ```bundle.js``` 内。
                    - 但是，```eval``` 以 ```eval("...")... sourceURL=...``` 方式把 ```映射关系``` 写入 ```bundle.js``` 中
                    - **缺点**：对于比较复杂的项目，```eval``` 这种打包方式的 **提示可能并不全面**
- ### 3-8 使用 ```WebpackDevServer``` 提升开发效率
    - ```wabpack --watch```
        - 1.配置 ```package.json```
            ```js
            // package.json
            {
                ...
                "scripts": {
                    "watch" : "webpack --watch",
                    // "bundle": "webpack"
                }
                ...
            }
            ```
        - 2.```npm run watch``` 执行该脚本
        - 3.这时候，当你改动项目中 ```src``` 下的任何文件，webpack 监听到变动，就会自动重新打包。
    - 存在问题：
        - ```wabpack --watch``` 这种方式还不足够方便
        - 如果我希望能，在我第一次执行 ```npm run watch``` 的时候，
            - 1.自动帮我打包，
            - 2.自动帮我把浏览器打开， 
            - 3.同时还可以帮我模拟一些服务器上的特性
    - #### ```WebpackDevServer```
        - 优点：
            - 监听入口文件，自动打包
            - 开启本地服务器
            - 自动刷新页面，不需要手动刷新
            - 由于是本地服务器(http://协议)，所以可以发送 ajax 请求。本地文件打开则不行(file://协议)
        - 1.安装 ```npm i -D webpack-dev-server```
        - 2.配置
            ```js
            // webpack.config.js
            const path = require('path')

            module.exports = {
                mode: 'development',
                devtool: 'cheap-module-eval-source-map',
                entry: './src/index.js',
                devServer: {
                    contentBase: './dist',   // 借助devServer起一个服务器，根路径为'./dist'
                    open: true      // 自动打开浏览器，并访问 http://localhost:8080
                },
                output: {
                    filename: 'bundle.js',
                    path: path.resolve(__dirname, 'dist')
                }
            }
            ```
            ```js
            // package.json
            {
                ...
                "scripts": {
                    "watch" : "webpack --watch",
                    "start" : "webpack-dev-server",
                    // "bundle": "webpack"
                }
                ...
            }
            ```
        - 3.执行脚本 ```npm run start```
            - 这时候 ```webpack-dev-server``` 就开启了本地服务器 ```http://localhost:8080```
            - 而且监听入口文件，自动打包
    - 手动写一个 ```webpackDevServer``` （了解即可）
        - 配置
            ```js
            // server.js
            const express = require('express')
            const webpack = require('webpack')
            const webpackDevMiddleware = require('webpack-dev-middleware')
            const config = require('./webpack.config.js')
            const complier = webpack(config)

            const app = express()

            app.use(webpackDevMiddleware(complier, {
                publicPath: config.output.publicPath
            }))

            app.listen(3000, () => {
                console.log('server is running')
            })
            ```
            ```js
            // webpack.config.js
            const path = require('path')

            module.exports = {
                mode: 'development',
                devtool: 'cheap-module-eval-source-map',
                entry: './src/index.js',
                devServer: {
                    contentBase: './dist',   // 借助devServer起一个服务器，根路径为'./dist'
                    open: true      // 自动打开浏览器，并访问 http://localhost:8080
                },
                output: {
                    publicPath: '/',
                    filename: 'bundle.js',
                    path: path.resolve(__dirname, 'dist')
                }
            }
            ```
            ```js
            // package.json
            {
                ...
                "scripts": {
                    // "bundle": "webpack"
                    "watch" : "webpack --watch",
                    "start" : "webpack-dev-server",
                    "server": "node server.js"
                }
                ...
            }
            ```
            ```
            // 项目目录
            +  |- /node_modules
            +  |- /src
               |- package.json
               |- server.js
               |- webpack.config.js
            ```
        - 开启服务
            - ```npm run server```
            - 而且监听入口文件，自动打包
            - 开启服务后，访问 ```http://localhost:3000``` 预览项目

- ### 3-9 ```Hot Module Replacement``` 模块热更新
    - 1.css热更新
        - 以前存在的问题：
            - 之前执行 ```webpack-dev-server``` 的时候，自动监听 src目录，当监听到文件修改的时候，就会自动打包并刷新页面。
            - 但是有时候我们会觉得挺麻烦的，例如：给button绑定事件，每点击一次，新增一个div，当我们修改该div的css，浏览器就会自动刷新了，导致每修改一次css，都需要重新点击button，听麻烦的...
            - 那么如何才能只修改css，不重载 document 呢？用下面的 ```Hot Module Replacement``` 模块热更新
        - 配置
            1. ```devServer``` 开启 hot 和 hotOnly
            2. ```const webpack = require('webpack')```
            3. ```plugins:[ new webpack.HotModuleReplacementPlugin() ]```
            ```js
            // webpack.config.js
            const path = require('path')
            const webpack = require('webpack')

            module.exports = {
                mode: 'development',
                devtool: 'cheap-module-eval-source-map',
                entry: './src/index.js',
                devServer: {
                    contentBase: './dist',
                    open: true,
                    hot: true,     // 开启 Hot Module Replacement
                    hotOnly: true  // 构建失败时不刷新页面
                },
                plugins:[
                    new webpack.HotModuleReplacementPlugin()
                ]
                output: {
                    publicPath: '/',
                    filename: 'bundle.js',
                    path: path.resolve(__dirname, 'dist')
                }
            }
            ```
    - 2.js模块的热更新
        - 核心关键：
            ```js
            if(module.hot){
                // 如果文件发生变化，就执行后面的函数
                module.hot.accept('./number', ()=>{
                    // ...执行内容
                })
            }
            ```
        - 配置案例：
            - 需求：修改 ```number.js模块``` 中的1000，不影响 ```// counter.js模块``` 中已经执行的结果
            ```js
            // number.js 模块
            function number(){
                let div = document.createElement('div');
                div.setAttribute('id','number')
                div.innerHTML = 1000
                document.body.appendChild(div)
            }
            module.exports = number
            ```
            
            ```js
            // counter.js 模块
            function counter(){
                let div = document.createElement('div');
                div.setAttribute('id','counter')
                div.innerHTML = 1
                div.onclick = function(){
                    div.innerHTML = parseInt(div.innerHTML, 10) + 1 // 每次点击+1
                }
                document.body.appendChild(div)
            }
            module.exports = counter
            ```
            ```js
            // index.js
            import counter from './counter';
            import number from './number';

            counter();
            number();


            if(module.hot){
                // 如果文件发生变化，就执行后面的函数
                module.hot.accept('./number', ()=>{
                    document.body.removeChild(document.getElementById('number'))
                    number();
                })
            }
            ```
            
            ```js
            // webpack.config.js
            const path = require('path')
            const webpack = require('webpack')

            module.exports = {
                mode: 'development',
                devtool: 'cheap-module-eval-source-map',
                entry: './src/index.js',
                devServer: {
                    contentBase: './dist',
                    open: true,
                    hot: true,     // 开启 Hot Module Replacement
                    hotOnly: true  // 构建失败时不刷新页面
                },
                plugins:[
                    new webpack.HotModuleReplacementPlugin()
                ]
                output: {
                    publicPath: '/',
                    filename: 'bundle.js',
                    path: path.resolve(__dirname, 'dist')
                }
            }
            ```

- ### 3-11 使用 ```Babel``` 处理 ES6 语法
    - [Babel 官网](https://babeljs.io/setup#installation)
    - 1.安装 ```npm install --save-dev babel-loader @babel/core```
    - 2.```npm i -D @babel/preset-env```
        - 为什么还要安装 @babel/preset-env 呢？实际上 babel-loader 只是 webpack和babel 的通信桥梁，而 ```@babel/preset-env``` 才能 ES6 转成 ES5
    - 3.配置
        ```js
        // webpack.config.js
        module:{
            rules:[
                test: /\.js$/, 
                exclude: /node_modules/,  // 排除 node_modules 目录的文件
                loader: "babel-loader",
                options: {
                    presets: ["@babel/preset-env"]
                }
            ]
        }
        ```
    - 4.在 ```webpack.config.js```中 module.rules 增加 ```options: { presets: ["@babel/preset-env"] }```
    - 5.```polyfill```
        - 在走完前面4步之后，还是有一些 对象 或 函数，在低版本的浏览器是没有的，所以这时候 **就要把这些缺失的函数补充进来**，这时候就要用到 ```polyfill``` 了
        - 安装 ```npm i @babel/polyfill -D```
        - 使用：
            - 在业务代码 ```index.js``` 的最顶部引入 ```polyfill```
                ```js
                // index.js
                import "@babel/polyfill"

                const arr = [
                    new Promise(() => {}),
                    new Promise(() => {})
                ]

                arr.map(item => {
                    console.log(item)
                })
                ```
        - 存在的问题：
            - ```polyfill``` 配置完上面的步骤后，如果直接打包，会发现他把低版本浏览器缺失的函数和对象全都加载进来了，导致输出文件非常大。
            - 问题的解决如下：给 ```presets``` 添加选项 ```useBuiltIns: 'usage'```
    - #### 6.业务代码 babel最佳配置
        ```js
        // webpack.config.js

        const path = require('path')

        module.exports = {
            mode: 'development',
            entry: {
                main: './src/index.js'
            },
            module: {
                rules: [{
                test: /\.js$/, 
                exclude: /node_modules/,  // 排除 node_modules 目录的文件
                loader: "babel-loader",
                options: {
                    presets: [[
                        "@babel/preset-env", {
                        targets: {
                            edge: "17",
                            firefox: "60",
                            chrome: "67",
                            safari: "11.1",
                        },
                        useBuiltIns: 'usage'    // 只添加 index.js 用到对象或函数
                        // 当设置了 useBuiltIns: 'usage' 之后，业务代码就不必再次 import "@babel/polyfill"，因为它会自动引入
                    }]]
                }}]
            },
            output: {
                filename: 'bundle.js',
                path: path.resolve(__dirname, 'dist')
            }
        }
        ```
    - #### 7.其他场景：开发类库、第三方模块、组件库
        - 1.存在的问题
            - 通过 ```import "@babel/polyfill"``` 这种方案实际是有问题的，因为 它在补充注入缺失的对象或者函数的时候，**是通过全局变量的形式，会污染到全局环境**。 解决方法如下
        - 2.安装
            - ```npm i -D @babel/plugin-transform-runtime```
            - ```npm i -D @babel/runtime```
            - ```npm install --save @babel/runtime-corejs2```
        - 配置
            ```js
            // webpack.config.js
            const path = require('path')

            module.exports = {
                mode: 'development',
                entry: {
                    main: './src/index.js'
                },
                module: {
                    rules: [{
                    test: /\.js$/, 
                    exclude: /node_modules/,
                    loader: "babel-loader",
                    options: {
                        "plugins": [["@babel/plugin-transform-runtime", {
                            "corejs": 2,    // 默认为false, 值为2时，要多安装一个 @babel/runtime-corejs2 包
                            "helpers": true,
                            "regenerator": true,
                            "useESModules": false
                        }]]
                    }}]
                },
                output: {
                    filename: 'bundle.js',
                    path: path.resolve(__dirname, 'dist')
                }
            }
            ```
            - 业务代码 ```index.js``` 中不需要引入 ```import "@babel/polyfill"```
                ```js
                // index.js
                const arr = [
                    new Promise(() => {}),
                    new Promise(() => {})
                ]

                arr.map(item => {
                    console.log(item)
                })
                ```
    - ```.babelrc``` babel配置文件
        - 1.存在的问题：由于 babel 的配置参数非常多，如果全都写在 ```webpack.config.js``` 则会非常长。所以可以把 ```babel-loader``` 中的 ```options``` 对象单独拿出来，写在 ```.babelrc``` 文件中，并放在 ```webpack.config.js``` 同级目录下。
        - 2.配置
            ```js
            // webpack.config.js
            const path = require('path')

            module.exports = {
                mode: 'development',
                entry: {
                    main: './src/index.js'
                },
                module: {
                    rules: [{
                    test: /\.js$/, 
                    exclude: /node_modules/,
                    loader: "babel-loader"
                    }]
                },
                output: {
                    filename: 'bundle.js',
                    path: path.resolve(__dirname, 'dist')
                }
            }
            ```
            ```js
            // .babelrc
            {
                "plugins": [["@babel/plugin-transform-runtime", {
                    "corejs": 2,
                    "helpers": true,
                    "regenerator": true,
                    "useESModules": false
                }]]
            }
            ```
            注意：```.babelrc``` 文件中不能写注释
- 3-13 Webpack 实现对React框架代码的打包
    - ... 暂不记录，请查阅视频讲解
- ### 第3章 Webpack 的核心概念总结 最佳配置
    ```js
    // webpack.config.js
    const path = require('path')
    const HtmlWebpackPlugin = require('html-webpack-plugin')
    const CleanWebpackPlugin = require('clean-webpack-plugin')
    const webpack = require('webpack')

    module.exports = {
        mode: 'development',
        devtool: 'cheap-module-eval-source-map',
        entry:{
            main: './src/index.js'
        },
        devServer: {
            contentBase: './dist',
            open: true,
            port: 8080,
            hot: true,
            hotOnly: true
        },
        module: {
            rules: [{
                test: /\.js$/,
                exclude: /node_modules/,
                loader: 'babel-loader',
                options: {
                    presets: [[
                        "@babel/preset-env", {
                        targets: {
                            edge: "17",
                            firefox: "60",
                            chrome: "67",
                            safari: "11.1",
                        },
                        useBuiltIns: 'usage'    // 只添加用到对象或函数
                    }]]
                }
            },{
                test: /\.(jpg|png|gif)$/,
                use: {
                    loader: 'url-loader',
                    options: {
                        name: '[name].[hash].[ext]',
                        outputPath: 'images/',
                        limit: 10240
                    }
                }
            },{
                test: /\.(eot|ttf|svg)$/,
                use: {
                    loader: 'file-loader'
                }
            },{
                test: /\.scss$/,
                use: [
                    'style-loader',
                    {
                        loader: 'css-loader',
                        options: {
                            importLoaders: 2
                        }
                    },
                    'sass-loader',
                    'postcss-loader'
                ]
            },{
                test: /\.css$/,
                use: [
                    'style-loader',
                    'css-loader',
                    'postcss-loader'
                ]
            }]
        },
        plugins: [
            new HtmlWebpackPlugin({
                template: 'src/index.html'
            }),
            new CleanWebpackPlugin(),
            new webpack.HotModuleReplacementPlugin()
        ],
        output: {
            filename: '[name].js',  // [name] 值根据 entry 的 key 值决定的
            path: path.resolve(__dirname, 'dist')
        }
    }
    ```
## 第4章 Webpack 的高级概念
- ### 4-1 Tree Shaking 概念详解
    - 1.什么是 ```Tree Shaking``` ?
        - 中文译名：摇树       （好像翻译有点生硬(　＾∀＾)）
        - ```Tree Shaking``` 是干嘛的？
            - 简单的说，就是把需要的留下，不需要的甩掉
            - 举个例子：
                - 现在有一个 math 函数库，但是在我的业务代码中只使用了 add、minus 两个函数 ( 实现思路：先 import "math 函数库" 进来，然后调用 add、minus 两个函数 )。
                - 在打包的时候，你会发现，虽然我们只使用的 add、minus 两个函数，但是 webpack 会把你 import 进来 **整个函数库都一并打包进来了**，这并不是我想要的，因为这会导致代码量剧增，而且这些没用到的都是没用的代码。
                - ```Tree Shaking``` 是干嘛的？ 答：就是帮助你，**把需要的留下，不需要的甩掉**
    - 2.```Tree Shaking``` 的特点
        - 1.```Tree Shaking``` 只支持 ES Module 的引入方式
            - 只支持 √ ```import { add } from './math.js'```    ，底层是静态引入
            - 不支持 × ```const add = require('./math.js') ```  ，底层是动态引入
        - 2.配置 ```Tree Shaking``` 
            - 1.在```development``` 环境模式下要使用```Tree Shaking```，需要开启```optimization: { usedExports: true }```
                ```js
                // webpack.config.js
                const path = require('path')

                module.exports = {
                    mode: 'development', // development 模式默认是没有 Tree Shaking 功能的，所以要在 optimization.usedExports 开启
                    entry:{ main: './src/index.js' },
                    module: {...},
                    plugins: [...],
                    optimization: {
                        usedExports: true   // 只导出，被使用的部分
                    }
                    output: {
                        filename: '[name].js',
                        path: path.resolve(__dirname, 'dist')
                    }
                }
                ```
        - 3.```sideEffects``` 帮助你, 排除掉那些没有导出的模块
            ```js
            // package.json
            {
                "sideEffects": false    // 默认为 false
            }
            ```
            - 1.只要你配置了 ```Tree Shaking``` ，那么 webpack 只要打包一个模块，就会应用 ```Tree Shaking``` 的方式
            - 2.存在的问题
                - 假如你引入了一个 ```import @babel/polyfill```
                - 而， ```import @babel/polyfill``` 实际上并没有导出任何的内容，而是给全局变量直接绑定一些对象或函数，如```window.Promise```
                - 这时候，如果你使用了 ```import @babel/polyfill```，那么 ```Tree Shaking``` 就会检测到，你并没有导出任何的内容，于是 ```Tree Shaking``` 就会直接把 ```@babel/polyfill``` 给忽略掉了
                - 但是，这并不是我们想要的。我们希望保留 ```@babel/polyfill``` ，那么怎么办呢？看下面解决方法
            - 3.解决方法
                - 当中业务代码中用到那些，没有导出 ( ```mudule.exports = {...}``` ) 的模块时，就需要在 ```package.json``` 中 ```sideEffects``` 排除掉那些文件模块
                ```js
                // index.js
                import "./src/style.css"
                import "./src/index.scss"
                import "@babel/polyfill"
                ```
                ```js
                // package.json
                {
                    "sideEffects": [
                        "@babel/polyfill",  // Tree Shaking 直接跳过这个文件
                        "*.css",            // Tree Shaking 直接跳过这个文件
                        "*.scss"            // Tree Shaking 直接跳过这个文件
                    ]
                }
                ```
                这样配置完后，Tree Shaking 就会跳过 @babel/polyfill、css文件、scss文件 了
        - 4.在 webpack development 环境下，即使使用了 ```Tree Shaking``` , 在打包的时候 **不会直接把那些你没用到的代码直接丢掉**，仍然存在于 出口文件 bundle.js 中，但是会在其中提示 ```exports provided:...```提供的函数， ```exports used:...``` 用到的函数
        - 5.在生产环境 ```production``` 中,就会完全丢掉那些没用到的代码了
            ```js
            // package.json
            {
                "sideEffects": false
            }
            ```
            ```js
            // webpack.config.js
            const path = require('path')
            const HtmlWebpackPlugin = require('html-webpack-plugin')
            const CleanWebpackPlugin = require('clean-webpack-plugin')
            const webpack = require('webpack')

            module.exports = {
                mode: 'production',
                devtool: 'cheap-module-source-map',
                entry:{
                    main: './src/index.js'
                },
                devServer: {
                    contentBase: './dist',
                    open: true,
                    port: 8080,
                    hot: true,
                    hotOnly: true
                },
                module: {
                    rules: [{
                        test: /\.js$/,
                        exclude: /node_modules/,
                        loader: 'babel-loader',
                        options: {
                            presets: [[
                                "@babel/preset-env", {
                                targets: {
                                    edge: "17",
                                    firefox: "60",
                                    chrome: "67",
                                    safari: "11.1",
                                },
                                useBuiltIns: 'usage'    // 只添加用到对象或函数
                            }]]
                        }
                    },{
                        test: /\.(jpg|png|gif)$/,
                        use: {
                            loader: 'url-loader',
                            options: {
                                name: '[name].[hash].[ext]',
                                outputPath: 'images/',
                                limit: 10240
                            }
                        }
                    },{
                        test: /\.(eot|ttf|svg)$/,
                        use: {
                            loader: 'file-loader'
                        }
                    },{
                        test: /\.scss$/,
                        use: [
                            'style-loader',
                            {
                                loader: 'css-loader',
                                options: {
                                    importLoaders: 2
                                }
                            },
                            'sass-loader',
                            'postcss-loader'
                        ]
                    },{
                        test: /\.css$/,
                        use: [
                            'style-loader',
                            'css-loader',
                            'postcss-loader'
                        ]
                    }]
                },
                plugins: [
                    new HtmlWebpackPlugin({
                        template: 'src/index.html'
                    }),
                    new CleanWebpackPlugin(),
                    new webpack.HotModuleReplacementPlugin()
                ],
                // optimization: {          // production 模式下，usedExports 自动开启，不用再手动开启
                //     usedExports: true
                // },
                output: {
                    filename: '[name].js',  // [name] 值根据 entry 的 key 值决定的
                    path: path.resolve(__dirname, 'dist')
                }
            }
            ```

- ### 4-2 ```Development``` 和 ```Production``` 模式的区分打包
    - 1.我们对 ```Development``` 和 ```Production```  的功能要求不同
        - ```Development``` 模式下，我们希望
            - 1.```Webpack-Dev-Server``` 可以帮我们启动一个本地服务器，而且这个服务器还集成了 ```Hot Module Replacement```, 只要我更改了代码，它就会自动帮我重新打包，然后会实时更新页面
            - 2.```Source Map``` 错误提示尽可能全面，方便调试
            - 3.不希望压缩代码，可以简单查阅，可以看到更多说明项
        - ```Production``` 模式下
            - 1.```Source Map``` 尽可能简洁，或直接另外生成 ```.map``` 文件进行存储
            - 2.希望压缩代码
    - #### 2.开发环境、生产环境 配置文件分而治之 ```webpack.dev.js``` 和 ```webpack.prod.js```
        - 存在问题：
            - 由于对于 ```Development``` 和 ```Production``` 两种模式下的要求不同，如果只是通过手动修改 ```webpack.config.js``` 就会显得很麻烦。那么有没有什么方便的方法呢？看下面
        - ```webpack.dev.js``` 开发环境配置文件
            ```js
            // webpack.dev.js
            const path = require('path')
            const HtmlWebpackPlugin = require('html-webpack-plugin')
            const CleanWebpackPlugin = require('clean-webpack-plugin')
            const webpack = require('webpack')

            module.exports = {
                mode: 'development',    // 1. 切换模式
                devtool: 'cheap-module-eval-source-map',  // 2.添加 eval
                entry:{
                    main: './src/index.js'
                },
                devServer: {             // 3.开启 devServer
                    contentBase: './dist',
                    open: true,
                    port: 8080,
                    hot: true,
                    hotOnly: true   // 不自动刷新页面，只更新css (静态文件)
                },
                module: {
                    rules: [{
                        test: /\.js$/,
                        exclude: /node_modules/,
                        loader: 'babel-loader',
                        options: {
                            presets: [[
                                "@babel/preset-env", {
                                targets: {
                                    edge: "17",
                                    firefox: "60",
                                    chrome: "67",
                                    safari: "11.1",
                                },
                                useBuiltIns: 'usage'    // 只添加用到对象或函数
                            }]]
                        }
                    },{
                        test: /\.(jpg|png|gif)$/,
                        use: {
                            loader: 'url-loader',
                            options: {
                                name: '[name].[hash].[ext]',
                                outputPath: 'images/',
                                limit: 10240
                            }
                        }
                    },{
                        test: /\.(eot|ttf|svg)$/,
                        use: {
                            loader: 'file-loader'
                        }
                    },{
                        test: /\.scss$/,
                        use: [
                            'style-loader',
                            {
                                loader: 'css-loader',
                                options: {
                                    importLoaders: 2
                                }
                            },
                            'sass-loader',
                            'postcss-loader'
                        ]
                    },{
                        test: /\.css$/,
                        use: [
                            'style-loader',
                            'css-loader',
                            'postcss-loader'
                        ]
                    }]
                },
                plugins: [
                    new HtmlWebpackPlugin({
                        template: 'src/index.html'
                    }),
                    new CleanWebpackPlugin(),
                    new webpack.HotModuleReplacementPlugin()    // 4.使用 HMR
                ],
                optimization: {
                    usedExports: true   // 5.开启 usedExports
                },
                output: {
                    filename: '[name].js',
                    path: path.resolve(__dirname, 'dist')
                }
            }
            ```
        - ```webpack.prod.js``` 生产环境配置文件
            ```js
            // webpack.prod.js
            const path = require('path')
            const HtmlWebpackPlugin = require('html-webpack-plugin')
            const CleanWebpackPlugin = require('clean-webpack-plugin')
            const webpack = require('webpack')

            module.exports = {
                mode: 'production',    // 1. 切换模式
                devtool: 'cheap-module-source-map',   // 2.eval 去掉
                entry:{
                    main: './src/index.js'
                },
                // devServer: {        // 3. 生产环境不需要 DevServer
                //     contentBase: './dist',
                //     open: true,
                //     port: 8080,
                //     hot: true,
                //     hotOnly: true
                // },
                module: {
                    rules: [{
                        test: /\.js$/,
                        exclude: /node_modules/,
                        loader: 'babel-loader',
                        options: {
                            presets: [[
                                "@babel/preset-env", {
                                targets: {
                                    edge: "17",
                                    firefox: "60",
                                    chrome: "67",
                                    safari: "11.1",
                                },
                                useBuiltIns: 'usage'
                            }]]
                        }
                    },{
                        test: /\.(jpg|png|gif)$/,
                        use: {
                            loader: 'url-loader',
                            options: {
                                name: '[name].[hash].[ext]',
                                outputPath: 'images/',
                                limit: 10240
                            }
                        }
                    },{
                        test: /\.(eot|ttf|svg)$/,
                        use: {
                            loader: 'file-loader'
                        }
                    },{
                        test: /\.scss$/,
                        use: [
                            'style-loader',
                            {
                                loader: 'css-loader',
                                options: {
                                    importLoaders: 2
                                }
                            },
                            'sass-loader',
                            'postcss-loader'
                        ]
                    },{
                        test: /\.css$/,
                        use: [
                            'style-loader',
                            'css-loader',
                            'postcss-loader'
                        ]
                    }]
                },
                plugins: [
                    new HtmlWebpackPlugin({
                        template: 'src/index.html'
                    }),
                    new CleanWebpackPlugin(),
                    // new webpack.HotModuleReplacementPlugin()   // 4. HMR 不需要
                ],
                // optimization: {
                //     usedExports: true           // 5.usedExports 不需要
                // },
                output: {
                    filename: '[name].js',
                    path: path.resolve(__dirname, 'dist')
                }
            }
            ```
        - ```package.json```
            ```js
            // package.json
            {
                "scripts": {
                    "dev"  : "webpack-dev-server --config webpack.dev.js",  // 开发环境。为了高性能，打包内容存于内存中，不生成打包
                    "build": "webpack --config webpack.prod.js"   // 线上环境
                }
            }
            ```
    - #### 3.最佳配置：开发环境、生产环境
        - 1.存在问题
            - 执行完上面的配置 [2.开发环境、生产环境 配置文件分而治之](#2开发环境生产环境-配置文件分而治之-webpackdevjs-和-webpackprodjs) 之后，你会发现，```webpack.dev.js``` 和 ```webpack.prod.js``` 两个配置文件的内容重复代码比较多，那么该如何减少重复代码呢？
        - ##### 2.抽取公共代码 ```webpack.dev.js``` 和 ```webpack.prod.js```
            - ```npm i -D webpack-merge``` 借助 webpack-merge 模块，合并配置文件
            - 项目配置
                - 项目结构目录
                ```
                webpack-demo
                +   |- /build
                        |- webpack.common.js
                        |- webpack.dev.js
                        |- webpack.prod.js
                +   |- /dist
                +   |- /node_modules
                +   |- /src
                    |- .babelrc
                    |- package-lock.json
                    |- package.json
                    |- postcss.config.js
                ```
                ```js
                // package.json
                {
                    "scripts": {
                        "dev"  : "webpack-dev-server --config ./build/webpack.dev.js",  // 开发环境
                        "build": "webpack --config ./build/webpack.prod.js"   // 线上环境
                    }
                }
                ```
                ```js
                // webpack.prod.js
                const merge = require('webpack-merge')
                const commonConfig = require('./webpack.common.js')

                const prodConfig = {
                    mode: "production",
                    devtool: "cheap-module-source-map"
                }

                module.exports = merge(commonConfig, prodConfig)
                ```
                ```js
                // webpack.dev.js
                const webpack = require('webpack')
                const merge = require('webpack-merge')
                const commonConfig = require('./webpack.common.js')

                const devConfig = {
                    mode: 'development',
                    devtool: 'cheap-module-eval-source-map',
                    devServer: {
                        contentBase: './dist',
                        open: true,
                        port: 8080,
                        hot: true
                    },
                    plugins: [
                        new webpack.HotModuleReplacementPlugin()
                    ],
                    optimization: {
                        usedExports: true
                    }
                }

                module.exports = merge(commonConfig, devConfig)
                ```
                ```js
                // webpack.common.js
                const path = require('path')
                const HtmlWebpackPlugin = require('html-webpack-plugin')
                const CleanWebpackPlugin = require('clean-webpack-plugin')

                module.exports = {
                    entry: {
                        main: './src/index.js'
                    },
                    module: {
                        rules: [{
                            test: /\.js$/,
                            exclude: /node_modules/,
                            loader: 'babel-loader',
                            options: {
                                presets: [[
                                    "@babel/preset-env", {
                                    targets: {
                                        edge: "17",
                                        firefox: "60",
                                        chrome: "67",
                                        safari: "11.1",
                                    },
                                    useBuiltIns: 'usage'
                                }]]
                            }
                        },{
                            test: /\.(jpg|png|gif)$/,
                            use: {
                                loader: 'url-loader',
                                options: {
                                    name: '[name].[hash].[ext]',
                                    outputPath: 'images/',
                                    limit: 10240
                                }
                            }
                        },{
                            test: /\.(eot|ttf|svg)$/,
                            use: {
                                loader: 'file-loader'
                            }
                        },{
                            test: /\.scss$/,
                            use: [
                                'style-loader',
                                {
                                    loader: 'css-loader',
                                    options: {
                                        importLoaders: 2
                                    }
                                },
                                'sass-loader',
                                'postcss-loader'
                            ]
                        },{
                            test: /\.css$/,
                            use: [
                                'style-loader',
                                'css-loader',
                                'postcss-loader'
                            ]
                        }]
                    },
                    plugins: [
                        new HtmlWebpackPlugin({
                            template: 'src/index.html'
                        }),
                        new CleanWebpackPlugin({
                            default: ['dist'],
                            root: path.resolve(__dirname, '../'),
                            // 由于 CleanWebpackPlugin 默认会认为，当前文件目录就是 根目录，所以要重写 根目录
                            
                            cleanOnceBeforeBuildPatterns: ['*.js', '!vendor', '!vendor.manifest.json']
                            // 这个参数配置要删除那些文件，和不要删除那些文件。要删除的文件'*.js'，不删除的文件前加个!
                        })
                    ],
                    output: {
                        filename: '[name].js',
                        path: path.resolve(__dirname, '../dist')
                    }
                }
                ```
            - 都配置完后，都执行打包命令，验证是否配置正确
                - ```npm run dev```
                - ```npm run build```
- ### 4-3 ```Code Splitting``` 代码分割
    - #### 代码分割 总结：
        - 1.代码分割，和 Webpack 无关
        - 2.Webpack 中实现代码分割，有两中方式
            - 1.同步加载代码：只需要在 ```webpack.common.js``` 中做 ```optimization``` 配置即可
            - 2.异步加载代码(import)：异步代码，无需做任何配置，会自动进行代码分割，放置到新的文件中
    - 1.```Code Splitting``` 到底是什么？
        - 为什么需要 ```Code Splitting``` ?
            - 现在我有一个业务代码 ```main.js```
                ```js
                // main.js
                const _ = require('lodash.js')  // 引入 lodash.js

                console.log(_.join(['a','b','b'], '***'))   // 拼接字符串，以 '***' 分割
                console.log(_.join(['a','d','c'], '***'))
                ```
            - 假设
                - ```lodash.js``` 为 1Mb，
                - 业务代码```main.js``` 也为 1Mb。
                - 那么打包后，整个 ```main.js``` 为 2Mb
                - 当页面业务逻辑发生变化时，又要重新加载 2Mb 的内容，但是其中 ```lodash.js```(1Mb) 是没变的，这样的话，会很浪费性能和资源
            - 另外一种方式：**手动代码分割**
                - ```main.js``` 拆成 ```lodash.js```(1Mb) 和 ```main.js```(1Mb)
                - 当页面业务逻辑发生变化时，只需要重新 加载 ```main.js```(1Mb) 即可
                ```js
                // lodash.js 函数库
                const _ = require('lodash.js')
                window._ = _
                ```
                ```js
                // main.js 业务逻辑
                console.log(_.join(['a','b','b'], '***'))   // 拼接字符串，以 '***' 分割
                console.log(_.join(['a','d','c'], '***'))
                ```
                ```js
                // webpack.config.js
                const path = require('path')

                module.exports = {
                    entry: {
                        main: './src/index.js',
                        lodash: './src/lodash.js'
                    },
                    output: {
                        filename: '[name].js',
                        path: path.resolve(__dirname, 'dist')
                    }
                }
                ```
    - 2.```splitChunks``` 自动代码分割
        - 1.配置
            ```js
            // webpack.common.js
            const path = require('path')

            module.exports = {
                entry: {
                    main: './src/index.js'
                },
                optimization: {
                    splitChunks: {      // 配置 splitChunks 自动代码分割
                        chunks: 'all'
                    }
                },
                output: {
                    filename: '[name].js',
                    path: path.resolve(__dirname, 'dist')
                }
            }
            ```
            ```js
            // index.js
            import _ from 'lodash'  // 同步加载模块
            
            console.log(_.join(['a','b','b'], '***'))
            console.log(_.join(['a','d','c'], '***'))
            ```
        - 2.打包结果
            - 业务代码 ```dis/main.js```
            - 第三方库 ```dis/vendors~main.js```
    - #### 3.异步加载模块, (另一种代码分割方法)
        - 异步加载第三方模块，不需要在 ```webpack.common.js``` 中加 ```splitChunks```
            ```js 
            // webpack.common.js
            const path = require('path')

            module.exports = {
                entry: {
                    main: './src/index.js'
                },
                // ptimization: {
                //     splitChunks: {       // 异步加载第三方模块，不需要
                //         chunks: 'all'
                //     }
                },
                output: {
                    filename: '[name].js',
                    path: path.resolve(__dirname, 'dist')
                }
            }
            ```
        - 业务代码
            ```js
            // index.js
            // 思路：异步加载lodash.js，加载成功后，赋值给"_"，并创建 element, 将其挂载到 body 上
            function getComponent(){
                return import('lodash').then(( {default: _} ) => {
                    var element = document.createElement('div')
                    element.innerHTML = _.join(['Dell','Lee'], '-')
                    return element
                })
            }

            getComponent().then(element => {
                document.body.appendChild(element)
            })
            ```
        - 执行打包后报错
            - <p style="color:red">"support for the experimental syntax dynamicImport is not currently enabled"</p>
            - 目前不支持实验性语法dynamicImport
            - 解决方法：
                - 借助 babel ```npm i -D babel-plugin-dynamic-import-webpack```
                - ```.babelrc```文件，放在项目根目录
                    ```js
                    // .babelrc
                    {
                        plugins: ["dynamic-import-webpack"]
                    }
                    ```
        - 再执行打包后，生成文件
            - 业务代码 ```dist/main.js```
            - 第三方库 ```dist/0.js```

- ### 4-5 ```Split Chunks Plugin``` 配置参数详解
    - 1.存在问题
        在 [3.异步加载模块, (另一种代码分割方法)](#3.异步加载模块,-(另一种代码分割方法)) 项目中，最终打包后生成的代码，第三方库被命名为 ```0.js```。那么如果我需要改它的名字呢？
    - 2.在异步加载组件中，我们有一种语法，叫 ```Magic Comments```魔法注释。异步加载的组件，加载完后会被赋值给魔法注释的值，即给组件命名
        ```js
        // index.js
        function getComponent(){
            return import(/* webpackChunkName:"lodash" */ 'lodash').then(( {default: _} ) => {
                var element = document.createElement('div')
                element.innerHTML = _.join(['Dell','Lee'], '_')
                return element
            })
        }

        getComponent().then(element => {
            document.body.appendChild(element)
        })
        ```
        执行打包后发现，```dist/0.js```，还是没有成功命名为 ```lodash.js```，原因：```dynamic-import-webpack``` 不是官方插件，它不支持魔法注释
        ```js
        // .babelrc
        {
            presets: [[
                "@babel/preset-env", {
                    targets: {
                        edge: "17",
                        firefox: "60",
                        chrome: "67",
                        safari: "11.1"
                    },
                    // useBuiltIns: 'usage'
                }
            ]],
            // plugins: ["dynamic-import-webpack"]  // 非官方插件
            plugins: ["@babel/plugin-syntax-dynamic-import"]    // 官方推荐插件
        }
        ```
        - ```npm i -D @babel/plugin-syntax-dynamic-import```
        - 再执行打包，这时候 动态加载进来的组件被命名为：```dist/vendors~lodash.js```
    - 3.如果要把 动态加载组件 被直接命名为 ```lodash.js```，需要配置 ```webpack.common.js``` 里面的 ```optimization```
        ```js
        // webpack.common.js
        const path = require('path')

        module.exports = {
            entry: {
                main: './src/index.js'
            },
            module: {
                rules: [{
                    test: /\.js$/,
                    exclude: /node_modules/,
                    loader: 'babel-loader'
                }]
            },
            optimization: {
                splitChunks: {  // 无论同步加载代码分割，还是异步加载代码分割，这个参数都有效
                    chunks: 'all',
                    cacheGroups: {
                        vendors: false,
                        default: false
                    }
                }
            },
            output: {
                filename: '[name].js',
                path: path.resolve(__dirname, 'dist')
            }
        }
        ```
- ### 4-6.```SplitChunksPlugin``` 参数详解
    - 1.一般情况下，我们配置 ```splitChunks``` 直接使用 ```chunks: 'all'``` 就可以，其他的都用默认配置就行
        ```js
        // webpack.config.js
        module.exports = {
            // ...
            optimization: {
                splitChunks: { // 默认配置
                    chunks: 'all' // 可选参数 all, async, initial(同步)
                }
            }
        }
        ```
    - 2.```SplitChunksPlugin``` ，默认参数详解
        [SplitChunksPlugin 官网参数详解](https://webpack.js.org/plugins/split-chunks-plugin#optimizationsplitchunks)
        ```js
        // webpack.config.js
        module.exports = {
            // ...
            optimization: {
                splitChunks: {
                    chunks: 'async', //可选参数 all, async, initial(同步)
                    minSize: 30000,  // 超过30Kb 的才执行分割
                    maxSize: 0,      // 一般情况下不配置，即不设置上限，多少进来，就多少出去
                    // 如果配置了 'maxSize: 50000' (50Kb)，如果加载的模块为 1Mb，那么它会尝试性的将它分割成 20个 50Kb 的子文件
                    minChunks: 1,   // 当一个模块，被 引用超过多次才做代码分割    (require() 或 import )
                    maxAsyncRequests: 5,    // 按需加载时的最大并行请求数，一般不用改。如果超过，则不再分割代码
                    maxInitialRequests: 3,  // 入口点处的最大并行请求数。一般不用改。  如果超过，则不再分割代码
                    automaticNameDelimiter: '~',    // 分割代码的定界符，如 vendors~main.js
                    name: true,      // 开启 cacheGroups 的自动命名
                    cacheGroups: {  // 缓存组。当initial(同步)，才会走cacheGroups。chunks 和 cacheGroups 要配合使用，才有效
                        vendors: {  // 只要是加载在 node_modules 内的模块，都打包到一起，并命名为'vendors.js'
                            test: /[\\/]node_modules[\\/]/,
                            priority: -10,  // 优先级。当一个模块 如 jquery，即满足 vendors，又满足 default 的要求，priority值 大的优先执行
                            filename: 'vendors.js'  // 如果没配置filename，那么打包文件名为 vendors~main.js，意思是：属于vendors组，且入口文件为 main.js
                        },
                        default: {  // 如果模块不在 node_modules 里，就都会走 default 这里
                            minChunks: 2,
                            priority: -20,  // 优先级。当一个模块 如 jquery，即满足 vendors，又满足 default 的要求，priority值 大的优先执行
                            reuseExistingChunk: true,   // 如果一个模块已经被打包过了，再次打包的时候就会跳过，直接复用之前的打包
                            filename: 'common.js'  // 如果没配置filename，那么打包文件名为 default~main.js，意思是：属于default组，且入口文件为 main.js
                        }
                    }
                }
            }
        }
        ```
- ### 4-7 ```Lazy Loading``` 懒加载，```Chunk``` 是什么？
    - 1.先看同一功能的 ```同步加载实现```，和 ```异步加载实现``` 的方法
        ```js
        // 同步加载
        import _ form 'lodash'

        var element = document.createElement('div')
        element.innnerHTML = _.join(['Dell','Lee'], '_')
        document.body.appendChild(element)


        // 异步加载  (懒加载)
        function getComponent(){
            return import(/* webpackChunkName: 'lodash' */ 'lodash').then(( {default:_} ) => {
                var element = document.createElement('div')
                element.innerHTML = _.join(['Dell','Lee'], '_')
                return element
            })
        }

        document.addEventListener('click', ()=>{
            getComponent().then(element => {
                document.body.appendChild(element)
            })
        })
        ```
        ```js
        // webpack.common.js
        const path = require('path')

        module.exports = {
            entry: {
                main: './src/index.js'
            },
            module: {
                rules: [{
                    test: /\.js$/,
                    exclude: /node_modules/,
                    loader: 'babel-loader'
                }]
            },
            output: {
                filename: '[name].js',
                chunkFilename: '[name].chunk.js',   // 非入口文件entry，都会走chunkFilename这里。chunkname是未被列在entry中，却又需要被打包出来的文件命名配置。被异步引入的模块。或没被html直接引用的文件。
                path: path.resolve(__dirname, 'dist')
            }
        }
        ```
        ```js
        // .babelrc
        {
            presets: [[
                "@babel/preset-env", {
                    targets: {
                        chrome: "67"
                    },
                    useBuiltIns: 'usage'
                    // 当设置了 useBuiltIns: 'usage' 之后，业务代码就不必再次 import "@babel/polyfill"，因为它会自动引入
                }
            ]],
            plugins: ["@babel/plugin-syntax-dynamic-import"]
        }
        ```
    - 2.异步加载 懒加载 的特性
        - 上面的例子中
        - 异步加载，其实就是 ```Lazy Loading``` 懒加载
        - 当你访问页面的时候，懒加载的模块组件 并不会也同时跟index.html 一起被加载进来，而是等到你调用了 import 的时候，才会加载。
        - 懒加载的优点：性能好，按需加载，页面打开快
        - import 这种语法其实是 ES6 提出的实验性的语法，
        - ```import().then()```   import 后面跟一个 then() , 说明这其实是一个 Promise
        - 如果你要用 ```import``` 这种语法，你就必须要用 ```polyfill``` 使其兼容低版本的浏览器
    - 3.ES7 的异步加载，更简洁，但是要记住 用 babel/polyfill
        ```js
        // ES7异步加载  (懒加载)
        async function getComponent(){
            const { default: _ } = await import(/* webpackChunkName: 'lodash' */ 'lodash')
            const element = document.createElement('div')
            element.innerHTML = _.join(['Dell','Lee'], '_')
            return element
        }

        document.addEventListener('click', ()=>{
            getComponent().then(element => {
                document.body.appendChild(element)
            })
        })
        ```

- ### 4-8 ```Bundle Analysis``` 打包分析，```Prefetching``` 预取，```Preloading``` 预加载
    [```Bundle Analysis```， ```Prefetching```， ```Preloading``` webpack官网讲解](https://webpack.js.org/guides/code-splitting#prefetchingpreloading-modules)
    - #### 1.```Bundle Analysis``` 打包分析
        [webpack/analyse 官网](https://github.com/webpack/analyse)
        - 1.要做打包分析的前提，是要先生成一个 **打包过程的描述文件**
            - 打包命名内加一条这个，```webpack --profile --json > stats.js```
            ```js
            // package.json
            {
                // ...
                "scripts":{
                    "dev-build": "webpack --profile --json > stats.js --config ./build/webpack.dev.js"
                }
            }
            ```
        - 2.执行打包命令后，在根目录下会生成一个 ```stats.json``` **打包过程的描述文件**
            - 借助分析工具，可视化的查看 **打包过程的描述文件**
            - [【推荐】webpack-bundle-analyzer 比较好用，比较全面的分析 插件工具](https://github.com/webpack-contrib/webpack-bundle-analyzer)
            - [webpack-visualizer 可视化分析工具](https://chrisbateman.github.io/webpack-visualizer/)
            - [http://webpack.github.io/analyse](http://webpack.github.io/analyse)
            - [webpack 官网推荐的分析工具大全](https://webpack.js.org/guides/code-splitting#bundle-analysis)
    - #### 2.```Prefetching``` 预取，```Preloading``` 预加载
        - 1.首先，问一个问题
            ```js
            // webpack.config.js
            module.exports = {
                // ...
                optimization: {
                    splitChunks: {
                        chunks: 'async'     // 这里 webpack 默认值为，async ，那么为什么webpack官方推荐 默认值为 async 呢？
                    }
                }
            }
            ```
        - 2.答：
            - 如 jquery, lodash 这样的模块，我们同步加载 import 进来，打包生成页面后，当我们打开过一次页面后，jquery, lodash 就已经被加载过了，在第二次打开页面的时候，就会速度非常快，因为已经缓存到本地了。
            - 但是，Webpack 并不满足于此，**Webpack 官方希望，我在第一次打开页面的时候，速度也能非常快。** 那么，如何才能实现呢？看下面
        - 3.在过去，我们是这么实现这样的一个功能的
            ```js
            // index.js
            document.addEventListener('click', () => {
                const element = document.createElement('div')
                element.innerHTML = "Dell Lee"
                document.body.appendChild(element)
            })
            ```
            **功能是实现了，但是，这样的代码 就完全没有优化空间了吗？？？**
        - 4.```Show Coverage``` 检测代码利用率
            - 在 Chrome 中，打开控制台
            - ```ctrl + Shift + P``` 输入 ```Show Coverage``` 并打开
            - 在 ```Show Coverage``` 面板左上角，有个灰色小圆点，点一下，它变红之后，刷新页面
            - 刷新页面后，在 ```Show Coverage``` 面板中，可以看到 **各个文件的利用率** ```Unused Bytes```。绿色代表代码被执行了，红色代表没有被执行。
            - [Chrome DevTools 代码覆盖率功能详解](https://zhuanlan.zhihu.com/p/26281581)
        - 5.在上面 第3点 中
            ```js
            // index.js
            document.addEventListener('click', () => {
                // const element = document.createElement('div')
                // element.innerHTML = "Dell Lee"
                // document.body.appendChild(element)
            })
            ```
            中间这三行代码，**在页面加载的时候，是不会被执行的，只有点击了页面，它被调用了之后，才会执行**
        - ##### 6.异步加载交互代码
            - **不会执行的代码，在页面加载的时候，你就把它下载下来，实际上是有性能浪费的**
            - 那么，像这种交互的代码，应该怎么写呢？应该把它放到一个异步加载的模块里去
            ```js
            // click.js
            function handleClick(){
                const element = document.createElement('div')
                element.innerHTML = "Dell Lee"
                document.body.appendChild(element)
            }

            export default handleClick   // 导出 相当于 module.exports = handleClick
            ```
            ```js
            // index.js
            document.addEventListener('click', () => {
                import('./click.js').then(( {default: func} ) => {  // import 进来的模块，赋值给 func
                    func()
                })
            })
            ```
            - 像这样，只有多写些异步的代码，才能增加首次打开页面的速度
            - 这也就是为什么 ```webpack.config.js``` module.exports.optimization.splitChunks.chunks 默认为 async 的原因
        - 使用场景举例：
            - [慕课网](https://www.imooc.com/) 里，右上角的 登陆功能
            - 当我们登陆的时候，会弹出一个登陆弹窗，那这个登陆弹窗，实际上，在页面打开的时候，就不应该加载下来。
            - 等到你点击登陆的时候，才去加载登陆弹窗的代码
            - 但是，这样又会产生一个问题：就是当你点击的时候，才去请求，加载代码，就会使得页面需要去等待代码的加载，给人卡顿的感觉。
            - 那什么才能帮我们解决这个问题呢？  就是接下来要讲的，**Prefetching** 预取，和 **Preloading** 预加载
    - #### 2.```Prefetching``` 预取，```Preloading``` 预加载
        - 当我们访问首页的时候，不需要加载登陆的逻辑，只需要加载首页的内容即可，等待首页的内容加载完后，**再去偷偷的加载登陆的逻辑即可。**
        - 这样的话，等待首页内容加载完后，实际上网络带宽就已经空闲了，如果能够利用这个空闲的时间，把其他业务逻辑代码再加载进来，就能够解决 上面说的那个卡顿的问题了。
        - **最佳实现方式**
            ```js
            // index.js
            document.addEventListener('click', () => {
                import(/* webpackPrefetch: true */ './click.js').then(( {default: func} ) => {  // 魔法注释/* webpackPrefetch: true */写上之后，当主要的js 加载完后，有空闲时，就会加载 click.js
                    func()
                })
            })
            ```
        - ```Prefetching``` 预取，```Preloading``` 预加载 的区别
            - ```Prefetching``` 预取
                - 当主要的js 加载完后，有空闲时，就会加载你需要的js
            ```Preloading``` 预加载
                - 和主要的js文件一起加载
            - Preloading 没有 Prefetching 的方式好
    - #### 3.在做前端性能优化的时候，缓存不是最重要的点，就重要的点是 Code Coverage 代码覆盖率上面思考
- ### 4-9 Css 文件的代码分割，打包独立的css文件
    - 过去对于css的打包，都是 ```css in js``` 方式的，css是被写在js文件里的，如
        ```js
        // index.js
        import './style.css'

        console.log('hello world')
        ```
        那么，如何才能生成独立的css文件呢？
    - #### MiniCssExtractPlugin css提取插件
        - 默认情况下，会把多个css文件，合并成一个css文件
            ```js
            // index.js
            import './index.css'
            import './index1.css'
            ```
            打包的结果，就只会生成一个 ```main.css```
        - 有时候，如果发现都配置好了，执行打包后，还是没有生成独立的css文件，那么有可能是 被Tree Shaking 过滤掉了
            ```js
            // '/package.json'
            {
                "sideEffects": [
                    "*.css"         // 让 Tree Shaking 不处理css文件，直接保留
                ]
            }
            ```
            ```js
            // webpack.common.js
            module.exports = {
                // ...
                optimization: {
                    usedExports: true
                }
            }
            ```
        - MiniCssExtractPlugin 配置
            ```js
            // webpack.config.js
            const MiniCssExtractPlugin = require('mini-css-extract-plugin')
            const devMode = process.env.NODE_ENV !== 'production'

            module.exports = {
                plguins: [
                    new MiniCssExtractPlugin({
                        filename: devMode ? '[name].css' : '[name].[hash].css',
                        chunkFilename: devMode ? '[id].css' : '[id].[hash].css'  // 当没有被 html文件 直接引用时，就会走这条
                    })
                ],
                module: {
                    rules: [{
                        test: /\.(sa|sc|c)ss$/,
                        use: [
                            {
                                loader: MiniCssExtractPlugin.loader,
                                option: {
                                    hmr: process.env.NODE_ENV === 'development'
                                }
                            },
                            'css-loader',
                            'postcss-loader',
                            'sass-loader'
                        ]
                    }]
                }
            }
            ```
    - #### Css压缩插件
        - [Css压缩插件 官网](https://webpack.js.org/plugins/mini-css-extract-plugin/#minimizing-for-production)
        - 1.安装插件 ```npm i -D optimize-css-assets-webpack-plugin```
        ```js
        // webpack.common.js
        const path = require('path')
        const MiniCssExtractPlugin = require('mini-css-extract-plugin')
        const OptimizeCSSAssetsPlugin = require('optimize-css-assets-webpack-plugin')   // 第2步，引入插件

        module.exports = {
            entry: {
                main: './src/index.js'
            },
            module: {
                rules: [{
                    test: /\.css$/,
                    use: [
                        MiniCssExtractPlugin.loader,
                        'css-loader',
                        'postcss-loader'
                    ]
                }]
            },
            plugins: [ new MiniCssExtractPlugin({}) ],
            optimization: {
                minimizer: [new OptimizeCSSAssetsPlugin({})],   // 第3步，配置
                usedExports: true,
                splitChunks: {
                    chunks: 'all',
                    cacheGroups: {
                        vendors: false,
                        default: false
                    }
                }
            },
            output: {
                filename: '[name].js',
                path: path.resolve(__dirname, '../dist')
            }
        }
        ```
        
    - #### 如果希望把所有css打包到一个文件
        ```js
        // webpack.config.js
        const MiniCssExtractPlugin = require('mini-css-extract-plugin')

        module.exports = {
            optimization: {
                splitChunks: {  // MiniCssExtractPlugin 的底层也要借助 splitChunks 模块
                    cacheGroups: {
                        styles: {
                            name: 'styles',  // 命名为 styles
                            test: /\.css$/,
                            chunks: 'all',
                            enforce: true    // 忽略掉其他 splitChunks默认参数
                        }
                    }
                }
            },
            plugins: [
                new MiniCssExtractPlugin({
                    filename: '[name].css'
                })
            ],
            modules: {
                rules: [
                    {
                        test: /\.css$/,
                        use: [MiniCssExtractPlugin.loader, 'css-loader']
                    }
                ]
            }
        }
        ```

    - #### 如果希望, 根据入口不同，打包成不同的 css文件
        [【官网文档】如果希望, 根据入口不同，打包成不同的 css文件](https://webpack.js.org/plugins/mini-css-extract-plugin/#extracting-css-based-on-entry)

- ### 4-10 Webpack 与浏览器缓存（Caching）
    - 1.存在问题：
        ```js
        // index.js
        import _ from 'lodash'
        import $ from 'jquery'

        const dom = $('<div>')
        dom.html(_.join(['Dell','lee'],'----'))
        $('body').append(dom)
        ```
        这时候打包，由于引入了 lodash 和 jquery 两个库，打包文件会超过 244kb，会警告。关闭警告方法：```module.exports.performance: false```
        ```js
        // webpack.config.js
        const path = require('path')

        module.exports = {
            mode: 'production',
            performance: false,  // 关闭 webpack 打包警告
            entry: {
                main: './src/index.js'
            },
            optimization: {
                usedExports: true,
                splitChunks: {
                    chunks: 'all',
                    cacheGroups: {
                        vendor: {
                            test: /[\\/]node_modules[\\/]/,
                            name: 'vendors'
                        }
                    }
                }
            },
            output: {
                filename: '[name].js',
                path: path.resolve(__dirname, 'dist')
            }
        }
        ````
        
    - 2.由上所示，打包的结果为：```main.js``` 和 ```vendors.js``` 两个文件
        - 当你修改了业务代码，```main.js``` 后，重新打包并上传服务器
        - 当浏览器第二次请求这两个文件，由于文件名还是 ```main.js``` 所以，浏览器会从本地缓存中读取 ```main.js```，而不会重新向服务器请求。这样的话，你后面修改的业务代码，就没有被及时的更新到用户端
    - 3.```contentHash```
        - ```contentHash``` 的特性：如果文件内容不变，contentHash 的值也不会改变
        ```js
        // webpack.config.js
        const path = require('path')
        const CleanWebpackPlugin = require('clean-webpack-plugin');

        module.exports = {
            mode: 'development',
            entry: {
                main: './src/index.js'
            },
            optimization: {
                runtimeChunk: 'single',  // mainfest 独立打包成一个文件：runtime.js
                usedExports: true,
                splitChunks: {
                    chunks: 'all',
                    cacheGroups: {
                        vendor: {
                            test: /[\\/]node_modules[\\/]/,
                            name: 'vendors'
                        }
                    }
                }
            },
            plugins: [ new CleanWebpackPlugin({default: 'dist'}) ],
            output: {
                filename: '[name].[contenthash].js',  // 使用 contenthash 自动 hash命名
                path: path.resolve(__dirname, 'dist')
            }
        }
        ````
    - 4.可能存在问题
        - 在 webpack4 以前的旧版webpack中，即使使用了contenthash，在不改变代码，每次打包的 hash值 也可能不一样。
        - 原因：```mainfest``` 记录着，文件和文件直接的关系。
        - 在 webpack4 以前，```mainfest``` 既存在于业务代码中 ```main.js```，也存在于第三方库的打包代码中 ```vendors.js```
        - 由于每次打包的 ```mainfest``` 有所不同，所以旧版本webpack，即使不改变代码，每次打包的 hash值 也不一样
        - 解决方法：```module.exports.optimization.runtimeChunk: 'single'``` 将 mainfest 独立打包成一个文件即可

        
- ### 4-11 Shimming 垫片 的作用
    - #### 什么是 Shimming ？
        - 修改 webpack 默认行为，实现一些原始webpack 实现不了的效果，这种行为都叫做 Shimming 垫片行为
    - 1.来看一个问题
        ```js
        // index.js
        import $ from 'jquery';
        import _ from 'lodash';
        import { ui } from './jquery.ui';

        ui()

        const dom = $('<div>')
        dom.html(_.join(['dell','lee'], '---'))
        $('body').append(dom)
        ```
        ```js
        // jquery.ui.js
        export function ui(){
            $('body').css('background','red')   // 打包后执行，浏览器报错 $ is not defined
        }
        ```
        - <span style='color:red'>**报错原因：**</span>
            - webpack 是基于模块打包的，模块内的变量只能在模块内使用。
            - 由于模块的独立性，模块与模块之间，不会有任何的耦合，如果出了问题 直接到对应的模块找问题就行了。
        - 简单 又 笨的解决办法
            ```js
            // jquery.ui.js
            import $ from 'jquery';     // 直接再引入，但是由于这是第三方库，不是你的业务代码，所以直接引入是不太现实的

            export function ui(){
                $('body').css('background','red')   // 打包后执行，浏览器报错 $ is not defined
            }
            ```
    - #### 2.借助 Shimming 让jquery变成全局作用域
        ```js
        // webpack.config.js
        const path = require('path')
        const CleanWebpackPlugin = require('clean-webpack-plugin');
        const HtmlWebpackPlugin = require('html-webpack-plugin');
        const webpack = require('webpack')      // 第一步，引入webpack

        module.exports = {
            mode: 'development',
            entry: {
                main: './src/index.js'
            },
            optimization: {
                runtimeChunk: 'single',
                usedExports: true,
                splitChunks: {
                    chunks: 'all',
                    cacheGroups: {
                        vendor: {
                            test: /[\\/]node_modules[\\/]/,
                            name: 'vendors'
                        }
                    }
                }
            },
            plugins: [ 
                new CleanWebpackPlugin({default: 'dist'}),
                new HtmlWebpackPlugin(),
                new webpack.ProvidePlugin({     // 第二步，配置webpack.ProvidePlugin
                    $: 'jquery',         // 如果发现模块中出现 '$' ，就会自动在模块中自动引入 'jquery'模块。相当于 " import $ from 'jquery' "
                    _join: ['lodash','join']    //  将 lodash 模块中的 join ，定义为 _join
                })
            ],
            output: {
                filename: '[name].[contenthash].js',
                path: path.resolve(__dirname, 'dist')
            }
        }
        ```
        - 这样配置好后，```jquery.ui.js``` 里就不用再次 ```import $ from 'jquery'``` 了
        
        ```js
        // index.js
        import $ from 'jquery';
        import _ from 'lodash';
        import { ui } from './jquery.ui';

        ui()

        const dom = $('<div>')
        dom.html(_.join(['dell','lee'], '---'))
        $('body').append(dom)
        ```
        ```js
        // jquery.ui.js
        export function ui(){
            $('body').css('background','red')   // 打包后执行，浏览器报错 $ is not defined
            $('.main').css('background', _join(['green'], ''))  // 将 .main 背景色 通过lodash里的 join 设置为蓝色
        }
        ```
    - #### 3.修改模块中 this指向
        - 1.看一例子
            ```js
            // index.js
            console.log(this)   // object，实际上模块中的 this 默认指向 模块它自身
            console.log(this === window)   // false
            ```
        - 2.如果我现在就是想要 **每个js模块的 this 都指向 window** 该怎么办呢
            - 1.借助 ```npm i -D imports-loader```
            - 2.配置
                ```js
                // webpack.config.js
                const path = require('path')
                const CleanWebpackPlugin = require('clean-webpack-plugin');
                const HtmlWebpackPlugin = require('html-webpack-plugin');
                const webpack = require('webpack')

                module.exports = {
                    mode: 'development',
                    entry: {
                        main: './src/index.js'
                    },
                    module: {
                        // rules: [{
                        //     test: /\.js$/,
                        //     exclude: /node_modules/,
                        //     loader: 'babel-loader'   // 以前，我们遇到了js文件，都是使用 babel-loader 来处理
                        // }]
                        rules: [{
                            test: /\.js$/,
                            exclude: /node_modules/,
                            use: [{
                                loader: 'babel-loader',
                                query: {    // query这里配置该loader的参数
                                    presets: [[
                                        "@babel/preset-env", {
                                        targets: {
                                            chrome: "67",
                                        }
                                    }]]
                                },
                            },{
                                loader: 'imports-loader?this=>window'   // 借助 imports-loader 使得 this 指向 window
                            }]
                        }]
                    },
                    optimization: {
                        runtimeChunk: 'single',
                        usedExports: true,
                        splitChunks: {
                            chunks: 'all',
                            cacheGroups: {
                                vendors: {
                                    test: /[\\/]node_modules[\\/]/,
                                    name: 'vendors'
                                }
                            }
                        }
                    },
                    plugins: [ 
                        new CleanWebpackPlugin({default: 'dist'}),
                        new HtmlWebpackPlugin(),
                        new webpack.ProvidePlugin({
                            $: 'jquery'
                        })
                    ],
                    output: {
                        filename: '[name].[contenthash].js',
                        path: path.resolve(__dirname, 'dist')
                    }
                }
                ```
            - 3.配置完后，在执行 ```index.js``` this 就指向 window 了
                ```js
                // index.js
                console.log(this)   // window
                console.log(this === window)   // true
                ```

- ### 4-12 环境变量的使用方法
    - #### 本节主要讲解知识点：
        - 如何在webpack的打包过程中，去使用全局变量
        - 在很多脚手架的工具里面，你会发现，有时候他会通过全局变量 env.prodcution 的值来决定 到底是 **打包生产环境**，还是 **打包开发环境**
    - 1.先来看一个问题
        - 在过去，我们是这样实现 [Development 和 Production 模式的区分打包](#3最佳配置开发环境生产环境) 的
        ```js
        // package.json
        "scripts" : {
            "dev-build" : "webpack --config ./build/webpack.dev.js",        // 开发打包
            "dev" : "webpack-dev-server --config ./build/webpack.dev.js",   // 开发环境
            "build" : "webpack --config ./build/webpack.prod.js"            // 线上环境
        }
        ```
        - 之前，我们是借助两个配置文件实现打包的，分别是 ```webpack.dev.js``` ```webpack.prod.js```
        - 那么有没有另一种方法，也可以实现这种功能呢？
    - 2.另一种区分打包方法
        ```js
        // webpack.prod.js
        // const merge = require('webpack-merge')               // 第一步
        // const commonConfig = require('./webpack.common.js')  // 第一步

        const prodConfig = {
            mode: "production",
            devtool: "cheap-module-source-map"
        }

        // module.exports = merge(commonConfig, prodConfig)     // 第二步
        module.exports = prodConfig                             // 第二步
        ```
        ```js
        // webpack.dev.js
        const webpack = require('webpack')
        // const merge = require('webpack-merge')               // 第一步
        // const commonConfig = require('./webpack.common.js')  // 第一步

        const devConfig = {
            mode: 'development',
            devtool: 'cheap-module-eval-source-map',
            devServer: {
                contentBase: './dist',
                open: true,
                port: 8080,
                hot: true
            },
            plugins: [
                new webpack.HotModuleReplacementPlugin()
            ],
            optimization: {
                usedExports: true
            }
        }

        // module.exports = merge(commonConfig, devConfig)      // 第二步
        module.exports = devConfig                              // 第二步
        ```
        ```js
        // webpack.common.js
        const path = require('path')
        const HtmlWebpackPlugin = require('html-webpack-plugin')
        const CleanWebpackPlugin = require('clean-webpack-plugin')
        
        const merge = require('webpack-merge')              // 第一步
        const devConfig = require('./webpack.dev.js')       // 第一步
        const prodConfig = require('./webpack.prod.js')     // 第一步

        // module.exports = {           // 第二步
        const commonConfig = {          // 第二步
            entry: {
                main: './src/index.js'
            },
            module: {
                rules: [{
                    test: /\.js$/,
                    exclude: /node_modules/,
                    loader: 'babel-loader',
                    options: {
                        presets: [[
                            "@babel/preset-env", {
                            targets: {
                                edge: "17",
                                firefox: "60",
                                chrome: "67",
                                safari: "11.1",
                            },
                            useBuiltIns: 'usage'
                        }]]
                    }
                },{
                    test: /\.(jpg|png|gif)$/,
                    use: {
                        loader: 'url-loader',
                        options: {
                            name: '[name].[hash].[ext]',
                            outputPath: 'images/',
                            limit: 10240
                        }
                    }
                },{
                    test: /\.(eot|ttf|svg)$/,
                    use: {
                        loader: 'file-loader'
                    }
                },{
                    test: /\.scss$/,
                    use: [
                        'style-loader',
                        {
                            loader: 'css-loader',
                            options: {
                                importLoaders: 2
                            }
                        },
                        'sass-loader',
                        'postcss-loader'
                    ]
                },{
                    test: /\.css$/,
                    use: [
                        'style-loader',
                        'css-loader',
                        'postcss-loader'
                    ]
                }]
            },
            plugins: [
                new HtmlWebpackPlugin({
                    template: 'src/index.html'
                }),
                new CleanWebpackPlugin({
                    default: ['dist'],
                    root: path.resolve(__dirname, '../'),
                    // 由于 CleanWebpackPlugin 默认会认为，当前文件目录就是 根目录，所以要重写 根目录
                    
                    cleanOnceBeforeBuildPatterns: ['*.js', '!vendor', '!vendor.manifest.json']
                    // 这个参数配置要删除那些文件，和不要删除那些文件。要删除的文件'*.js'，不删除的文件前加个!
                })
            ],
            output: {
                filename: '[name].js',
                path: path.resolve(__dirname, '../dist')
            }
        }

        module.exports = (env) => {            // 第三步，导出一个函数
            if(env && env.production){
                return merge(commonConfig, prodConfig)   // 导出线上环境
            }else{
                return merge(commonConfig, devConfig)    // 导出生产环境
            }
        }
        // 如果外部传递给我 env变量，且env存在，env.production也存在，那么就是 线上环境。否则就是 开发环境
        // 这个模块到底是 导出线上环境，还是 导出生产环境，取决于你传递给他什么样的 env 变量
        ```
        ```js
        // package.json
        "scripts" : {
            // "dev-build" : "webpack --config ./build/webpack.dev.js",
               "dev-build" : "webpack --config ./build/webpack.common.js",
            // "dev" : "webpack-dev-server --config ./build/webpack.dev.js",
               "dev" : "webpack-dev-server --config ./build/webpack.common.js",
            // "build" : "webpack --config ./build/webpack.prod.js"
               "build" : "webpack --env.production --config ./build/webpack.common.js"
            // --env.production 在这里加这个的意思是：我通过全局变量，向webpack配置文件里传递一个属性production，它的值默认就是true
        }
        ```
        - 题外话：也可以这么传值
            - 1.传值方法1
                - ```"build" : "webpack --env production --config ./build/webpack.common.js"```
                - ```js
                    module.exports = (production) => {            // 改成接收 production
                        if(production){
                            return merge(commonConfig, prodConfig)   // 导出线上环境
                        }else{
                            return merge(commonConfig, devConfig)    // 导出生产环境
                        }
                    }
                  ```
            - 2.传值方法2
                - ```"build" : "webpack --env.production=abc --config ./build/webpack.common.js"```
                - ```js
                    module.exports = (production) => {            // 改成判断 production是否等于 abc
                        if(env && env.production === 'abc'){
                            return merge(commonConfig, prodConfig)   // 导出线上环境
                        }else{
                            return merge(commonConfig, devConfig)    // 导出生产环境
                        }
                    }
                  ```

## 第5章 Webpack 实战配置案例讲解
- ### 5-1 Library 的打包，自己开发一个库
    之前的讲解，都是对业务代码的打包，那么如果我们现在要开发一个库，该怎么办呢？
    - 1.自己开发一个库
        ```
        // 目录结构
        Library
            |- /src
                |- math.js
                |- index.js  公共入口文件
                |- string.js
            |- package.json
        ```
        ```js
        // math.js
        export function add(a,b){
            return a + b
        }
        export function minus(a,b){
            return a - b
        }
        export function multiply(a,b){
            return a * b
        }
        export function division(a,b){
            return a / b
        }
        ```
        ```js
        // string.js
        export function join(a,b){
            return a + " " + b
        }
        ```
        ```js
        // index.js  公共入口文件
        import * as math from './math'
        import * as string from './string'

        export default { math, string }
        ```
        - 我们新建的库，提供了 加减乘除 和 字符串拼接 的方法
        - 到这里，我们流程跟打包业务代码差不多
        - 但是，index.js 如果直接给浏览器执行的话，浏览器是执行不了的，所以我们还是需要对其进行打包
        ```js
        // package.json
        "script": {
            "build": "webpack"
        }
        ```
        ```js
        // webpack.config.js
        const path = require('paht')

        module.exports = {
            mode: 'production',
            entry: './src/index.js',
            output: {
                path: path.resolve(__dirname, 'dist'),
                filename: 'library.js',
                library: 'library',    // 支持<script src='xxx'>的引入方式，定义名为 'library' 的全局变量
                libraryTarget: 'umd'   // 支持 CommonJS、AMD、import 三种引入方式
                // libraryTarget 可选参数 window(浏览器环境下)、global(node环境下)
            }
        }
        ```
        ```js
        // 这个库要给别人用的，那么别人一般会怎么用这个库呢？
        import library from 'library'           // ES Module 引入

        const library = require('library')      // CommonJS 模块引入

        require(['library'], function(){})      // AMD 引入

        // 外部引入我们的库，可以有多种方法，所以我们要在 webpack.config.js 配置好，以支持多种引入方式。libraryTarget: 'umd'

        <script src='library.js'></script>
        library.math
        // 我们还希望能通过标签引入，并且引入后能直接使用 library. 的方式去调用里面的函数。
        // library: 'library', 把打包结果，挂载到全局变量 library 上，可以library. 的方式去调用里面的函数。
        // 在浏览器 console 面板，输入 'library' 即可测试是否存在该全局变量
        ```
    - 2.使用其他库的函数
        - 1.在上面的案例中，我们的 ```string.js``` 文件中，字符串方法是自己写的
            ```js
            // string.js
            export function join(a,b){
                return a + " " + b
            }
            ```
        - 2.那我突然发现，lodash.js库 中有一个功能很好，所以我希望在我自己开发的这个库中使用部分lodash 的功能
            ```js
            // string.js
            import _ form 'lodash'              // 引入 lodash

            export function join(a,b){
                return _.join([a, b], ' ');     // 使用 lodash.join 方法
            }
            ```
            - 这样的话，我们就把之前的 ```string.js``` 改写成了现在这样，并且使用了 lodash.js库 中的方法
        - 3.但是，在打包后发现，打包的结果 ```library.js``` 文件变成了 70.8Kb , 之前没引用 lodash 时只有 1.5kb ， 因为我的库中引入的 lodash库
            - 别人在使用的时候，可能是这样
                ```js
                import _ from 'lodash'
                import library from 'library'
                ```
            - 这样就会出现问题
                - 1.我们自己开发的库 library 里已经把 lodash 打包进去了
                - 2.别人在写业务代码的时候，也可能使用 lodash
                - 3.这样的话，lodash 就被重复打包了，浪费性能
            - 解决办法
                ```js
                // webpack.config.js
                const path = require('paht')

                module.exports = {
                    mode: 'production',
                    entry: './src/index.js',
                    externals: ['lodash'],  // 打包过程中，如果遇到 lodash，就忽略它，不要把它打包进去
                    output: {
                        path: path.resolve(__dirname, 'dist'),
                        filename: 'library.js',
                        library: 'library',
                        libraryTarget: 'umd'
                    }
                }
                ```
                - 这时候，再打包 ```library.js``` 文件就又变回了 1.5Kb
                - 但是，假如别人引用我这个 ```library.js```库 的时候，如果是这样
                    ```js
                    import library from 'library'   // 仅仅这样，是无法使用的，因为 library 依赖于 lodash
                    ```
                - 所以，别人在用 ```library.js```库 的时候，应该
                    ```js
                    import _ from 'lodash'
                    import library from 'library'
                    ```
            - [externals 的具体使用方法参照 webpack官网](https://webpack.js.org/configuration/externals)
                ```js
                // webpack.config.js
                module.exports = {

                    externals : {
                        lodash : {
                            commonjs: 'lodash',     // const lodash = require('lodash')
                            amd: 'lodash',
                            root: '_'               // 通过 script 标签引入的全局变量, 变量名为 '_'

                            // key的值为变量名，
                            // 如 commonjs: 'lodash'
                            // 则必须为 const lodash = require('lodash')
                            // 而不能   const _ = require('lodash')
                        }
                    },

                    // 或者

                    externals : {
                        subtract : {
                            root: ['math', 'subtract']
                        }
                    }
                };
                ```
        - 4.上线发布到 npm 官方库中
            - 1.配置入口文件
                ```js
                // package.json
                {
                    "name": "library",              // 给你的库命名，但是不要与npm已有库重名
                    "main": "./dist/library.js"     // 这里入口文件，要改成我们要给别人使用的 library.js
                }
                ```
            - 2.到 npm 官网注册一个账号
            - 3.命令行执行 ```npm adduser```，添加用户名和密码
            - 4.```npm publish``` 把你的这个项目发布到 npm 官方仓库里去
            - 5.发布成功后，别人要使用就 ```npm i 你的仓库名```