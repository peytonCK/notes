# 工厂模式

## 定义：
提供一个接口来创建类似的对象。创建逻辑不对外暴露。让其子类自己决定实例化哪一个工厂类。

## 主要解决：
主要解决接口选择的问题。

## 何时使用：
当你只生成一些类似实例时，就用简单工厂模式就好；

> 举个例子：我有个自行车店，有山地车、公路车、城市车，可以对外销售。客户可以根据需求来提车。

``` javascript
    
    function Shandi(){
        this.speed=20;
        this.strength=20;
        this.run=function(){
            console.log(this.speed);
        };
    }

    function Gonglu(){
        this.speed=50;
        this.strength=15; 
    }

    function Chengshi(){
        this.speed=10;
        this.strength=10;
    }

    var gpdShop={
        getCircle:function(type){
            switch(type){
                case 'shandi':
                    return new Shandi();
                    break;
                case 'gonglu':
                    return new Gonglu();
                    break;
                case 'chengshi':
                    return new Chengshi();
                    break;

            }
        }
    };

    var circle1=gpdShop.getCircle('shandi');
    console.log(circle1);
    var circle2=gpdShop.getCircle('gonglu');
    console.log(circle2);
    var circle3=gpdShop.getCircle('chengshi');
    console.log(circle3);
```

当你要生成一些类似但有关联的实例时，就用工厂模式；

> 举个例子：我有个自行车店，车子都是从自己的自行车工厂里提出来的。客户把需求登记在店里，店里根据需求去自行车厂提车。

``` javascript

    var gpdShop={
        getCircle:function(type){
            return gpdFactory[type];
        }
    };

    var gpdFactory={
        'shandi':{
                    speed:20,
                    strength:30
                },
        'gonglu':{
                    speed:50,
                    strength:15
                },
        'chengshi':{
                    speed:10,
                    trength:10
                }
    }

    var circle1=gpdShop.getCircle('shandi');
    console.log(circle1);
    var circle2=gpdShop.getCircle('gonglu');
    console.log(circle2);
    var circle3=gpdShop.getCircle('chengshi');
    console.log(circle3);
```

当你要生成一些有关联的实例时，要用抽象工厂模式；

> 举个例子：我有个车店，卖电动车、自行车，车子分别是从自己的工厂两条产品线（电动车、自行车）提出来的。客户把需求 登记在店里，店里根据需求去工厂相应生产线提车。

``` javascript
    var gpdShop={
        getCircle:function(type){
            return gpdFactory['circles'][type];
        },
        getDiandong:function(type){
            return gpdFactory['diandong'][type];
        }
    };

    var gpdFactory={
        circles:{
          'shandi':{
                    speed:20,
                    strength:30
                },
        'gonglu':{
                    speed:50,
                    strength:15
                },
        'chengshi':{
                    speed:10,
                    trength:10
                }  
                },
        diandong:{
          'shandi':{
                    speed:40,
                    strength:60
                },
        'gonglu':{
                    speed:100,
                    strength:30
                },
        'chengshi':{
                    speed:20,
                    trength:20
                }  
                }
    }

    var circle1=gpdShop.getCircle('shandi');
    console.log(circle1);
    var circle2=gpdShop.getCircle('gonglu');
    console.log(circle2);
    var circle3=gpdShop.getCircle('chengshi');
    console.log(circle3);
    var circle4=gpdShop.getDiandong('shandi');
    console.log(circle4);
    var circle5=gpdShop.getDiandong('gonglu');
    console.log(circle5);
    var circle6=gpdShop.getDiandong('chengshi');
    console.log(circle6);

```



针对不同条件来创建对象就可以使用工厂模式。要深刻理解、实践。






