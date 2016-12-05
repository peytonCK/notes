# 建造者模式

分步骤创建复杂对象。”加工工艺“暴露，解耦组装过程和创建具体部件。

“分步骤”是一个稳定的算法，而复杂对象的各个部分则经常变化。

``` javascript
    function Fangzi(){
        this.desgin="Finish";
        this.bulid="Finish";
        this.decorate="Finish";
    }

    function Baogongtou(){
        this.gaifangzi=function(designer,bulider,decorater){
            designer.zuoshi();
            bulider.zuoshi();
            decorater.zuoshi();
            return new Fangzi();
        }
    }

    function Gongren(todo){
        this.zuoshi=function(){
            console.log("Finish "+todo);
        }
    }

    var gongren1=new Gongren('design'),
        gongren2=new Gongren('bulid'),
        gongren3=new Gongren('decorate');

    var baogongtou=new Baogongtou();

    var fangzi=baogongtou.gaifangzi(gongren1,gongren2,gongren3);

    console.log(fangzi);

```

建造者模式，简单来说，就是分步走，每一步都是独立封装的。

