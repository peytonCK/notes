# 装饰者模式

透明地把对象包装在具有同样接口的另一对象之中。

装饰者模式是为已有功能动态地添加更多功能的一种方式，把每个要装饰的功能放在单独的函数里，然后用该函数包装所要装饰的已有函数对象，因此，当需要执行特殊行为的时候，调用代码就可以根据需要有选择地、按顺序地使用装饰功能来包装对象。优点是把类（函数）的核心职责和装饰功能区分开了。

``` javascript
    
    //普通自行车
    function Bicycle(){
        this.brush=function(){
            console.log("清洗服务");
        }
        this.getPrice=function(){
            return 300;
        }
    }

    //给自行车装个铃铛
    function BellBicycle(bicycle){
        Bicycle.call(this);
        this.getPrice=function(){
            return bicycle.getPrice()+50;
        }
    }

    //给自行车装个灯
    function LightBicycle(bicycle){
        Bicycle.call(this);
        this.getPrice=function(){
            return bicycle.getPrice()+100;
        }
    }

    var myBicycle=new Bicycle();
    myBicycle=new BellBicycle(myBicycle);
    myBicycle=new LightBicycle(myBicycle);
    myBicycle=new LightBicycle(myBicycle);
    console.log(myBicycle.brush());
    console.log(myBicycle.getPrice());
```
