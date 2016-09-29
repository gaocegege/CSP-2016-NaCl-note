可以好好看看[Technical Overview](https://developer.chrome.com/native-client/overview)

## 使用

[./usage.md](./usage.md)

## PNaCl与NaCl的区别

* The left side of the diagram shows Portable Native Client (PNaCl, pronounced “pinnacle”). An LLVM based toolchain produces a single, portable (pexe) module. At runtime an ahead-of-time (AOT) translator, built into the browser, translates the pexe into native code for the relevant client architecture.
* The right side of the diagram shows (non-portable) Native Client. A GCC based toolchain produces multiple architecture-dependent (nexe) modules, which are packaged into an application. At runtime the browser determines which nexe to load based on the architecture of the client machine.

NaCl是完全跨平台的，一种是编译成各个平台的机器码，一种是利用LLVM类似的中间代码，在运行时去解释中间代码。前者是NaCl，后者被称作PNaCl，但做的事情是一样的，都是为了在浏览器里运行Native代码

![](https://developer.chrome.com/native-client/images/nacl-pnacl-component-diagram.png)
