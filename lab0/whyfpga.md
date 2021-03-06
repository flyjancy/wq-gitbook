# 为什么是 FPGA?

前面一节介绍了什么是 FPGA, 你可能要问: 为什么要是 FPGA? 使用 FPGA 设计 SoC 存在什么问题? 本节将致力于回答这些问题,

## FPGA 原型验证

硬件辅助的验证环境通常使用 FPGA 来对 ASIC 或 SoC 进行原型设计, 以提供更快的替代仿真的验证方案, 并允许软件开发与硬件设计并行进行. FPGA 原型验证是非常重要的, 主要体现在以下两个方面：

1. 与仅依靠软件仿真来验证其硬件设计是否合理的方法相比, 在 FPGA 原型上运行 SoC 设计是确保其功能正确的可靠方法. 只有三分之一的 SoC 设计中在第一次流片时不会出现故障, 但是, 几乎有一半的重新设计是由功能逻辑错误引起的. 单个原型平台可以在第一颗芯片流片之前就对硬件、固件和应用软件设计功能进行验证, 从而避免在流片以后再发现问题导致浪费成本. 

2. FPGA 原型设计缩短了产品的面市时间. 在当今的技术驱动型社会中, 新产品的出现非常迅速, 未能在给定的市场窗口中准备好产品会给公司带来极大的开发成本浪费. 如果产品在市场窗口中发布太晚, 则该产品可能会变得无用, 从而使公司损失了对该产品的投资资本. 设计过程完成后, 即可立马在 FPGA 上进行验证, 而 ASIC 则需要六个多月才能流片. 如果在 FPGA 原型验证阶段发现问题并及时修改, 确定以后再开始流片, 不仅节约了时间, 也节约了有故障的设计带来的额外流片费用. 

> #### fread::什么是 FPGA 原型验证? 
>
> 使用 FPGA 去模仿真实应用条件下的芯片和系统表现是否满足了实际应用场景要求.
>
> 了解更多:
>
> [关于 FPGA 原型验证](https://www.synopsys.com/blogs/smart-everything/zh-cn/2020/08/fpag-prototying/) 

## FPGA 原型验证的局限性

前面提到 FPGA 原型的重要性, FPGA 原型验证确实提高了验证的效率, 但不得不强调的是, FPGA 原型验证不能取代基于仿真的验证方法. 最主要的一个原因是, FPGA 并不是最终产品, 并且使用 FPGA 原型验证环境也有着很多需要考虑的因素：

1. 不同的物理实现: FPGA 有着不同的网表、版图、时钟树、复位电路、存储结构、I/O 端口、电源管理单元、时钟频率等. 

2. FPGA 不能实现的模块: 一个完整的设计一般不能在一片 FPGA 上完全实现, 因为这些设计可能会包含一些模拟电路, 或者规模过大需要多片 FPGA 互连, FPGA 的原型与最终的产品依然存在一些差别(比如片间互连). 

3. FPGA 的调试环境: 功能覆盖在 FPGA 原型验证环境中很难实现. 即使是使用最先进的探测技术, FPGA 内部事件的可控性和可观察性也是不足的. 如果原型环境碰巧产生了极端情况, 导致可观察到的故障, 则调试将非常耗时. 如果原型环境没有引起故障, 则很难知道它是真的没有产生极端情况, 还是设计真的没有出错. 

4. 追查错误困难: 由于 FPGA 原型环境也用于同时调试印制电路板(具有插座, 连接器等), 包含不断发展的软件, 因此追查出错的源头可能很困难且耗时. 

5. FPGA配置时间长: 对于以 FPGA 实现为目标的复杂 SoC, 综合, 布局, 静态时序分析和比特流生成的过程可能会消耗大量的处理器和内存资源, 这意味着每次对 FPGA 进行配置需要消耗大量的时间(对于一些高利用率的高速实现, 通常需要 4 到 8 个小时). 这意味着为了解决很小问题而对设计进行反复修改再利用FPGA继续验证的想法是不现实的. 

6. 工艺、电压、温度差异: FPGA 原型上的任何操作都代表所有器件环境操作条件的有限子集(以及不同的物理实现) - 即 PVT 曲线上的单个点. 即使 FPGA 原型验证环境可能会在电压和温度上施加有限的约束, 但永远不会模拟最终芯片的工艺公差. 因为大多数设计团队都关注在整个指定环境和功能操作条件下的可靠操作, 所以 FPGA 原型验证依然有其局限性. 

综上所述, 使用 FPGA 验证既有一定优势, 也有很多局限性, 因此应该充分考虑多方面的因素, 才能确保设计更好、更准确. 

