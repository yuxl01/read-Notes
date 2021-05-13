# WebPack
          Webpack具有Grunt、Gulp对于静态资源自动化构建的能力，但更重要的是，Webpack弥补了requireJS在模块化方面的缺陷
    同时兼容AMD与CMD的模块加载规范，具有更强大的JS模块化的功能。

#### 一、JS规范

|名称|描述|优点|缺点|使用方法|
|-|-|-|-|-|
|CommonJS|同步require|1.模块可以被复用</br>2.npm支持</br>3.容易上手使用|1.阻塞的调用,网络是异步的</br>2.无法同步require多个模块| require("module");</br>module.exports = someValue;|
|AMD|异步require|1.能满足网络请求的异步需求</br>2.能同步加载多个模块|1.代码复杂,更难写也更难读</br>2.似乎是一种绕路的笨办法|require(["module","../file.js"],function(module,file){/*...*/});</br>define("mymodule",["dep1","dep2"],function(d1,d2){return someExportValue;});|
|CMD|一个模块就是一个文件职责单一简单纯粹|...|...|define(function(require,exports,module){//模块代码});|



#### 二、WebPack使用

    任何静态资源都可以视作模块,然后模块之间也可以相互依赖，通过webpack对模块进行处理后，可以打包成我们想要的静态资源。
        
###### 1、全局安装webpack

 - npm init --初始化项目
 - npm install webpack -g  --全局安装webpack
 - npm install webpack@2.0.1 -g --安装指定版本
 
 ```.json
       {
  "name": "WebPack-Test",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack --config webpack.config.js",
    "dev": "webpack-dev-server"
  },
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "css-loader": "^5.2.4",
    "html-webpack-plugin": "^4.3.0",
    "less": "^3.11.3",
    "less-loader": "^6.1.0",
    "style-loader": "^2.0.0",
    "webpack": "^4.43.0",
    "webpack-cli": "^3.3.12",
    "webpack-dev-server": "^3.11.0"
  },
  "dependencies": {}
}

 ```
 
###### 2、编写配置文件
 
`更改webpack配置文件名字：打包用 webpack --config webpack.config.custom.js`

      

 - 1.在项目根目录下创建webpack.config.js

 - 2.定义module.exports对象配置对应参数

   
 ```.js
 const path = require("path"),
  HtmlWebpackPlugin1 = require("html-webpack-plugin");

module.exports = {
  mode: "development",//production development
  //配置内部服务器
  devServer: {
    port: 4200,
    progress: true,
    contentBase: "./dist",
  },
  //入口地址
  entry: "./src/index.js",
  //输出目录
  output: {
    filename: "bundle.js",
    path: path.resolve(__dirname, "dist"),
  },
  //插件
  plugins: [
    //打包html 插件
    new HtmlWebpackPlugin1({
      template: "./src/index.html",
      filename: "index.html",
      minify: {
        //删除标点
        removeAttributeQuotes: true,
        //折叠为一行
        collapseWhitespace: true,
      },
      hash: true,
    }),
  ],
  module: {
    //规则 css-loader
    // style-loader 将css插入到html
    // loader 加载顺序默认从右向左执行
    rules: [
      {
        test: /\.css$/,
        use: ["style-loader", "css-loader"],
      },
      // loader 加载顺序默认从右向左执行
      {
        test: /\.less$/,
        use: ["style-loader", "css-loader", "less-loader"],
      },
    ],
  },
};

 ```
###### 3、webpack loader加载器
       加载器主要做一些预处理的工作
       
   - 1.安装loader
     - npm install babel-loader babel babel-core 用以解析js
     - npm install css-loader style-loader  用以解析css
     - npm install url-loader file-loader 用以解析文件图片
     - npm install less-loader less  用以解析less写的文件
     
   - 2.配置webpack.config.js
     - 加入module对象属性，添加rules规则
        - { test: /.js$/, use: 'babel-loader' },
        - { test: /.css$/, use: ['style-loader', 'css-loader'] },
        - {test:/.(jpg|png)$/,use:'url-loader?limit=8192&name=./[name].[ext]'}
        
  - ###### `demo`
  ```.js
  module: {
        rules: [
            //加载js loader
            { test: /.js$/, use: 'babel-loader' },
            //style loader 和css-Loader
            { test: /.css$/, use: ['style-loader', 'css-loader'] },
            //{test:/.(jpg|png)$/,use:'url-loader?limit=8192&name=./[name].[ext]'}
            //打包图片  url-loader依赖file-loader
            {
                test: /.(jpg|svg|gif|png)$/, use: [{
                    loader: 'url-loader',
                    // loader: 'file-loader',
                    options: {
                        esModule: false, // 这里设置为false不然图片显示不出来
                        name: '[name].[ext]',
                        limit: 8192,
                        outputPath: './static',//输出路径
                        publicPath: './dist/static/'//查找路径
                    }
                }]
            }
        ]
    }
  ```
###### 4、css 独立打包

 

