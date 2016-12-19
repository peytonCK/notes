# 原型模式

创建对象时的方式，因为，要有一个明确的类对象，所以采用了原型继承。

``` javascript
    
    function inheritObj(o){
        var F=function(){};
        F.prototype=o;
        return new F();
    }

    function Books(){
        this.name="gpd";
        this.seller=['gpd1','gpd2','gpd3'];
        this.sell=function(){
            console.log(this.seller);
        };
    }

    var book=new inheritObj(Books);
    book.buyer="gpd4";
    book.buy=function(){
        console.log(this.seller+this.buyer);
    }
    book.buy();
```
