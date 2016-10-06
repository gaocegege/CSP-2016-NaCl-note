可以好好看看[Technical Overview](https://developer.chrome.com/native-client/overview)

## 使用

[./usage.md](./usage.md)

## 分析

### 内外两层沙箱各自的作用

都是为了防御攻击，内层运用SFI和Memory Segment来进行保护，外层有点类似ptrace这样的技术。

内层沙箱使用代码静态分析技术来在不可信的x86代码中检查安全隐患。以前这样的分析受到了很多技术的挑战，比如说可自我修改的代码，重叠指令。为了解决这样的问题，在Native Client中为代码制定了一系列契约和结构规则，从而确保本地代码模块能可靠地进行分解，然后通过代码验证器来确保可执行文件只包含合法指令集中的指令。

这样做的好处是不需要信任编译器，只需要信任这个非常小的静态代码检查器。

内层沙箱还利用了x86内存分段机制来限制数据和指令的内存引用。

内层沙箱用于在一个本地进程中创建一个安全的子域。在这个子域中我们可以将一个可信的运行时服务(Service Runtime)子系统和不可信模块放置在同一个进程中。通过一个安全的跳跃/计分板机制来允许可信代码和不可信代码之间的控制转移。

内层沙箱不仅将系统与本地模块进行分离，而且还使得本地模块与操作系统进行了分离。

外层沙箱是第二道防御机制。它会对运行NaCl模块的进程的所有系统调用，通过与一个允许的系统调用白名单进行比对来拒绝或通过此调用。目前白名单上允许的系统调用有46个。

## PNaCl与NaCl的区别

* The left side of the diagram shows Portable Native Client (PNaCl, pronounced “pinnacle”). An LLVM based toolchain produces a single, portable (pexe) module. At runtime an ahead-of-time (AOT) translator, built into the browser, translates the pexe into native code for the relevant client architecture.
* The right side of the diagram shows (non-portable) Native Client. A GCC based toolchain produces multiple architecture-dependent (nexe) modules, which are packaged into an application. At runtime the browser determines which nexe to load based on the architecture of the client machine.

NaCl是完全跨平台的，一种是编译成各个平台的机器码，一种是利用LLVM类似的中间代码，在运行时去解释中间代码。前者是NaCl，后者被称作PNaCl，但做的事情是一样的，都是为了在浏览器里运行Native代码

![](https://developer.chrome.com/native-client/images/nacl-pnacl-component-diagram.png)

## Comments

这篇论文已经有些年头了，是09年发表的。从09年到现在2016年，技术和趋势都有了很大的发展。可能从当时看，Native Client这种在浏览器里安全地运行Native代码的技术有些前景，毕竟是谷歌发的论文。但是从现在来看，它并不是发展的趋势。目前Javascript已经可以说是统一整个前端了，现在的潮流是所有东西都用js来实现，随着硬件的发展，还有js开发者数量的不停增长下，甚至是Native app都已经逐渐由js来进行开发，比如electron，React Native这样的技术。这样的方式能够降低人力成本，以前需要招PC开发，iOS开发，现在只需要统一招世界第一语言的开发。

虽然Native Client研究本身很有趣，但Native Client的应用场景相对要小一些。只有那些对性能极其敏感的Web，才会考虑这样的技术。它本身的设想是把计算部分扔给Native Code去做，js可以只做展示。？Native Client主要的工作是保证安全性，和做好了Native Code跟js的交互。

但是很尴尬，场景太小了。现在来看大部分跟计算有关的工作都是由服务器去做的，到了浏览器这边本身就是一些偏展示性的工作。Native Client在页端游戏渲染，这样对前端计算负载比较大的任务里可能会有好的表现吧。我觉得还是场景太小，也可能是我人生阅历不够丰富。

不过，Native Client的思想发展一下，跟Chrome OS有点搭，本身操作系统不支持安装任何原生应用，所有应用都在Chrome里面，如果想写原生应用，只有通过Native Client这样的曲线救国的方式来实现。

在提高性能这块，除了Native Client，还有另一个流派，Asm.js

![](http://static.oschina.net/uploads/img/201312/30100811_P7Hv.png)

Asm.js 来自于 JavaScript 应用的一个新领域: 编译成JavaScript的C/C++应用。它是 JavaScript 应用的一个全新流派，由 Mozilla 的 Emscripten项目催生而来。Emscripten 将 C/C++ 代码传入  LLVM, 并将 LLVM 生成的字节码转换成 JavaScript (具体的, Asm.js, 是 JavaScript 的一个子集)。

```cpp
char xInt8 = 127;
char yInt8 = xInt8 + 1; // 128
char zInt8 = yInt8 / 2; // 63
```

```javascript
var xInt8 = 127;
var $add = (xInt8 + 1) | 0;
var yInt8 = ($add << 24) >> 24;
var $div = ((yInt8 | 0) / 2) & -1;
var zInt8 = ($div << 24) >> 24; 
```

![](http://i2.wp.com/kripken.github.com/mloc_emscripten_talk/macro4b.png)

firefox + asm.js 可以做到只比Native慢一倍

Principle：问题可以从不同层次来进行解决
