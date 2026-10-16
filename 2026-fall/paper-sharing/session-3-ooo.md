# 第 3 期：乱序执行

> 时间：10 月 29 日（周四）· 人数：2 人（两方向各 1 人）
> 形式与要求见 [Paper Sharing 总页](index.md)

乱序执行让指令不必按程序顺序等待：操作数就绪、功能单元空闲，即可先行执行。它是单核时代挖掘指令级并行（ILP）的核心机制，而该机制的核心是"窗口"——同时在飞的指令数量。本期两个方向各讲窗口的一半故事：方向①讲机制的发明过程（指令如何乱序执行、又如何在乱序中维持顺序的架构状态），方向②讲窗口的规模上限（一堵是电路延迟的墙，一堵是访存延迟的墙）。

**方向①**

- [Tomasulo（IBM J.'67）](https://doi.org/10.1147/rd.111.0025) → [Smith & Pleszkun 精确中断（ISCA'85，读 IEEE TC'88 扩展版）](https://doi.org/10.1109/12.4607)

机制的诞生：Tomasulo 用标签（tag）与公共数据总线（CDB）使指令乱序执行——每条在飞指令获得唯一身份，假依赖（WAR/WAW）被寄存器重命名消除[1]；但乱序完成破坏了顺序的架构状态，精确中断由此成为必须解决的问题——Smith & Pleszkun 系统给出五种方案，其中带旁路的重排序缓冲（ROB）方案仅损失 3% 性能，而强制按序完成要损失 16–23%[2]。

**方向②**

- [Palacharla 复杂度有效超标量（ISCA'97）](https://doi.org/10.1145/264107.264201) → [Kilo-instruction（IEEE Micro'05）](https://doi.org/10.1109/MM.2005.53)

窗口的两堵墙：Palacharla 用电路级分析证明，窗口越大、发射越宽，唤醒/选择与旁路逻辑的延迟随之急剧增长——窗口扩大与时钟频率相互制约[3]；Kilo-instruction 则论证另一堵墙：当主存延迟增至 1000 周期时，128 项窗口的性能损失达 45–65%，而 4096 项窗口仅损失 15–40%——窗口值得做大，但需要专门的解耦设计来支撑[4]。

**也欢迎自己找**：和"乱序执行"相关即可，确定后提前发邮件告知助教（[zhu_20021122@stu.pku.edu.cn](mailto:zhu_20021122@stu.pku.edu.cn)）。

**参考文献**

[1] Tomasulo. *An Efficient Algorithm for Exploiting Multiple Arithmetic Units*. IBM Journal of Research and Development, 1967.

[2] Smith, Pleszkun. *Implementing Precise Interrupts in Pipelined Processors*. IEEE Transactions on Computers, 1988（ISCA'85 的期刊扩展版）.

[3] Palacharla, Jouppi, Smith. *Complexity-Effective Superscalar Processors*. ISCA 1997.

[4] Cristal et al. *Kilo-Instruction Processors: Overcoming the Memory Wall*. IEEE Micro, 2005.
