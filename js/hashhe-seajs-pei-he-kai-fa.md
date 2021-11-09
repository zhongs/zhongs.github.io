# hash和seajs配合开发

* 页面引入seajs

```
<script src="https://cdn.bootcss.com/seajs/3.0.2/sea.js" data-main="main"></script>

data-mian 是入口文件
```

* main.js

```
define(function(require, exports, module){
    //首次进入页面
    manageHash();
    //只要hash改变就会触发这个方法
    window.onhashchange = manageHash;

    function manageHash(){
        var hash = window.location.hash.substring(1) || 'index';
        swicth(hash){
            case 'page1':
                require('./page1.js').init();
            case 'page2':
                require('./page2.js').init();
            case 'page3':
                require('./page3.js').init();
        }
    }
});
```

* page1.js    page2.js    page3.js&#x20;

```
define(function(require, exports, module){
    var page = {
        init:function(){}
    }

    module.exports = page;
});
```
