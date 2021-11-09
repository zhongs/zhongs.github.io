# 事件委托

* 事件委托

```
var node = document.querySelector('ul');
node.addEventListener('click',function(e){
    var e = e || window.event;
    var target = e.target || e.srcElement;
    if(target.nodeName.toLowerCase() == 'li'){
        target.style.background = 'red';
    }
},false);
```
