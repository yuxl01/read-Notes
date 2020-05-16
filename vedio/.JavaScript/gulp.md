# Gulp
  
#### 一、基本使用

    gulp3.0压缩顺序默认是顺序执行的，4.0引入压缩并行的处理方式：当前是4.0
    
|--save-dev|--save|
|-|-|
|表示开发过程用的包|上线运行依赖的包|
    
###### 1.安装Gulp及基本api

   - 1.全局安装 npm install gulp -g
   
|gulp.task|gulp.src|gulp.dest|gulp.watch|gulp.series|gulp.parallel|
|-|-|-|-|-|-|
|创建任务|文件目录|写入目录|监听任务|用于串行（顺序）执行|用于并行（顺序）执行|
  
###### 2.使用gulp

   - 1.创建gulp配置文件 gulpfile.js
   
   - 2.引入gulp  const [gulp, less] = [require('gulp'), require('gulp-less')]
   
   - 3.创建执行任务 gulp.task("default",gulp.series(['task'])); 默认任务

###### 3.监听任务
    
   - 1.创建监听任务 gulp.task('watch',()=>{gulp.watch(['filename']),gulp.series(['task']) })
    
###### 4.实现demo
```.js
 
const [gulp, less] = [require('gulp'), require('gulp-less')]

//由default任务触发，再default触发之前会触发html任务
//触发之后的回调
gulp.task("html", ()=>{
       return gulp.src('./src/index.html').pipe(gulp.dest("./dist"));
});

//创建监听
gulp.task("watch", ()=>{
        return gulp.watch(['./src/index.html'],gulp.series(['html']))    
});

//默认任务，终端输入gulp 触发
gulp.task("default", gulp.series(['html','watch']));
 
```

#### 二、Gulp插件

###### 1.gul-connect 开启服务器

  - 1.安装 npm instaall gulp-connect --save-dev 表示开发过程用到的包
  
  - 2.引入const conntect require('gulp-connect')
  
  - 3.创建任务启动服务 gulp .task('server',(){//开启服务})
  
###### 2.实例demo

```.js
//定义引入服务器对象
 const connect = require('gulp-connect');
 
 //创建一个将由默认task执行的任务
 gulp.task('server', () => {
   return conntect.server({
                port: 8090,
                livereload:true
        });
})
/对页面任务输出后加入浏览器监听中间件
gulp.task("html", () => {
        return gulp.src('./src/index.html').pipe(gulp.dest("./dist")).pipe(conntect.reload());
});

//在默认任务中加入新增服务器任务
//这里传入参数执行顺序应该是并行的，不然watch任务执行会阻塞导致后面任务不执行
gulp.task("default", gulp.parallel(['html', 'watch','server']));
```
          
