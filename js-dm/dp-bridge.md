# 桥接模式

将抽象部分和实现部分进行分离，使它们可以独立地变化。

## 常用在事件监控上


``` javascript
    //平常代码
    addEvent("#id1","click",getBeerById);
    function getBeerById(e){
        var id=this.id;
        asyncRequest('GET', 'beer.uri?id=' + id, function(resp) {
        // Callback response.
        console.log('Requested Beer: ' + resp.responseText);
    }

    //加入桥接后代码
    addEvent("#id1","click",getBeerByBridge);
    function getBeerByBridge(e){
        var id=this.id;

        getBeerById(id,function(beer){
            console.log('Requested Beer: ' + beer);
        });
    }

    function getBeerById(id,callback){
        asyncRequest('GET', 'beer.uri?id=' + id, function(resp) {
        // Callback response.
        callback(resp.responseText);
    }
```