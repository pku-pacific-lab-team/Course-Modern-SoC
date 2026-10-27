# 第 4 期：Cache 与内存层次

> 时间：11 月 5 日（周四）· 人数：2 人（两方向各 1 人）
> 形式与要求见 [Paper Sharing 总页](index.md)

Cache 容量永远不够大——在 Lab 1 我们已经测过：当处理器的工作集越过拐点，MPKI 就显著上升。当工作集规模超过可用容量时，LRU 会发生 thrashing：装入的行在获得任何命中之前即被逐出，超过六成的行从装入到淘汰从未被复用[1]。容量之外还能靠什么？本期两个方向给出两代回答：方向①把对访问模式的认识写成显式规则——主动压低"等不到复用的行"的驻留优先级，用极少的硬件状态同时防住 thrashing 与突发的不重复访问流（scan）[1][2]；方向②不预设规则，而是先在历史访问序列上重建 Belady 最优算法的每一个决策，再让片上预测器学习这些决策的规律，用学到的规律指导未来的替换[3][4]。四篇论文串起来，是 LRU 与最优解之间的差距被逐步缩小的二十年。

**方向①**

- [DIP（ISCA'07）](https://jaleels.org/ajaleel/publications/isca2007-dip.pdf) → [RRIP（ISCA'10）](https://jaleels.org/ajaleel/publications/isca2010-rrip.pdf)

DIP 将替换策略分解为插入与淘汰两个问题：新行插入 LRU 端，使部分工作集得以驻留以防止 thrashing；再以仅 15 比特开销的 Set Dueling 机制在两种插入策略间动态选择，弥合了 LRU 与最优替换之间三分之二的 miss 差距[1]。RRIP 进一步为每行附加 2 比特的再引用间隔预测，从而同时具备 scan 抵抗能力——由于每行仅需 2 比特且在 M=1 时退化为 NRU，该结构可直接嵌入现有硬件实现，成为其后十余年商用处理器替换策略的蓝本[2]。

**方向②**

- [Hawkeye（ISCA'16）](https://www.cs.utexas.edu/~lin/papers/isca16.pdf) → [Mockingjay（HPCA'22）](https://www.cs.utexas.edu/~lin/papers/hpca22.pdf)

另一条路线不依赖启发式，而是直接学习最优解的行为：Hawkeye 以 8 倍于缓存容量的历史窗口重建 Belady 算法在过去访问序列上的决策（复现精度 99%），并用基于 PC 的分类器学习"哪条指令装入的行应当驻留"，在全部 28 个评测负载上均未劣于 LRU[3]；Mockingjay 进一步将"是否驻留"的二分类问题改为"何时再被引用"的 ETA 排序问题，单核性能达到 Belady 最优解的 95%（5.7% vs 6.0%），并成为第一个以 1MB 容量超过 2MB LRU 缓存的替换策略[4]。

**也欢迎自己找**：和"Cache 与内存层次"相关即可，确定后提前发邮件告知助教（zhu_20021122[at]stu.pku.edu.cn）。

**参考文献**

[1] Qureshi, Jaleel, Patt, Steely, Emer. *Adaptive Insertion Policies for High Performance Caching*. ISCA 2007（ISCA 2026 Influential Paper Award）.

[2] Jaleel, Theobald, Steely, Emer. *High Performance Cache Replacement Using Re-reference Interval Prediction (RRIP)*. ISCA 2010.

[3] Jain, Lin. *Back to the Future: Leveraging Belady's Algorithm for Improved Cache Replacement*（Hawkeye）. ISCA 2016.

[4] Shah, Jain, Lin. *Effective Mimicry of Belady's MIN Policy*（Mockingjay）. HPCA 2022（Best Paper Finalist；IEEE Micro Top Picks 2023）.
