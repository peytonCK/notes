# hybird APP 开发

## 目标
    1. 在现有的APP中嵌入一些业务尝试页面；
    2. 可以热更新；
    3. 时间要快，技术要成熟；

## 四种方案
    1. ionic
    2. react native
    3. weex
    4. 原生APP直接调用H5页面

### ionic

较易入手；
github 28K+ star
社区较好，坑基本填平，需要学习angularJs


### react native

最稳定，性能好；
github 45K+ star
社区活跃，生态较好，学习路线较陡峭 react 全家桶
可单独开发APP
可内嵌已有APP

### weex

最不确定；
github 13K+ star
以目前调查来看，坑比较多，版本不稳定，已有APP案例较少,社区还不怎么活跃

### 原生APP直接调用H5页面

最简单，性能一般；
利用原生APP的webview直接渲染远程页面，交互利用提供的接口，也可引入jsbridges。

