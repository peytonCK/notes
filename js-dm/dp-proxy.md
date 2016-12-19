# 代理模式
代理（proxy)是一个对象，用来控制对另一个对象（本体）的访问。它与本体实现同样的接口，会把任何方法调用传递给本体。

代理可以代替本体进行实例化，并使其可以远程访问。

代理可以把本体实例化推迟到真正需要的时候。

代理对象不会在本体基础上修改或添加其方法（那是装饰者干的事），也不会简化接口（那是门面模式干的事）。

# 简单的代理
``` javascript
    
    //图书管 查、借、还
    var library={
        selectBook:function(bookId){
            ...
        },
        checkoutBook:function(bookId){
            ...
        },
        returnBook:function(bookId){
            ...
        }
    };

    //图书管代理 查、借、还
    var libraryProxy={
        selectBook:function(bookId){
            library.selectBook(bookId);
        },
        checkoutBook:function(bookId){
            library.checkoutBook(bookId);
        },
        returnBook:function(bookId){
            library.returnBook(bookId);
        }
    }
```

> 这种代理方式没有什么用处


## 虚拟代理
用于控制对那种开销很大的本体的访问。
它会把本体的实例化推迟到有方法被调用的时候，有时还会提供关于实例化状态的反馈。
它可以在本体在加载之前扮演其替身的角色。

``` javascript
    function Library={
        this.catalog={};

    }
```

## 远程代理
控制对其他语言中的本体的访问。这种本体可能是一个Web服务资源，也可能是一个PHP对象。
结构型模式，提供了一个访问位于其他环境中的资源的原生Javascript API(native javascript API)。

## 保护代理
根据用户身份不一样，进行不一样处理。这个在js中无法应用。

> 代理优点：将远方资源本地化，在修改时，只需要修改代理就好； 将实例化推迟到要使用时，减少无关加载。

> 代理缺点：代理能做的，本体都可以做，代理会增加代码，增加工作量，需要权衡利弊。

