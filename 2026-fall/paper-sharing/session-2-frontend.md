# 第 2 期：取指与前端

> 时间：10 月 22 日（周四）· 人数：2 人（两方向各 1 人）
> 形式与要求见 [Paper Sharing 总页](index.md)

宽发射处理器每个周期需要供给多条指令，但整数程序的基本块平均只有 4–6 条指令，taken 分支还会进一步切断取指的连续性——指令供给与分支预测一样，长期决定流水线能否被充分利用。本期两个方向横跨二十年：方向①回答"怎么取得快"（取指带宽），方向②回答"怎么取得早"（解耦前端与预取）；方向②的前一篇正是方向① FDIR 路线的直系延续，后一篇则是这条路线在今天的工业落地。

**方向①**

- [Trace Cache（IEEE TC'99，MICRO'96 的期刊扩展版）](https://ericrotenberg.wordpress.ncsu.edu/files/2025/06/paper_IEEE-TC-Feb-1999.pdf) → [FDIR 取指预取（MICRO'99）](https://cseweb.ucsd.edu/~calder/papers/MICRO-99-FDP.pdf)

九十年代的两种回答：Trace Cache 把动态指令序列整个缓存下来，使不连续的代码在取指时表现为连续，相比同级的多块连续取指机制提速 15–35%[1]；FDIR 则让分支预测器与取指解耦、运行在当前取指之前，用预测出的地址提前预取，配合缓存探测过滤时平均提速 26%–40%，代价是总线带宽占用明显上升[2]。

**方向②**

- [Boomerang 控制流供给（HPCA'17）](https://doi.org/10.1109/HPCA.2017.53) → [香山南湖处理器（MICRO'22，读前端章节）](https://doi.org/10.1109/MICRO56248.2022.00080)

二十年后，问题变成服务器负载 MB 级的指令足迹：Boomerang 证明 FDIR 的思路依然有效——不需要数百 KB 的专用预取元数据，仅用核内已有的 BTB 与分支预测器即可同时预取 I-cache 块与 BTB 项，以 540 字节的额外状态达到与当时最优方案（需 240KB）相当的性能[3]；香山南湖则把"预测器先于取指"的解耦前端实现在真实芯片中，其方向预测器正是第 1 期 TAGE-SC 一系[4]。

**也欢迎自己找**：和"取指与前端"相关即可，确定后提前发邮件告知助教（zhu_20021122[at]stu.pku.edu.cn）。

**参考文献**

[1] Rotenberg, Bennett, Smith. *A Trace Cache Microarchitecture and Evaluation*. IEEE Transactions on Computers, 1999（MICRO'96 的期刊扩展版）.

[2] Reinman, Calder, Austin. *Fetch Directed Instruction Prefetching*. MICRO 1999.

[3] Kumar, Huang, Grot, Nagarajan. *Boomerang: A Metadata-Free Architecture for Control Flow Delivery*. HPCA 2017.

[4] Xu et al. *Towards Developing High Performance RISC-V Processors using Agile Methodology*（香山南湖）. MICRO 2022（读前端/FTB 章节）.
