# 封装
封装，就是将一个东西抽象成对象，将其属性和行为都封装进去。说白了就是创建对象。多种方法，是为了更容易扩展、更容易修改的模式。

# 最简单封装
``` javascript
   var book={
     name:"gpd",
     sayName:function(){
        console.log(this.name)
     }
   };
   book.sayName();//gpd
```

> 优点：简单快捷

> 缺点：如果创建类似的对象，需要多次这么写


# 工厂模式封装
``` javascript
   function book(name){
        var o={};
        o.name=name;
        o.sayName=function(){
            console.log(this.name);
        };
        return o;
   }

   var book1=book("gpd1");
   var book2=book("gpd2");
   book1.sayName();//gpd1
   book2.sayName();//gpd2
```

> 优点：解决了快速创建相似对象

> 缺点：无法进行对象识别


# 构造函数封装
``` javascript
    function Book(name){
        this.name=name;
        this.sayName=function(){
            console.log(this.name);
        };
    }
    var book1=new Book("gpd1");
    var book2=new Book("gpd2");
    book1.sayName();//gpd1
    book2.sayName();//gpd2
```
> 优点：可以进行对象识别 constructor/instanceof

> 缺点：构造函数中的方法无法共用，每次都重新定义了


# 原型属性封装
``` javascript
    function Book(name){
    }
    Book.prototype.name="gpd";
    Book.prototype.sayName=function(){
        console.log(this.name);
    }
    var book1=new Book("gpd1");
    var book2=new Book("gpd2");
    book1.sayName();//gpd
    book2.sayName();//gpd
```
> 优点：可以进行对象识别 constructor/instanceof/isPrototypeOf

> 缺点：原型对象中的属性和方法都是共用的，想要的是属性应该是对象自己的

# 构造函数和原型的组合封装
``` javascript
    function Book(name){
        this.name=name
    }
    Book.prototype.name="gpd";
    Book.prototype.sayName=function(){
        console.log(this.name);
    }
    var book1=new Book("gpd1");
    var book2=new Book("gpd2");
    book1.sayName();//gpd1
    book2.sayName();//gpd2
```
> 优点：最经常用的封装方式，可以进行对象识别 constructor/instanceof/isPrototypeOf

# 动态构造函数封装
``` javascript
    function Book(name){
        this.name=name
        if(typeof this.sayName!="function"){
            Book.prototype.sayName=function(){
                console.log(this.name);
            }
        }
    }
    var book1=new Book("gpd1");
    var book2=new Book("gpd2");
    book1.sayName();//gpd1
    book2.sayName();//gpd2
```
> 优点：和组合模式差不多，更好的地方是不用提前设计原型对象，在第一次生成对象的时候才去执行。

# 寄生式构造函数封装
``` javascript
    function book(name){
        var o=new Object();
        o.name=name;
        o.sayName=function(){
            console.log(this.name);
        };
        return o;
    }
    var book1=new book("gpd1");
    var book2=new book("gpd2");
    book1.sayName();//gpd1
    book2.sayName();//gpd2

    function specialArray(){
        var values=new Array();
        values.push.apply(values,arguments);
        values.toPipeString=function(){
            return this.join("|");
        }
        return values;
    }
    var colors=new specialArray("red","blue","green");
    console.log(colors.toPipeString());
```
> 特殊运用  无法确定创建出的对象和构造函数之间的联系

# 稳妥构造函数封装
``` javascript
    function book(name){
        var o={};
        o.sayName=function(){
            console.log(name);
        };
        return o;
    }
    var book1=book("gpd1");
    var book2=book("gpd2");
    console.log(book1.name);//undefined 
    book1.sayName();//gpd1
    book2.sayName();//gpd2
```
> 优点：安全性高