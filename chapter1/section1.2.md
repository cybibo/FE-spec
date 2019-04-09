#PC页面制作规范
##概述
本文档概述道枢信息PC端页面制作规范，主要为了更好的团队协作，保证前端实现的一致性，提升网站的可维护性。


> 总则

> 统一使用UTF-8编码，除非是旧站点而且需要用到include顶条的用GBK(GB2312)。  
> 命名使用英文小写，词组用'-'连接，力争做到贴切且见名知意。  
> 代码应添加必要的注释，注释有意义且易懂。  
> 代码需做到易于他人阅读的，便于维护管理。  
> 代码版本管理统一使用git。 
 
##移动、PC跳转. 
PC专题如有对应的移动端，需要在页面加入适配跳转代码，请将下面代码完整加在head标签css引入之前，不用漏了. <!--ignore-->
```
<script type=text/javascript>
  !function(){
    function params(u, p){
      var m = new RegExp("(?:&|/?)"+p+"=([^&$]+)").exec(u);
      return m ? m[1] : '';
    }
    if(/iphone|android|ipod/i.test(navigator.userAgent.toLowerCase()) == true && params(location.search, from) != mobile){
      location.href = '移动端地址';
    }
  }();
</script><!--ignore-->
```
移动端需要判断是PC打开，自动跳转到PC页面，请将下面代码完整加在head标签css引入之前，不用漏了<!--ignore-->
```
<script type=text/javascript>
  !function(){
    if(/iphone|android|ipod/i.test(navigator.userAgent.toLowerCase()) == false && params(location.search, from) != 'desktop'){
      location.href = 'PC地址';
    }
  }();
</script><!--ignore-->
```
##文件命名
*  所有文件名统一使用小写  
*  文件名禁止以数字开头  
*  HTML 首页、引导页命名为index.html  
*  HTML 官网栏目命名，按官网目录结构&命名参考，页面命名为index.html  
*  HTML 专题内页，有明显分类，按英文 或 拼命首字母命名+首字母+尾字母，如：标题为“这是一个充值活动”，页面名为“zsygczhdzd.html”  
*  HTML 专题内页，有明显分类、带参数，如：标题为“充值活动-充值返利”，页面名为“czhdcd.html?page=czflcl”或“czhdcd.html#page=czflcl”  
*  HTML 专题内页，无明显分类、带参数，如：只有一个内页通过参数展示不同内容，页面名为“page.html?page=czflcl”或“page.html#page=czflcl”  

##HTML规范  
*  编写原则  
*  遵循w3c标准，构建良好的 HTML 结构和语义。  
*  保证 HTML 代码整洁工整，结构清晰。可使用相关工具格式化  
*  HTML 里的类名、ID 名，全部小写，词组用'-'连接  
*  静态页面必须做好足够详细的注释。  
*  推荐将所有JS放到页面底部。  
*  HTML代码模板示例  

##HTML代码模板示例
```
<!DOCTYPE html>

<!doctype html>
<html>
<head>
<meta charset="utf-8">
<!--include virtual="/global_include/config.html"-->
<title><!--#echo var='Title'--></title>
<meta name="keywords" content="<!--#echo var='Keywords'-->" />
<meta name="description" content="<!--#echo var='Description'-->" />
<link rel="dns-prefetch" href="https://daoshu.com">

<!-- 适配跳转代码 -->
<script type="text/javascript" data-fixed="true">
  !function(){
    function params(u, p){
      var m = new RegExp('(?:&|/?)'+ p +'=([^&$]+)').exec(u);
      return m ? m[1] : '';
    }
    if(/iphone|ios|android|ipod/i.test(navigator.userAgent.toLowerCase()) == true && params(location.search, "from") != "mobile"){
      location.href = '移动端地址';
    }
  }();
</script><!--ignore-->

<!-- 样式 -->
<!--usemin css(pkg/boot.css)-->
<link rel="stylesheet" type="text/css" href="css/index.less" />
<!--end-->

<!--[if lt IE 9]>
<script src="https://daoshu.com/comm/html5/html5shiv.js"></script>
<![endif]-->
</head>
<body>
  <!--分享信息-->
  <div id="share_content" style="display:none">
    <div id="share_title" pub-name="分享文案" ></div>
    <div id="share_url" pub-name="分享地址" ></div>
    <div id="share_desc" pub-name="分享文本" ></div>
    <img id="share_pic" data-src="#" pub-name="分享图片" />
  </div>

  <!--SEO start-->
    <h1 class="hide"></h1>
  <!--SEO end-->

  <!--【顶条】官网首页统一使用include，其他页面采用<div id="NIE-topBar"></div>的方式-->
  <!--#include virtual="/global_site_inc/topBar-include.html"-->

  <div class="wrap">

  </div>

  <!--版权-->
  <div id="NIE-copyRight"></div>

<!-- jquery mix NIE (最新版本）-->
<script src="https://daoshu.com/comm/js/jquery(mixNIE).1.11.js"></script>

<!--usemin js(pkg/index.js)-->
<script src="js/app/index.js"></script>
<!--end-->
</body>
</html>
```

