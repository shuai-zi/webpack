# webpack
介绍webpack+vue入门过程
Webpack菜鸟入门

webpack+vue的开发
## 一.环境配置
Webpack基于node环境，所以必须先安装node 安装完之后，命令输入node -v 显示版本号则表示node安装成功。
Node安装成功npm也会安装，命令npm -v显示版本号则表示安装成功

国外npm安装比较慢，推荐国内淘宝镜像
```
npm install -g cnpm --registry=https://registry.npm.taobao.org
```
全局安装（推荐） 
```
cnpm install webpack -g
```
局部安装到项目中
```
cnpm install webpack --save-dev
```
## 二.创建项目
1.创建文件夹 webpack-vue

2.进入文件目录输入命令 
```
cnpm install vue-cli -g
```
3.输入命令
```
vue init webpack vueApp
```
运行之后需要填写一些信息，这里我们只填写第一个name项app其他直接回车默认

5.之后命令进入项目目录 cd vueApp然后安装项目依赖输入命令 cnpm install （重要）安装时间较长，需要等待，安装完全之后项目构建完成

6.安装完成后，命令输入
```
cnpm run dev
```
将会自动打开浏览器地址为
```
"http://localhost:8080/#/"
```
的初始化Vue例子，这时候修改项目保存，页面将自动实时刷新，并且编写代码过程出错，或显示屏幕上提醒，对开发过程来说效率大大的提高。

如果你觉得输入命令过于长而麻烦易出错：你可以进入package.json文件修改”scripts”:{...}对象更改自己喜欢的命令，记住双引号和逗号，不然会报错。

7.打包上线。当项目开发完成后，使用命令cnpm run build它将会生成一个dist文件夹，所有必要的代码都被压缩打包放进去了，此时文件后缀自动加hash值，html引入也自动补上hash后缀，然后只需要将dist文件夹放上服务器就上线成功。

8.单文件Vue组件“.vue”后缀格式

9.打包之后的回调操作，替换成.min文件
```
plugins: [
		//编译完后触发'done'事件
		//需要安装插件 cheerio
		//cnpm install cheerio --save-dev
        function(){
        	let fag = false  //防止重复操作
            this.plugin('done', (stats) => {
            	const hash = stats.hash
                fs.readFile('./dist/static/index.html', (err, data) => {
                	if(!data) return
                    const $ = cheerio.load(data.toString());
                    //重新引入带hash的js文件
                    if(fag||process.env.NODE_ENV !== 'production') return
                    $('script[src*="vue.2.5.2"]').attr('src', 'http://afajr.oss-cn-hangzhou.aliyuncs.com/pcweb/js/vue.2.5.2.min.js');
                    fag = true
                    fs.writeFile('./dist/static/index.html', $.html(), err => {
                        !err && console.log('Set has success: '+hash,new Date().getHours()+':'+new Date().getMinutes())
                    })
                })
            })
        },
	]
```


### vue组件开发笔记
1. 子组件直接触发父组件方法
```
<box @ChildrenGet = "parentFun"></box>   this.$emit("get",msg) 
 或
this.$parent.parentFun(msg)
```
2. vue-路由传参数
home.vue 通过query来传递num参数为1，相当与在 url 地址后面拼接参数
```
<template> 
  <div> 
    <h3>首页</h3> 
    <router-link :to="{ path:'/home/game', query: { num: 1} }"> 
      <button>显示<tton> 
    </router-link> 
  
    <router-view></router-view> 
  </div> 
</template> 
game.vue 子路由中通过 this.$route.query.num 来显示传递过来的参数

<template> 
  <h3>王者荣耀{{ this.$route.query.num }}</h3> 
</template>
```
输出：王者荣耀1
