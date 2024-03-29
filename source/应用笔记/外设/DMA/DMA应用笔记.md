# DMA

[TOC]

## 1. 基本功能介绍

-   不同的SoC, 其DMA功能配置不尽相同. 具备不同的DMA 实例数量且每个DMA实例可用的传输通道不一样

-   下表描述各芯片的DMA 通道配置及能力

| SoC    | DMA实例 | 通道数及FIFO深度                                       | 基础功能                 | 使用限制                                                     |
| ------ | ------- | ------------------------------------------------------ | ------------------------ | ------------------------------------------------------------ |
| GR551x | DMA0    | 支持8个通道,  每个通道 FIFO 8x4字节                    | 基础的数据传输           | 单次传输不超过4095拍                                         |
| GR5525 | DMA0    | 支持6个通道, 通道0 FIFO 32x4字节,其他通道FIFO 4x4 字节 | 基础的数据传输、S&G、LLP | 1. 单次传输不超过4095拍; <br/>2.使用LLP功能扩展单次传输数据量 |
|        | DMA1    | 支持6个通道, 通道0 FIFO 32x4字节,其他通道FIFO 4x4 字节 | 基础的数据传输、S&G、LLP | 1. 单次传输不超过4095拍; <br/>2.使用LLP功能扩展单次传输数据量 |
| GR5526 | DMA0    | 支持6个通道, 通道0 FIFO 32x4字节,其他通道FIFO 4x4 字节 | 基础的数据传输、S&G、LLP | 1. 单次传输不超过4095拍; <br/>2.使用LLP功能扩展单次传输数据量 |
|        | DMA1    | 支持6个通道, 通道0 FIFO 32x4字节,其他通道FIFO 4x4 字节 | 基础的数据传输、S&G、LLP | 1. 单次传输不超过4095拍; <br/>2.使用LLP功能扩展单次传输数据量 |
| GR533x | DMA0    | 支持5个通道, 通道0 FIFO 8x4字节,其他通道FIFO 4x4 字节  | 基础的数据传输           | 单次传输不超过4095拍                                         |

-   DMA 支持的传输方式有:

    -   Memory to Memory - 从内存到内容
    -   Memory to Peripheral - 从内存到外设
    -   Peripheral to Memory - 从外设到内存
    -   Peripheral to Peripheral - 从外设到外设
-   注意: 当传输种某一端是外设时, DMA传输信号需要依赖硬件握手信号, 对于存在多个 DMA实例的GR5525和GR5526。DMA和外设不能任意搭配, 需要从Datasheet 参考DMA 硬件握手信号分配表. 详情请参考各SoC 的Datasheet 关于DMA 及握手信号章节.
    
-   当DMA进行基础的数据传输时， 一次性最大传输4095拍数据。“拍”指DMA传输数据的位宽度。

    -   如果使用的数据位宽是字节，DMA单次最多传输数据4095字节
    -   如果使用的数据位宽是半字，DMA单次最多传输数据4095x2字节
    -   如果使用的数据位宽是字，DMA单次最多传输数据4095x4字节
    -   如果DMA一次基础传输时，数据量超过了4095拍，则会给出错误的DMA传输中断提示
    -   扩展单次传输数据超过 4095拍的方式是适用 LLP链式传输, 只有GR5525 & GR5526支持这种传输方式, GR551x和GR553x则不支持.

-   提高DMA的数据传输宽度, 可用提高DMA 的传输性能. 使用 32位传输宽度其传输性能大于 16位 大于8位. 
    
-   GR5525 和 GR5526 拓展了 DMA的 LLP和 S&G能力, 配合QSPI 等外设使用, 可以极大的提高QSPI等外设的吞吐率. 关于 LLP 和S&G的介绍, 请参考文档 : [GR552x刷屏指南](https://docs.goodix.com/zh/online/detail/display_refresh_guide_bl_b/V1.0/aee091969c2eaa366cb279984812bb42) 



## 2. 应用笔记

-   GR5525 & GR5526提供DMA0和DMA1两个可供用户使用的DMA实例，共计6个DMA通道，其中通道0 FIFO深度
    为32，通道1 ～ 5 的FIFO深度为4。
    -   由于通道0具备很深的FIFO深度，在DMA传输时候能提供极深的数据缓存，从而提高DMA传输吞吐，因此
        在穿戴类应用中，请优先将DMA0/DMA1的通道0分配给高吞吐外设使用，如用于Flash访问、PSRAM访问、大
        块Memory搬运、Display显示等  

-   GR5525 & GR5526 的DMA相对GR551x和GR553x的DMA, 拓展了如下功能:
    -   DMA链式数据传输 : 将需要传输的所有数据区块，通过指针链表方式进行管理。上一个区块发送完成
        后，根据Next链表指针自动对下一个区块进行加载，直到最后Next链表指针为空. 其优点为:
        -   突破单次传输4095拍的限制
        -   支持单次传输周期中传输不连续地址空间的数据
        -   支持单次传输周期使用不同的传输配置进行数据传输
    -   DMA分散传输: DMA分散（Scatter）传输指将连续地址空间的数据通过一定的规则，分散到不连续的地址空间的传输
    -   DMA聚合传输2: DMA聚合（Gather）传输指将不连续地址空间的数据，聚合到连续的地址空间的传输。可视作Scatter的逆向传输过程

-   GR551x和GR553x均只有一个DMA实例, 外设模块均可使用DMA 进行传输. GR5525和GR5526有2个DMA实例, 每个实例对外设的支持存在区别。

> GR5525 DMA 搭配不同外设传输的支持状态如下:
	

| 外设       | DMA0 | DMA1 |
| ---------- | ---- | ---- |
| QSPI0      | YES  | NO   |
| QSPI1      | YES  | YES  |
| QSPI2      | NO   | YES  |
| SPI Master | YES  | YES  |
| SPI Slave  | YES  | NO   |
| UART0      | YES  | YES  |
| UART1      | YES  | NO   |
| UART2      | YES  | NO   |
| UART3      | YES  | YES  |
| I2C0       | NO   | YES  |
| I2C1       | NO   | YES  |
| I2C2       | YES  | NO   |
| I2C3       | YES  | NO   |
| I2S Master | NO   | YES  |
| I2S Slave  | NO   | YES  |
| DSPI       | NO   | YES  |
| PDM        | NO   | YES  |
| ADC        | YES  | NO   |



> GR5526 DMA 搭配不同外设传输的支持状态如下:


| 外设       | DMA0 | DMA1 |
| ---------- | ---- | ---- |
| QSPI0      | YES  | NO   |
| QSPI1      | YES  | YES  |
| QSPI2      | NO   | YES  |
| SPI Master | YES  | YES  |
| SPI Slave  | YES  | NO   |
| UART0      | YES  | YES  |
| UART1      | YES  | NO   |
| UART2      | YES  | NO   |
| UART3      | YES  | YES  |
| UART4      | YES  | YES  |
| UART5      | NO   | YES  |
| I2C0       | NO   | YES  |
| I2C1       | NO   | YES  |
| I2C2       | YES  | NO   |
| I2C3       | YES  | NO   |
| I2C4       | YES  | NO   |
| I2C5       | YES  | NO   |
| I2S Master | NO   | YES  |
| I2S Slave  | NO   | YES  |
| DSPI       | NO   | YES  |
| PDM        | NO   | YES  |
| ADC        | YES  | NO   |



## 3. FAQ

### 3.1 DMA传输会用到拍的概念, 什么是拍 (Beat)?

-   拍(Beat) 是IC层面常用到的一个术语, 表示单次数据访问使用的位宽，在嵌入式领域一般是 8bit、16bit、32bit, 对应C程序的基础数据类型 char、short和int (32bit).  
    -   如果访问数据位宽使用的8bit (char), 1 拍对应的数据长度是 1字节; 
    -   如果访问数据位宽使用的16bit (short), 1 拍对应的数据长度是 2字节; 
    -   如果访问数据位宽使用的32bit (int), 1 拍对应的数据长度是 4字节; 



### 3.2 是否可以将同一个DMA实例的同一个DMA通道同时配置给不同的外设适用?

-   只有配置为Memory to Memory 的DMA实例及通道, 可以在各种 Memory (如SRAM、PSRAM及完成Memory Mapped配置的Nor Flash地址空间)之间使用
-   在其他DMA传输模式下, 目前的驱动框架不支持将同一个DMA实例的同一个通道同时分配给不同外设使用; 但可以分时使用, 每次使用前后要进行外设及dma 的 初始化和反初始化 (除非DMA通道不够用, 否则不建议这样使用, 不熟悉的情况下容易造成bug).

