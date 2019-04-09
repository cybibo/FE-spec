#Webpack项目架构DEMO  
>  这里展示的一个Webpack项目最基础的文件与文件夹结构，以及Webpack配置说明，项目源码查看：MTI-FE-Frame  

##项目目录结构
```
├── README.md
├── build                       # 编译输出目录
|  ├── build.js
|  ├── check-versions.js
|  ├── logo.png
|  ├── utils.js
|  ├── vue-loader.conf.js
|  ├── webpack.base.conf.js     # webpack公用的基础配置
|  ├── webpack.dev.conf.js
|  ├── webpack.prod.conf.js
|  └── webpack.test.conf.js
├── config                      # 环境配置文件
|  ├── dev.env.js
|  ├── index.js
|  ├── prod.env.js
|  └── test.env.js
├── docs                        # 项目文档
├── index.html
├── node_modules                # node安装依赖包
├── package-lock.json
├── package.json
├── src                         # 项目源码目录
|  ├── App.vue
|  ├── api                      # API请求
|  ├── assets                   # 图片等资源文件,目录中的文件会被webpack处理解析为模块依赖，只支持相对路径形式
|  ├── components               # 通用组件
|  ├── main.js                  # 入口文件
|  ├── mock                     # API请求拦截器
|  ├── pages                    # 页面文件夹
|  ├── resources                # mock 数据源
|  ├── router                   # 路由文件夹
|  ├── store                    # 状态管理文件夹
|  └── utils                    # 工具库文件夹
├── static                      # 静态资源文件，目录下的文件并不会被Webpack处理:会直接被复制到最终的打包目录下。必须使用绝对路径
|  └── styles                   # 通用样式文件夹
└── test                        # 测试案例文件夹
   ├── e2e
   └── unit
├── .babelrc                    # 设置转码的规则和插件
├── package.json                # 项目信息
├── .eslintrc                   # Eslint配置
```

##命名的规则
开发最难的是什么？命名！

记住一个法宝：名称前用 sel,  sear, get, del, add, upda, edit。

前端开发规范：http://10.168.4.143/都有详细说明,

在这里主要强调一下的规则
###html的命名规则
HTML 属性应当按照以下给出的顺序依次排列

###css的命名规则
声明的顺序问题

###javascript的命名规则
####1.api的命名规则
命名规则：驼峰式命名方式，动词放前面，名词放后面，


####2.组件通信之间的命名规则
子组件里
```
this.$emit(“addalarm”, data)
```
emit会用小写，
父组件里还是用驼峰式命名
@addalarm =“addAlarm”,这样的话也符合函数的命令规则。

如果是中央通信的话，
```
bus.$on(‘sel-round-item’,(data) => {})
bus.$emit(‘sel-round-item’,data)
```
也全是小写，单词与单词之间采用中间带“-”的这种形式

####3.组件的命名规则
Components目录下每一个文件夹就是一个组件，
每一个文件夹下面的文件名称是index.vue,最好每个文件夹下面还有一个index.js，因为引用的时候import,它会默认去找文件夹下面的index.js，所以你可以少写一个index.vue

####4.模块的命名规则
pages目录下每个文件夹就是一个模块。同样，每一个文件夹下面的输出文件名称是index.vue,

####5.静态资源存放规则
不经常修改的，大一点的图片，比如大于10k的图片，放在static/images/目录下，免得每个编译都要花很长时间去做图片的编译。
经常修改的，小一点的图片，放在assets/images/目录下。


##API请求调用方式
```
http.post({     #post方法
  api:apis.url,
  param:{
    param1:param1,
    param2:param2
  }
}).then(res => {
  if(res.code === 200) {
  ...
  }else{
  ...
  }
})
或
http.get({      #get方法
  api:apis.url,
  param:{
    param1:param1,
    param2:param2
  }
}).then(res => {
  if(res.code === 200) {
  ...
  }else{
  ...
  }
})
或  
http.delete({      #delete方法
  api:apis.url,
  param:{
    param1:param1,
    param2:param2
  }
}).then(res => {
  if(res.code === 200) {
  ...
  }else{
  ...
  }
})
或  
http.put({      #put方法
  api:apis.url,
  param:{
    param1:param1,
    param2:param2
  }
}).then(res => {
  if(res.code === 200) {
  ...
  }else{
  ...
  }
})
```
##项目发布后的结构
```
    dist        #存放需要发布的文件
    |--css      #这里是所有编译好的less转css的文件
    |--fonts    #这里是static/目录下拷贝过来的字体
    |--images   #这里是static/目录下拷贝过来的图片
    |--img      #所有需要发布的img
    |--styles   #这里是static/目录下拷贝过来的css
    |--js       #这里是已经自动打包合并好的js和css文件，以及lib合并文件
    |--index.html     //修改完毕的html
```
##Webpack相关知识  
如何安装Webpack，如何使用来开发，如何使用来打包发布等

###1.安装Webpack与相关插件  
*  安装Nodejs，直接到官网下载安装http://nodejs.org/，目前只能安装v6.11以上版本，而且只能是v6系列  
*  安装webpack，在命令行下，输入：
```
    npm install -g webpack webpack-cli webpack-dev-server //webpack核心、命令行工具、调试服务
```
*  安装webpack需要的插件(下面路径中用户名为各自电脑的用户名)  

*  拷贝Mti-Gis-Demo中的package.json文件到 C:\Users\用户名\AppData\Roaming\npm下  
*  在命令行中输入：npm install进行插件安装  
*  添加名为：NODE_PATH，值为：C:\Users\用户名\AppData\Roaming\npm\node_modules到系统的环境变量中.  
*  进入C:\Users\用户名\AppData\Roaming\npm\node_modules\webpack\node_modules\enhanced-resolve\lib\路径.  
*  修改ModulesInHierachicDirectoriesPlugin.js文件，在第26行后面回车，就是forEachBail循环前面，添加代码addrs.push(process.env.NODE_PATH);   
*  重新打开cmd命令行，即可全局使用webpack.  
*  Mac安装方法请看此云笔记mac下修改全局webpack支持操作方法.  

###2.如何使用Webpack   
*  html文件，需要放在src根目录下. 
```
    //表示将此html文件内容引用到该标签位置
    <link rel="import" href="inline/index.html?__inline">
    //项目的css与js文件，不需要单独引用，webpack会打包
```
*  css和less文件.  
        需要自动合并为雪碧图的图片，在引用文件名后添加?__sprite，而且路径必须带双引号，单个css文件中所有添加此标记的，会合并成一个单独文件  
        那些被合并的原图，文件名必须以下划线_开头，这样打包发布，就不会出现未合并之前的图片，详情参考源码  

* js文件  

        entry下的入口js文件，需要单独引用页面的less和css文件
        每一个单独的js文件最终都会打包成为一个单独模块，变量也是模块内部变量  
        项目内模块加载使用import方式  
        组件加载废弃nie.require方式，采用nie.pack  
        可以使用ES6、7语法特性  
```
    //引用CSS文件
    require('css/index.less');

    //引入html文件，最终返回html内容的字符串
    import inline_html from '../../inline/header.html';

    //js中，需要引用到资源的文件，需要这样写，打包时候才会替换为CDN路径
    import img_url from '../../img/demo.png';

    //引用art-template模板文件，模板语法为js原生语法
    let under = require("../../template/header.art");
    let html = under({list : [1,2,3]});

    //引用项目内其他模块
    import Common from '../app/index.js';

    //加载组内的组件，兼容ie8,9浏览器
    nie.pack(["nie.util.videoV2","nie.util.fur3"]).then((rets)=>{
        //获取组件对应的引用，名字与组件最后一个单词相同
        let {videoV2,fur3} = rets;
    });

    //加载组内的组件方式2，只支持ie10，chrome，移动端
    async init(){
        //获取组件引用
        let {fur3,videoV2} = await nie.pack(["nie.util.fur3","nie.util.videoV2"]);
    }
    init();

    //默认自带两个变量
    let isDebug = __DEBUG ; //编译打包后，测试环境会变为true，正式环境为false
    let cdn = __CDNPATH ; //编译后，会变成对应的cdn-path值

    //返回模块接口
    export default  {
        init : init,
        show : show
    }
```

###3.如何开始Webpack  
*  进入项目的文件夹，输入命令：
```
    npm run dev //启动webpack服务，并且监听文件变化，自动编译，刷新浏览器，服务默认：http://127.0.0.1:9000
    npm run build:test //打为测试环境的包
    npm run build //打为发布环境的包
```

###4.注意事项  
*  使用webpack，不支持IE6、7浏览器.  
*  尽量使用ES6的特性.  
*  强制模块化开发，一个文件即一个模块.  
*  默认开启热更新模式，修改html文件不会刷新页面和内容，修改js与less则只刷新对应的内容，不刷页面；需要刷新，请在webpack.dev.conf.js中去掉hot:true.  
*  node_modules禁止提交上git.  
*  需要针对项目的特殊，修改配置文件.  