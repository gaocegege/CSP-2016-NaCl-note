# 论文笔记

与NaCl有关的问题：

* [https://www.zhihu.com/question/19811741](https://www.zhihu.com/question/19811741)
* [https://www.zhihu.com/question/19811586](https://www.zhihu.com/question/19811586)
* [https://www.zhihu.com/question/26529678](https://www.zhihu.com/question/26529678)
* [https://www.zhihu.com/question/23067497/answer/40878704](https://www.zhihu.com/question/23067497/answer/40878704)
* [NaCl Module开发指南](https://developer.chrome.com/native-client)
* [开发NaCl的文档](https://www.chromium.org/nativeclient)

## 谷歌发表的与NaCl有关的论文

[http://www.chromium.org/nativeclient/reference/research-papers](http://www.chromium.org/nativeclient/reference/research-papers)

## 解释图

![](https://developer.chrome.com/native-client/images/web-app-with-nacl.png)

## gaocegege的笔记

[gaocegege-note](./gaocegege.md)

## Comments

这篇论文已经有些年头了，是09年发表的。从09年到现在2016年，技术和趋势都有了很大的发展。可能从当时看，Native Client这种在浏览器里安全地运行Native代码的技术有些前景，毕竟是谷歌发的论文。但是从现在来看，它并不是发展的趋势。目前Javascript已经可以说是统一整个前端了，现在的潮流是所有东西都用js来实现，随着硬件的发展，还有js开发者数量的不停增长下，甚至是Native app都已经逐渐由js来进行开发，比如electron，React Native这样的技术。这样的方式能够降低人力成本，以前需要招PC开发，iOS开发，现在只需要统一招世界第一语言的开发。

虽然Native Client研究本身很有趣，但Native Client的应用场景相对要小一些。只有那些对性能极其敏感的Web，才会考虑这样的技术。它本身的设想是把计算部分扔给Native Code去做，js可以只做展示。？Native Client主要的工作是保证安全性，和做好了Native Code跟js的交互。

但是很尴尬，场景太小了。现在来看大部分跟计算有关的工作都是由服务器去做的，到了浏览器这边本身就是一些偏展示性的工作。Native Client在页端游戏渲染，这样对前端计算负载比较大的任务里可能会有好的表现吧。我觉得还是场景太小，也可能是我人生阅历不够丰富。

不过，Native Client的思想发展一下，跟Chrome OS有点搭，本身操作系统不支持安装任何原生应用，所有应用都在Chrome里面，如果想写原生应用，只有通过Native Client这样的曲线救国的方式来实现。
