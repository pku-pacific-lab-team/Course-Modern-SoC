# 第 5 期：内存一致性

> 时间：11 月 12 日（周四）· 人数：2 人（两方向各 1 人）
> 形式与要求见 [Paper Sharing 总页](index.md)

多处理器中"最后一个写入的值"不再显而易见——多个处理器同时读写同一位置时，"后续读"、"最近写"都失去了唯一定义。内存一致性就是硬件对程序员承诺的那份契约：哪些访存顺序是允许的。本期两个方向是同一件事的两半——方向①讲这份契约怎么写，方向②讲它怎么实现。有趣的是，四篇里有两篇是同年、同组、互引的姊妹作（斯坦福 CSL，ISCA 1990）：一篇提出释放一致性模型，一篇正是它的第一台实现。

**方向①**

- [Lamport SC（IEEE TC'79）](https://lamport.azurewebsites.net/pubs/multi.pdf) → [Gharachorloo 释放一致性（ISCA'90）](https://web.csl.cornell.edu/courses/ece5750/gharachorloo.isca90.pdf)

契约的写法演进：Lamport 用两页纸把"正确执行"定义清楚——顺序一致性要求任何执行的结果都等同于所有处理器的操作按某个全序串行执行、且每个处理器自身的操作保持程序顺序，并给出两条充分条件（处理器按程序顺序发出请求、同一内存模块按单一 FIFO 队列服务）[1]。但这一约束禁止了大量优化，Gharachorloo 等人提出释放一致性：只在同步点（acquire/release）强制顺序，普通访存之间不再要求，并证明只要程序标注得当（properly-labeled），释放一致性与顺序一致性对程序结果不可区分——程序员仍可按顺序一致性写程序，却能在更快的硬件上运行[2]。

**方向②**

- [DASH 目录协议（ISCA'90）](https://doi.org/10.1145/325096.325132) → [Token Coherence（ISCA'03）](http://www.cis.upenn.edu/~milom/papers/isca03_token_coherence.pdf)

契约的实现演进：DASH 用分布式目录替代总线广播，使共享内存多处理机可扩展到大量处理器——目录为每个内存块记录哪些节点持有副本，写者只向实际缓存该块的节点发送点对点失效消息；代价是缓存间传输的请求须先到达 home 节点、再由其定位并转发给持有副本的节点[3]。Token Coherence 则把一致性拆成两层：一层正确性基板保证安全与无饿死，一层性能协议只管快——由于性能协议出错只会变慢、不会变错，它可以放心地去掉间接寻址、直接在无序互连上广播请求，在 16 处理器商业负载上比目录协议快 17–54%[4]。

**也欢迎自己找**：和"内存一致性"相关即可，确定后提前发邮件告知助教（zhu_20021122[at]stu.pku.edu.cn）。

**参考文献**

[1] Lamport. *How to Make a Multiprocessor Computer That Correctly Executes Multiprocess Programs*. IEEE Transactions on Computers, 1979.

[2] Gharachorloo, Lenoski, Laudon, Gibbons, Gupta, Hennessy. *Memory Consistency and Event Ordering in Scalable Shared-Memory Multiprocessors*. ISCA 1990.

[3] Lenoski, Laudon, Gharachorloo, Gupta, Hennessy. *The Directory-Based Cache Coherence Protocol for the DASH Multiprocessor*. ISCA 1990.

[4] Martin, Hill, Wood. *Token Coherence: Decoupling Performance and Correctness*. ISCA 2003（IEEE Micro Top Picks 2003）.
