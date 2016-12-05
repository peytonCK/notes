# 单体模式
将代码组织成一个逻辑单元。
常用方式：命名空间
单体是一个用来划分命名空间并将一批相关方法和属性组织在一起的对象，如果它可以被实例化，那么它只能被实例化一次。

## 简单的创建单体 
使用对象字面量来创建简单的单体。
``` javascript 
    var singleton={
        name:"gpd",
        sayName:function(){
            console.log(this.name);
        }
    };
```

## 单体中习惯用 _ 来表示私有方法
``` javascript
    function singleton(){
        return {
        _sayName:function(){
            console.log(this.name);
        },
        name:"gpd1",
        sayName:function(){
            this.name+="2";
            this._sayName();
        }   
        } 
    }

    var singleton1=singleton();
    singleton1.sayName();//gpd12
```

## 创建带有私有变量的单体

``` javascript
    
    function singleton(){
        var name="gpd";
        function sayName(name){
            console.log(name);
        }

        return {
            name:name+"1",
            sayName:function(){  
                this.name+="2"; 
                sayName(this.name);
            }
        }
    }
    
    var singleton1=singleton();
    singleton1.sayName();//gpd12
```

## 闭包来创建单体
``` javascript
    var singleton=(function(){
        var name="gpd";
        function sayName(name){
            console.log(name);
        }

        return {
            name:name+"1",
            sayName:function(){  
                this.name+="2"; 
                sayName(this.name);
            }
        }
        })();
    singleton.sayName();//gpd12
```

## 惰性加载单体
``` javascript
    var singleton=(function(){
        var instance;
        function getInstance(){
            var name="gpd";
            function sayName(name){
                console.log(name);
            }

            return {
                name:name+"1",
                sayName:function(){  
                    this.name+="2"; 
                    sayName(this.name);
                }   
            };
        }
        
        return{
            init:function(){
                if(!instance){
                 instance=getInstance();
                }
                return instance;
            }
        };
        

        })();
    singleton.init().sayName();
```

## 构造函数实现单体

``` javascript
    function Singleton(){
            if(typeof Singleton.instance==='object'){
                return Singleton.instance;
            }
        this.name="gpd";
        this.sayName=function(){
            console.log(this.name);
        };
        
        Singleton.instance=this;
    }
    var singleton1=new Singleton();
    var singleton2=new Singleton();
    console.log(singleton1===singleton2);
```

``` javascript
    function Singleton(){
        var instance=this;
        this.name="gpd";
        this.sayName=function(){
            console.log(this.name);
        };

        Singleton=function(){
            return instance;
        }
    }
    var singleton1=new Singleton();
    var singleton2=new Singleton();
    console.log(singleton1===singleton2);
```
