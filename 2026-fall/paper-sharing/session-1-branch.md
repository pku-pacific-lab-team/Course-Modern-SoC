# 第 1 期：分支预测

> 时间：10 月 15 日（周四）· 人数：4 人（两方向各 2 人，一人一篇）
> 形式与要求见 [Paper Sharing 总页](index.md)

分支损害指令提取带宽和 ILP 并行度、中断顺序的控制流程——流水线越深、发射越宽，误预测的代价越大。这个领域几十年的主线，是在有限的存储与计算预算内逼近预测精度的极限：两级自适应、混合预测器、感知机、TAGE 都是这条主线上留下的路标。本期从这条主线里抽出两个方向，各配两篇有承接关系的论文：

**方向①**

- [TAGE（JILP'06）](https://jilp.org/vol8/v8paper1.pdf) → [TAGE-SC-L（CBP-5'16）](https://jilp.org/cbp2016/paper/AndreSeznecLimited.pdf)

把片上存储结构做到极致：几何级数历史长度加部分标记，同精度下存储开销只有前代 O-GEHL 的四分之一；此后十年主结构几乎不动，靠统计校正器等外挂继续收割剩余的误预测。香山昆明湖的方向预测器（16K TAGE-SC）就是这一系的工业落地。

**方向②**

- [感知机（HPCA'01）](https://www.cs.utexas.edu/~lin/papers/hpca01.pdf) → [BranchNet（MICRO'20）](https://microarch.org/micro53/papers/738300a118.pdf)

把机器学习引入预测器：感知机用权重学习历史上哪些分支相关，是 AI/ML 用于架构硬件的早期代表，如今已出现在 AMD Ryzen、Samsung Exynos 等商用处理器中；BranchNet 再进一步——离线训练 CNN、片上只做推理，专攻少数难预测的分支。

**选读**：[Yeh & Patt（MICRO'91）](https://hps.ece.utexas.edu/pub/yeh_micro24.pdf)——两级自适应的源头，两个方向共同的祖先。

**也欢迎自己找**：和"分支预测"相关即可，确定后提前发邮件告知助教（[zhu_20021122@stu.pku.edu.cn](mailto:zhu_20021122@stu.pku.edu.cn)）。
