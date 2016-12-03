# 继承

就是两个对象，子对象继承父对象的属性和方法，并会拥有自己的属性和方法，如果和父对象同名，则会使用自己的属性和方法。

既然是对象，对象也分种类，一种是构造函数对象，一种是非构造函数对象（普通function,array,object）

# 拷贝继承

## 浅拷贝
只复制基础类型，引用类型共用

``` javascript
    function extend(obj1,obj2){
        var obj1=obj1||{};
        for(var p in obj2){
            obj1[p]=obj2[p];
        }
        return obj1;
    }
```


## 深拷贝
复制所有类型，单独一份（方法还是共用的）

``` javascript
    function deepExtend(obj1,obj2){
        var obj1=obj1||{};
        for(var p in obj2){
            if(typeof obj2[p] === 'object'){
                obj1[p]=(obj2[p].constructor === Array)?[]:{};
                deepExtend(obj1[p],obj2[p]);
            }else{
                obj1[p]=obj2[p];
            }
        }
        return obj1;
    }

    var obj1={};

    var obj2={
            time:"2016",
            books:['gpd1','gpd2','gpd3'],
            alert:function(){
                alert('123');
            }
        };

    obj1=deepExtend(obj1,obj2);
    console.log(obj1.time===obj2.time);//true 基础类型比对
    console.log(obj1.books===obj2.books);//false 引用类型比对 地址不一样
    console.log(obj1.alert===obj2.alert);//true 方法没有进行重新定义，所以还是一个

    var obj3=function(){
        this.time="2016";
        this.books=['gpd1','gpd2','gpd3'];
        this.alert=function(){
                alert('123');
            };
    };

    var obj4=deepExtend({},obj3);
    console.log(obj4);//Object {} function对象，所以无法进行拷贝，拷贝后也是{}；

    var obj5=['gpd1','gpd2','gpd3'];//数组对象进行拷贝得到类数组对象

    var obj6=deepExtend({},obj5);
    console.log(obj6);//Object {0: "gpd1", 1: "gpd2", 2: "gpd3"}
```

# 类式（原型链）继承

子类对象的原型是父类对象的实例；要区分原型对象和原型属性（只有函数有）

原型，不是函数对象才有原型对象，但函数对象会有一个prototype属性。
函数的原型属性(prototype property )是一个对象，当这个函数被用作构造函数来创建实例时，该函数的原型属性将被作为原型赋值给所有对象实例(注:即所有实例的原型引用的是函数的原型属性)
参考：[理解Javascript原型](http://blog.jobbole.com/9648/)
``` javascript
        var obj1={
            name:"gpd"
        };
        console.log(obj1.prototype);//undefined
        console.log(Object.getPrototypeOf(obj1));//Object {}
        console.log(obj1.__proto__);//Object {}

        var obj2=["gpd1","gpd2","gpd3"];
        console.log(obj2.prototype);//undefined
        console.log(Object.getPrototypeOf(obj2));//Array []
        console.log(obj2.__proto__);//Array []

        var obj3=function(){
            alert('123');
        };
        console.log(obj3.prototype);//Object {}
        console.log(Object.getPrototypeOf(obj3));//function() {}
        console.log(obj3.__proto__);//function() {}
```

``` javascript

    function SuperObject(){
        ...
    }

    function SubObject(){
        ...
    }

    SubObject.prototype=new SuperObject();

    var obj1=new SubObject();

```

> 优点：
> * 新创建对象可以共用SuperObject里的属性和方法；

> 缺点：
> * 如果SuperObject里的属性是引用类型，所有新创建对象都可以进行修改，并影响其它对象的使用；


# 构造函数继承

父类对象使用构造函数方式，子类对象实现中，利用call，改变父类对象的作用域，达到代码共用

``` javascript
    function SuperObject(id){
        this.books=['gpd1','gpd2','gpd3'];
        this.id=id;
    }
    
    SuperObject.prototype.showBooks=function(){
        console.log(this.books);
    }

    function SubObject(id){
        SuperObject.call(this,id);
    }

    var obj1=new SubObject(1);

    obj1.showBooks()；//TypeError
```

> 优点：
> * 代码减少，引用类型都是新创建对象自己的

> 缺点：
> * 如果父类有方法的话，没有共用，而是每个新创建对象都自己重新定义
> * 如果父类原型定义方法的话，子类实例对象也用不了

# 组合继承

将类式继承和构造函数继承相结合，不共用引用类型，但共用方法

``` javascript

    function SuperObject(id){
        this.books=['gpd1','gpd2','gpd3'];
        this.id=id;
    }

    SuperObject.prototype.showBooks=function(){
        console.log(this.books);
    }

    function SubObject(id,time){
        SuperObject.call(this,id);
        this.time=time;
    }

    SubObject.prototype=new SuperObject();

    SubObject.prototype.showTime=function(){
        console.log(this.time);
    };

    var obj1=new SubObject(1,12);
    obj1.showBooks();
    obj1.showTime();
```

> 优点：
> * 不共用引用类型，但共用方法

> 缺点：
> * 执行了两遍父类对象代码 SuperObject.call(),new SuperObject()


# 原型式继承

借助原型prototype属性可以根据已有的对象创建一个新的对象，同时不必创建新的自定义对象类型。

``` javascript
    
    function inheritObject(o){
        var F=function(){};
        F.prototype=o;
        return new F();
    }

    var book={
        name:"gpd",
        price:120
    }
    
    var book1=inheritObject(book);
    console.log(book1.name);

```

# 寄生式继承

借助原型式继承，并在此基础上扩展自我的属性

``` javascript

    function inheritObject(o){
        var F=function(){};
        F.prototype=o;
        return new F();
    }

    var book={
        name:"gpd",
        price:120
    }

    var book1=function(time){
        var obj=new inheritObject(book);//有没有new都可以
        obj.time=time;
        return obj;
    }

    var bookObj=book1("2016");
    console.log(bookObj.name+":"+bookObj.time);
```

# 寄生式组合继承
将寄生式和构造函数式两种模式相结合

``` javascript

    function inheritObject(o){
        var F=function(){};
        F.prototype=o;
        return new F();
    }

    function inheritPrototype(subObj,superObj){
        var p=new inheritObject(superObj.prototype);
        p.construcor=subObj;
        subObj.prototype=p;
    }

    function superObj(){
        this.books=[1,2,3];
        this.name="gpd";
    }

    superObj.prototype.showBooks=function){
        console.log(this.books);
    };

    function subObj(time){
        superObj.call(this,time);
        this.time=time;
    }

    inheritPrototype(subObj,superObj);

    var bookObj=new subObj("2016");

    bookObj.showBooks();
    console.log(bookObj.time);
```


> 优点：
> * 利用寄生式来改变子类对象的原型链




















