##随记
* `初衷`想要能够通过native code获取更高的computational performance，同时利用起已有的大量非js开发的程序。
* `传统误解`认为native code没有一个技术性手段去做合法性检测，所以side effect会probably是危险的。
* 每个NaCl component有自己独立的地址空间，通过NPAPI或srpc传递指令（基于IMC）。
* IMC通道是双向的（trusted <-> untrusted），所以其中的指令都需要validation。
* ###Inner Sandbox
    * TCB + modified compiler chain + validator，强调`light-weight`
    * 基于CISC的fault isolation
    * 基于`reliable disassembly`，从而可以限定whitelist，从而保证控制流合法
    * 基于`segmented memory`保证所有memory reference有效，从而保证数据流合法
    * 通过一个`Service Runtime`，用trampoline和springboard传递指令
    * 认为OS本身可能有缺陷，所以同时防护native module & OS （双向validation）
    * 4个需要解决的子问题：
        * `dataintegrity`：segmented memory
        * `reliable disassembly`：C1 + C6
        * `no unsafe instructions`：TCB
        * `CFI`: direct branch -> 静态分析；indirect branch -> _nacljmp_
* ###Outer Sandbox
    * 基于[ptrace](http://blog.csdn.net/edonlii/article/details/8717029)实现对系统调用的跟踪，从而限制side effect，会产生一定的overhead，但是可以接受。
    * `limitations`在于存在一些alternative sys call（lcall7 and lcall27）
* 实际上我感觉whitelist非常依赖经验，而依赖经验就意味着可能__boom boom boom__。
