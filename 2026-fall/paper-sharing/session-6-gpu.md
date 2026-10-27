# 第 6 期：GPU 架构

> 时间：11 月 19 日（周四）· 人数：2 人（两方向各 1 人）
> 形式与要求见 [Paper Sharing 总页](index.md)

GPU 的核心是一套执行模型——SIMT（单指令多线程）：硬件以 32 线程的 warp 为单位调度，每个线程持有独立的程序计数器与寄存器状态；warp 内出现控制分歧时，硬件串行执行各分支路径，并在所有路径结束后重聚。本期两个方向从两个时间点考察这套模型及其扩展：方向①看它如何被定义，以及当深度学习成为主要负载时专用单元如何被嵌入；方向②看一套开放实现如何以极小的 ISA 扩展复现 SIMT，以及把矩阵单元从 SIMT 核内移至核簇会带来什么变化。

**方向①**

- [Tesla（IEEE Micro'08）](https://doi.org/10.1109/MM.2008.31) → [Volta（IEEE Micro'18）](https://doi.org/10.1109/MM.2018.022071134)

执行模型的确立与扩展：Tesla 给出 SIMT 的完整定义——warp 为 32 线程、线程各有独立状态、分歧串行执行后重聚（以 branch synchronization stack 管理），每个 SM 以零开销调度管理 768 个并发线程；GeForce 8800 GTX 以 128 个流处理器、1.5 GHz、峰值 576 GFLOPS、150 W 实现这一模型[1]。十年后 Volta 在同一个 SIMT 核内加入 Tensor Core：GV100 共 640 个（每 SM 8 个），每个每周期执行一次 4×4 矩阵乘加（D=A×B+C），FP16 输入、FP32 累加，V100 峰值 120 TFLOPS；同时引入独立线程调度，使 SIMT 首次保证线程最终前进，从而支持需要互斥的算法[2]。该文对 Tensor Core 内部结构的披露有限（相关章节约占全文 8%），其微架构细节由后续逆向工作补足[3]。

**方向②**

- [Vortex（MICRO'21）](https://arxiv.org/abs/2110.10857) → [Virgo（ASPLOS'25）](https://arxiv.org/abs/2408.12073)

开源实现与内部架构：Vortex 是一个开源的 GPU 实现（[项目地址](http://vortex.cc.gatech.edu)），提供完整的硬件 RTL 与软件栈（编译器、驱动、OpenCL 运行时），支持通用计算与图形，可在 FPGA 上运行[4]。Virgo 是在 Vortex 之上开展的 GPU 微架构工作：把矩阵单元从 SIMT 核内移到核簇级，使操作数直接取自共享内存，从而支持更大的操作粒度、减少核内指令与寄存器访问，相比核内紧耦合的 Tensor Core 显著降低功耗与能耗[5]。本方向的主线是：以一个开源实现为参照讲清 GPU 内部架构，并把 Virgo 的矩阵单元与 Tensor Core 的做法放在一起对照。

**也欢迎自己找**：和"GPU 架构"相关即可，确定后提前发邮件告知助教（zhu_20021122[at]stu.pku.edu.cn）。

**参考文献**

[1] Lindholm, Nickolls, Oberman, Montrym. *NVIDIA Tesla: A Unified Graphics and Computing Architecture*. IEEE Micro, 2008.

[2] Choquette, Giroux, Foley. *Volta: Performance and Programmability*. IEEE Micro, 2018.

[3] Raihan, Goli, Aamodt. *Modeling Deep Learning Accelerator Enabled GPUs*. ISPASS 2019.

[4] Tine, Elsabbagh, Yalamarthy, Kim. *Vortex: Extending the RISC-V ISA for GPGPU and 3D-Graphics Research*. MICRO 2021.

[5] Kim, Yan, You, Yang, Shao. *Virgo: Cluster-level Matrix Unit Integration in GPUs for Scalability and Energy Efficiency*. ASPLOS 2025.

[6] Choquette, Gandhi. *NVIDIA Hopper H100 GPU: Scaling Performance*. IEEE Micro, 2023.
