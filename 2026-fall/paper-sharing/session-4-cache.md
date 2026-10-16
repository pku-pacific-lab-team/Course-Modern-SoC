# 第 4 期：Cache 与内存层次

> 时间：11 月 5 日（周四）· 人数：2 人（两方向各 1 人）
> 形式与要求见 [Paper Sharing 总页](index.md)

Cache 容量永远不够大——你们在 Lab 1 已经测过：工作集越过拐点，MPKI 就起飞。LRU 在这种时候会把整个工作集 thrash 掉，装入的行超过六成直到被淘汰都零复用。容量之外还能靠什么？本期两个方向给出两代回答：一个方向把人类的先验（"刚用的还会用、scan 的不会再用"）压缩进几个比特；另一个方向干脆不学启发式，直接学 1966 年 Belady 最优算法在历史访问上的行为。四篇论文串起来，就是 LRU 到最优解之间的差距被逐步吃掉的二十年。

**方向①**

- [DIP（ISCA'07，ISCA 2026 Influential Paper Award）](https://jaleels.org/ajaleel/publications/isca2007-dip.pdf) → [RRIP（ISCA'10）](https://jaleels.org/ajaleel/publications/isca2010-rrip.pdf)

把替换拆成"插入"和"淘汰"两个问题：DIP 让新行从 LRU 端插入、保住部分工作集防 thrash，再用 15 比特的 Set Dueling 在两种策略间动态选择，弥合了 LRU 与最优解之间三分之二的差距；RRIP 再给每行 2 比特预测"再引用间隔"，连 scan 也能防——因为只要 2 比特，它直接嵌进现有 NRU 硬件的形态，成为此后十几年真实处理器替换策略的蓝本。

**方向②**

- [Hawkeye（ISCA'16）](https://www.cs.utexas.edu/~lin/papers/isca16.pdf) → [Mockingjay（HPCA'22，Best Paper Finalist）](https://www.cs.utexas.edu/~lin/papers/hpca22.pdf)

不向启发式学，直接向最优解学：Hawkeye 用 8 倍于缓存的历史窗口重建 Belady 在过去会怎么换（99% 复现精度），让 PC 分类器学出"哪条指令装的行该留"，对全部 28 个负载无一退化；Mockingjay 再进一步——把"该不该留"的二分类换成"什么时候再用"的 ETA 排序，单核性能吃到 Belady 最优解的 95%（5.7% vs 6.0%），成为第一个 1MB 缓存打赢 2MB LRU 的替换策略。

**也欢迎自己找**：和"Cache 与内存层次"相关即可，确定后提前发邮件告知助教（[zhu_20021122@stu.pku.edu.cn](mailto:zhu_20021122@stu.pku.edu.cn)）。
