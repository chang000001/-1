# -1

全双工、半双工及单工通讯

根据通讯的数据同步方式，分为同步和异步两种，可以根据通讯过程中是否有使用到时钟信号进行简单的区分。

在同步通讯中，收发设备双方会使用一根信号线表示时钟信号，在时钟信号的驱动下双方进行协
调，同步数据.
通讯中通常双方会统一规定在时钟信号的上升沿或下降沿对数据线进行采样。

在异步通讯中不使用时钟信号进行数据同步，它们直接在数据信号中穿插一些同步用的信号位，
或者把主体数据进行打包，以数据帧的格式传输数据
