---
title: javascript设计模式
date: 2017-11-26 19:29:57
tags: javascript
---
<img align="center" src="http://7xqrwh.com1.z0.glb.clouddn.com/image/designMode/1.jpg" width="80%" height="50%">
在合适的时间，看到合适的书是幸运的！文中的例子摘自《JavaScript设计模式与开发实践》这本书；
<!-- more -->

## 单例模式

>保证一个类仅有一个实例，并提供一个访问它的全局访问点；

登录弹窗

````javascript
    var getSingle = function(fn){
        var result;
        return function(){
            return result || (result = fn.apply(this, arguments));
        }
    };

    var createLoginLayer = function(){
        var div = document.createElement('div');
        div.innerHTML = '我是登录弹窗';
        div.style.display = 'none';
        document.body.appendChild(div);
        return div;
    };

    var createSingleLoginLayer = getSingle( createLoginLayer );

    document.getElementById('loginBtn').onclick = function(){
        var loginLayer = createSingleLoginLayer();
        loginLayer.style.display = 'block';
    }
````

## 策略模式

>定义一系列的算法，把他们一个个封装起来，并且使它们可以相互替换

表单验证

````javascript
    //策略对象
    var strategies = {
        isNonEmpty: function( value, errorMsg ){
            if( value==='' ){
                return errorMsg;
            }
        },
        minLength: function( value, length, errorMsg ){
            if( value.length < length ){
                return errorMsg;
            }
        },
        isMobile: function( value, errorMsg ){
            if( !/(^1[3|5|8][0-9]{9}$)/.test( value ) ){
                return errorMsg;
            }
        }
    };

    //Validator 类
    var Validator = function(){
        this.cache = [];
    };

    Validator.prototype.add = function( dom, rules ){
        var self = this;
        for( var i=0, rule; rule = rules[ i++ ]; ){
            (function(rule){
                var strategyAry = rule.strategy.split(':');
                var errorMsg = rule.errorMsg;
                self.cache.push(function(){
                    var strategy = strategyAry.shift();
                    strategyAry.unshift( dom.value );
                    strategyAry.push(errorMsg);
                    return strategies[ strategy ].apply( dom, strategyAry );
                });
            })(rule)
        }
    };

    Validator.prototype.start = function(){
        for( var i = 0, validatorFunc; validatorFunc = this.cache[ i++ ]; ){
            var errorMsg = validatorFunc();
            if( errorMsg ){
                return errorMsg;
            }
        }
    };

    //客户调用代码
    var registerForm = document.getElementById( 'registerForm' );

    var validateFunc = function(){
        var validator = new Validator();
        validator.add( registerForm.userName,
         [{ strategy: 'isNonEmpty', errorMsg: '用户名不能为空' },
          { strategy: 'minLength:10', errorMsg: '用户名长度不能小于10位' }]
        );

        validator.add( registerForm.password,[{ 
            strategy: 'minLength:6', 
            errorMsg: '密码长度不能小于6位' 
            }]);

        validator.add( registerForm.phoneNumber,[{ 
            strategy: 'isMobile', 
            errorMsg: '手机号码格式不正确' 
            }]);

        var errorMsg = validator.start();
        return errorMsg;
    };

    registerForm.onsubmit = function(){
        var errorMsg = validataFunc();
        if( errorMsg ){
            alert( errorMsg );
            return false;
        }
    };

````

## 代理模式

>为一个对象提供一个代用品或占位符，以便控制对它的访问。

图片预加载

````javascript
    var myImage = (function(){
        var imgNode = document.createElement('img');
        document.body.appendChild( imgNode );
        return function(){
            imgNode.src = src;
        }
    })();

    var proxyImage = (function(){
        var img = new Image;
        img.onload = function(){
            myImage( this.src );
        }
        return function( src ){
            myImage('http://sucimg.itc.cn/avatarimg/903387522_1510992087902_c55');
            img.src = src;
        }
    })();

    proxyImage('http://7xqrwh.com1.z0.glb.clouddn.com/image/designMode/1.jpg');
````

## 迭代器模式

>提供一种方法顺序访问一个聚合对象中的各个元素，而又不需要暴露该对象的内部表示。

外部迭代器

````javascript
    var Iterator = function( obj ){
        var current = 0;
        var next = function(){
            current += 1;
        }
        var isDone = function(){
            return current >= obj.length;
        }
        var getCurrItem = function(){
            return obj[current];
        }
        return {
            next:next,
            isDone:isDone,
            getCurrItem:getCurrItem
        }
    };

    //比较2个数组是否相等
    var compare = function(iterator1, iterator2){
        while(!iterator1.isDone() && !iterator2.isDone()){
            if(iterator1.getCurrItem() !== iterator2.getCurrItem()){
                throw new Error('iterator1 和 iterator2不相等');
            }
            iteration1.next();
            iteration2.next();
        }
        alert('iterator1 和 iterator2相等');
    }

    var iterator1 = Iterator([1,2,3]);
    var iterator2 = Iterator([1,2,3]);

    cpmpare(iterator1, iterator2);
````

倒序迭代器

````javascript
    var reverseEach = function(ary, callback){
        for( var l = arr.length - 1; l>=0; l--; ){
            callback(l, ary[l]);
        }
    };

    reverseEach([0,1,2], function(i, n){
        console.log(n);  //2,1,0
    });
````

终止迭代器

````javascript
    var each = function(ary, callback){
        for( var i=0; i<ary.length; i<l; i++ ){
            if( callback(i, ary[i]) === false ){
                break;
            }
        }
    };

    each([1,2,3,4,5], function(i, n){
        if(n>3){
            return false;
        }
        console.log(n); //1,2,3
    })
````

## 发布订阅模式

>发布订阅模式又叫观察者模式，它定义对象间的一种一对多的依赖关系，当一个对象的状态发生改变时，所有依赖于它的对象都将得到通知。

全局的发布订阅对象

````javascript
    var Event = (function(){
        var clientList = {},
            listen,
            trigger,
            remove;

        listen = function(key, fn){
            if( !clientList[key] ){
                clientList[key] = [];
            }
            clientList[key].push(fn);
        };

        trigger = function(){
            var key = Array.prototype.shift.call( arguments );
                fns = clientList[key];
                if(!fns || fns.length === 0){
                    return false;
                }
                for(var i=0, fn; fn = fns[i++];){
                    fn.apply(this, arguments);
                }
        };

        remove = function(key, fn){
            var fns = clientList[key];
            if(!fns){
                return false;
            }
            if(!fn){
                fns && ( fns.length = 0 ); //没有传fn就remove所有的订阅消息
            }else{
                for(var l = fns.length-1; l>=0; l--){
                    var _fn = fns[l];
                    if(_fn === fn){
                        fns.splice(l, 1);
                    }
                }
            }
        };

        return {
            listen: listen,
            trigger: trigger,
            remove: remove
        }
    })();

    //调用
    Event.listen('squareMeter88',function(price){
        console.log('价格='+price);
    });

    Event.trigger('squareMeter88', 2000000);
````

可以先发布在订阅的发布订阅模式

````javascript
    var Event = (function(){
        var global = this,
            Event,
            _default = 'default';
        Event = function(){
            var _listen, _trigger, _remove,
                _slice = Array.prototype.slice,
                _shift = Array.prototype.shift,
                _unshift = Array.prototype.unshift,
                namespaceCache = {},
                _create,
                find,
                each = function( ary, fn ){
                    var ret;
                    for(var i=0, l=ary.length; i<l; i++){
                        var n = ary[i];
                        ret = fn.call(n, i, n);
                    }
                    return ret;
                };
                _listen = function( key, fn, cache ){
                    if( !cache[key] ){
                        cache[key] = [];
                    }
                    cache[key].push(fn);
                };
                _remove = function( key, cache, fn ){
                    if( cache[key] ){
                        if(fn){
                            for(var i=cache[key].length; i>=0; i--){
                                if(cache[key][i]===fn){
                                    cache[key].splice(i, 1);
                                }
                            }
                        }else{
                            cache[key] = [];
                        }
                    }
                };
                _trigger = function(){
                    var cache = _shift.call(arguments),
                        key = _shift.call(arguments),
                        args = arguments,
                        _self = this,
                        ret,
                        stack = cache[key];
                    if(!stack || !stack.length){
                        return;
                    }
                    return each(stack, function(){
                        return this.apply(_self, args);
                    });
                };
                _create = function(namespace){
                    var namespace = namespace || _default;
                    var cache = {},
                        offlineStack = [],
                        ret = {
                            listen: function(key, fn, last){
                                _listen(key, fn, cache);
                                if(offlineStack === null){
                                    return;
                                }
                                if(last === 'last'){
                                    offlineStack.length && offlineStack.pop()();
                                }else{
                                    each(offlineStack, function(){
                                        this();
                                    });
                                }
                                offlienStack = null;
                            },
                            one: function(key, fn, last){
                                _remove(key, cache);
                                this.listen(key, fn, last);
                            },
                            remove: function(key, fn){
                                _remvoe(key, cache, fn);
                            },
                            trigger: function(){
                                var fn,
                                    args,
                                    _self = this;
                                _unshift.call(arguments, cache);
                                args = arguments;
                                fn = function(){
                                    return _trigger.apply(_self, args);
                                };
                                if(offlineStack){
                                    return offlineStack.push(fn);
                                }
                                return fn();
                            }
                        };
                        return namespace ? ( namespaceCache[ namespace ] ? namespaceCache[ namespace ] : namespaceCache[ namespace ] = ret ) : ret;
                };

                return {
                    create: _create,
                    one: function(key, fn, last){
                        var event = this.create();
                            event.one(key, fn, last);
                    },
                    remove: function(key, fn){
                        var event = this.create();
                            event.remove( key, fn );
                    },
                    listen: function(key, fn, last){
                        var event = this.create();
                            event.listen(key, fn, last);
                    },
                    trigger: function(){
                        var event = this.create();
                        event.trigger.apply(this, arguments);
                    }
                };
        }();
        return Event;
    })();

    //先发布后订阅
    Event.trigger('click', 1);

    Event.listen('click', function(a){
        console.log(a)
    });

    //使用命名空间
    Event.create('namespace1').listen('click', function(a){
        console.log(a);
    });

    Event.create('namepace1').trigger('click', 1);

    Event.create('namespace2').listen('click', function(a){
        console.log(a);
    });

    Event.create('namepace2').trigger('click', 2);
````

## 命令模式

>命令模式是最简单和优雅的模式之一，命令模式中的命令指的是一个执行某些特定事情的指令。

菜单程序

````javascript

    var setCommand = function(button, command){
        button.onclick = function(){
            command.execute();
        }
    };

    var MenuBar = {
        refresh: function(){
            console.log('刷新菜单目录');
        }
    };
    var SubMenu = {
        add: function(){
            console.log('添加子菜单');
        },
        del: function(){
            console.log('删除子菜单');
        }
    };

    var RefreshMenuBarCommand = function(receiver){
        this.receiver = receiver;
    };

    RefreshMenuBarCommand.prototype.execute = function(){
        this.receiver.refresh();
    };

    var AddSubMenuCommand = function(receiver){
        this.receiver = receiver;
    };

    AddSubMenuCommand.prototype.execute = function(){
        this.receiver.add();
    };

    var DelSubMenuCommand = function(receiver){
        this.receiver = receiver;
    };

    DelSubMenuCommand.prototype.execute = function(){
        console.log('删除子菜单');
    };

    var refreshMenuBarCommand = new RefreshMenuBarCommand(MenuBar);
    var addSubMenuCommand = new AddSubMenuCommand(SubMenu);
    var delSubMenuCommand = new DelSubMenuCommand(SubMenu);

    setCommand(button1, refreshMenuBarCommand);
    setCommand(button2, addSubMenuCommand);
    setCommand(button3, delSubMenuCommand);

````

## 组合模式

>组合模式就是用小的子对象来构建更大的对象，而这些小的对象本身也许是由更小的“孙对象”构成的。

更强大的宏命令

````javascript
    var MacroCommand = function(){
        return {
            commandsList:[],
            add:function(command){
                this.commandsList.push( command );
            },
            execute: function(){
                for(var i=0, command; command = this.commandsList[ i++ ];){
                    command.execute();
                }
            }
        }
    };
    var openAcCommand = {
        execute: function(){
            console.log('打开空调');
        }
    };
    //组合对象
    var openTvCommand = {
        execute: function(){
            console.log('打开电视');
        }
    };

    var openSoundCommand = {
        execute: function(){
            console.log('打开音响');
        }
    };

    var macroCommand1 = MacroCommand();
    macroCommand1.add(openTvCommand);
    macroCommand1.add(openSoundCommand);

    //组合对象
    var closeDoorCommand = {
        execute: function(){
            console.log('关门');
        }
    };
    var closePcCommand = {
        execute: function(){
            console.log('打开电脑');
        }
    };
    var closeQQCommand = {
        execute: function(){
            console.log('登录');
        }
    };

    var macroCommand2 = MacroCommand();
    macroCommand2.add(closeDoorCommand);
    macroCommand2.add(closePcCommand);
    macroCommand2.add(closeQQCommand);

    //根组合对象
    var macroCommand = MacroCommand();
    macroCommand.add(openAcCommand);
    macroCommand.add(macroCommand1);
    macroCommand.add(macroCommand2);

    //组合对象是一个树形结构，它会被递归调用
    var setCommand = (function(command){
        command.execute();
    })(macroCommand);
````