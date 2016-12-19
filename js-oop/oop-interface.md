# 接口
js中没有接口的概念，但可以模仿其它面向对象语言来作接口检测。

## 想一想怎么写

先想想JAVA中的面向对象的接口如何实现。
``` java
    
    //定义一个接口
   public interface interface1(){
      void getName();
      void getAge();
      void getSex();
   }

   //创建对象的构造函数
    
    public class Person implements interface1{
        ...
        public getName(){
            return this.name;
        }
        public getAge(){
            return this.age;
        }
        public getSex(){
            return this.sex;
        }
    }

```

接口只是用来定义对象应该具有哪些方法，但不去实现它。在构造函数中去实现。

其实经常使用的方法是什么？利用父类原型来做子类实例化时接口方法是否实现的检测。

```javascript
    
    //写个动物的类
    function Animal(){

    }

    Animal.prototype.say=function(){
        throw new Error("Say method must be implemented!");
    };

    Animal.prototype.run=function(){
        throw new Error("Say method must be implemented!");
    };

    function Dog(color){
        this.color=color;
    }

    Dog.prototype=new Animal();

    Dog.prototype.constructor=Dog;

    Dog.prototype.say=function(){
        console.log("wang,wang,wang!");
    };

    var dog1=new Dog("black");
    dog1.say();
    dog1.run();//抛出异常
```

但我们今天讨论的是不使用父类继承的方式，只利用类本身。

## 直接一点，利用原型上的定义方法。
``` javascript
    
    //利用原型，模仿写一个接口
    function Person(name,age,sex){
        this.name=name;
        this.age=age;
        this.sex=sex;
    }
    
    Person.prototype.getName=function(){
        return this.name;
    };

    Person.prototype.getAge=function(){
        return this.age;
    };

    Person.prototype.getSex=function(){
        return this.sex;
    };

```

> 优点：简单，快捷

> 缺点：如果是多个程序员进行协作，接口方法只有靠文档来传播，而且接口是否已经实现也未知。

## 再想想有没有说明对象的接口有哪些的方法？

很直接的方法是，在对象的一个属性直接将接口以数组形式赋给它。

``` javascript
    
    //利用原型，模仿写一个接口
    function Person(name,age,sex){
        this.name=name;
        this.age=age;
        this.sex=sex;
        this.interfaces=['getName','getAge','getSex'];//用来标识对象需要实现哪些接口
    }
    
    Person.prototype.getName=function(){
        return this.name;
    };

    Person.prototype.getAge=function(){
        return this.age;
    };

    Person.prototype.getSex=function(){
        return this.sex;
    };

```


## 接着上面想想，对象需要实现哪些接口是有了，但到底有没有实现这个需要检测一下吧？！

``` javascript
    
    //利用原型，模仿写一个接口
    function Person(name,age,sex){
        this.name=name;
        this.age=age;
        this.sex=sex;
        this.interfaces=['getName','getAge','getSex'];//用来标识对象需要实现哪些接口
    }
    
    Person.prototype.getName=function(){
        return this.name;
    };

    Person.prototype.getAge=function(){
        return this.age;
    };

    Person.prototype.getSex=function(){
        return this.sex;
    };

    //检测对象是不是实现了它需要实现的那些接口
    function ensureInterfaces(obj){
        //首先获取对象要实现的接口数组
        var interfaces=obj.interfaces?obj.interfaces:[],
            len=interfaces.length;
        
        if(!len){
            return true;
        }

        //开始验证
        for(var i=0;i<len;i++){
            if(!obj[interfaces[i]]){//一旦有一个接口没有实现，就立马返回false
                return false;
            }
        }

        return true;
    }
    
    var person=new Person('gpd','29','man');
    console.log(ensureInterfaces(person));

```


## 我们接着再想想，这样每次在创建完成后再检测有点不好，能不能在实例化的时候就检测呢？

``` javascript
    
    //利用原型，模仿写一个接口
    function Person(name,age,sex){
        this.name=name;
        this.age=age;
        this.sex=sex;
        this.interfaces=['getName','getAge','getSex'];//用来标识对象需要实现哪些接口
        console.log(ensureInterfaces(this));
    }
    
    Person.prototype.getName=function(){
        return this.name;
    };

    Person.prototype.getAge=function(){
        return this.age;
    };

    Person.prototype.getSex=function(){
        return this.sex;
    };

    //检测对象是不是实现了它需要实现的那些接口
    function ensureInterfaces(obj){
        //首先获取对象要实现的接口数组
        var interfaces=obj.interfaces?obj.interfaces:[],
            len=interfaces.length;
        
        if(!len){
            return true;
        }

        //开始验证
        for(var i=0;i<len;i++){
            if(!obj[interfaces[i]]){//一旦有一个接口没有实现，就立马返回false
                return false;
            }
        }

        return true;
    }
    
    var person=new Person('gpd','29','man');
    
```

> 其实就是这么简单，直接把检测函数扔到构造函数里，并在每次实例化时进行检测。

## 之上是我目前的理解，这样也可以满足要求。但我们来看看《javascript 设计模式》接口是如何实现的。

### 注释法
``` javascript

    /*
    这里说有两个接口，里面分别含有方法
    interface Composite{
        function add(child);
        function remove(child);
        function getChild(index);
    }

    interface FormItem{
        function save();
    }

    */

    //实现Composite FormItem 两个接口
    var CompositeForm=function(id,method,action){
        ...
    }

    //开始实现 Composite 接口里的方法
    CompositeForm.prototype.add=function(child){}
    CompositeForm.prototype.remove=function(child){}
    CompositeForm.prototype.getChild=function(index){}

    //开始实现 FormItem 接口里的方法
    CompositeForm.prototype.save=function(){}

```

> 简单但不实用，没进行检查抛错

### 属性检查

``` javascript

    /*
    这里说有两个接口，里面分别含有方法
    interface Composite{
        function add(child);
        function remove(child);
        function getChild(index);
    }

    interface FormItem{
        function save();
    }

    */

    //实现Composite FormItem 两个接口
    var CompositeForm=function(id,method,action){
        this.implementsInterfaces=['Composite','FormItem'];
        ...
    }
    ...
    //添加一个表单，在添加之前，检测实例
    function addForm(formInstance){
        if(!implements(formInstance,'Composite','FormItem')){
            throw new Error("Object does not implement a required interface.");
        }
        ...
    }

    //用来检测一个对象是否实现了它声明的接口
    function implements(object){
        for(var i=1; i<arguments.length; i++){
            var interfaceName=arguments[i];
            var interfaceFound=false;

            for(var j=0; j<object.implementsInterfaces.length; j++){
                if(object.implementsInterfaces[j]==interfaceName){
                    interfaceFound=true;
                    break;
                }
            }

            if(!interfaceFound){
                return false;
            }
        }
        return true;
    }

```

> 这种方式就是在创建完实例后检测有没有实现必要的接口，没有就抛出异常
> 缺点很明显：只能检测，接口列表里的接口是否存在对象的implementsInterfaces里，并不能保证它们已经被实现。


### 鸭式辨型法
不用说自己是鸭子，只要像鸭子一样走路并且嘎嘎叫的就是鸭子。

``` javascript
    
    var Composite=new Interface('Composite',['add','remove','getChild']);
    var FormItem=new Interface('FormItem',['save']);

    var CompositeForm=function(id,method,action){
        ...
    };

    ...

    function addForm(formInstance){
        ensureImplements(formInstance,Composite,FormItem);
        ...
    }

```

> 优点：实用，检测了对象接口是否全部实现

> 缺点：没有声明，对象要实现哪些接口，并且需要辅助类 Interface、辅助函数 ensureImplements，最后还没检查参数的名称、数目、类型。


### 书上采用的实现方法

``` javascript
    
        //定义了一个接口类，name为接口名称，methods为接口要实现的方法数组
    var Interface=function(name,methods){
        if(arguments.length!=2){
            throw new Error("Interface constructor called with "+ arguments.length+"arguments,but expected exactly 2.");
        }
        this.name=name;
        this.methods=[];
        for(var i=0,len=methods.length;i<len; i++){
            if(typeof methods[i] !== 'string'){
                throw new Error("Interface constructor expects method names to be "+"passed in as a string.");
            }
            this.methods.push(methods[i]);
        }
    }
    
    //静态类方法
    Interface.ensureImplements=function(object){
        if(arguments.length<2){//先判断参数是否只有一个，只有一个对象就抛出异常
            throw new Error("Function Interface.ensureImplements called with "+arguments.length + "arguments,but expected at least 2.");
        }

        //如果参数有两个以上，开始逐个验证接口
        for(var i=1,len=arguments.length; i<len; i++){
            var interface=arguments[i];
            if(interface.constructor !== Interface){//判断某个接口的类型是不是 Interface 不是就抛出异常
                throw new Error("Function Interface.ensureImplements expects arguments"+"two an above to be instances of Interface.");
            }
            
            //如果是接口，开始对里面的方法是否实现开始验证
            for(var j=0,methodsLen=interface.methods.length; j<methodsLen; j++){
                var method=interface.methods[j];

                //如果此方法不存在或此方法不是function类型，就抛出异常
                if(!object[method] || typeof object[method] !== 'function'){//
                    throw new Error("Function Interface.ensureImplements:object does not implement the "+interface.name+" interface.Method "+method+" was not found.");
                }
            }
        }
    };

    //定义了两个接口，每个接口里面有声明要实现的方法名
    var Composite=new Interface('Composite',['add','remove','getChild']);
    var FormItem=new Interface('FormItem',['save']);

    //定义一个Form类，实现上面定义的两个接口
    var CompositeForm=function(id,method,action){
        this.id=id;
        this.method=method;
        this.action=action;
    };
    CompositeForm.prototype.add=function(child){
        console.log("add "+child);
    };
    CompositeForm.prototype.remove=function(child){
        console.log("remove "+child);
    };
    CompositeForm.prototype.getChild=function(index){
        console.log("get "+index+" child");
    };
    /*
    CompositeForm.prototype.save=function(){
        console.log("save finish;");
    };*/

    function addForm(formInstance){
        Interface.ensureImplements(formInstance,Composite,FormItem);
        console.log(formInstance);
        console.log("add form "+ formInstance.toString() +"successfully!");
    }

    var form1=new CompositeForm(1,"post","/login");
    addForm(form1);//如果注释掉原型里的任何一个方法，都会抛出异常

```

> 最后要说的是，接口是面向对象方式编程重要的点。但也要结合自身项目需要，来考虑是否要用接口方式。当然，如果你要研究javascript的设计模式，你更要深刻认识接口。




