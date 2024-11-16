# -1

1、通讯

全双工、半双工及单工通讯

根据通讯的数据同步方式，分为同步和异步两种，可以根据通讯过程中是否有使用到时钟信号进行简单的区分。

在同步通讯中，收发设备双方会使用一根信号线表示时钟信号，在时钟信号的驱动下双方进行协
调，同步数据.
通讯中通常双方会统一规定在时钟信号的上升沿或下降沿对数据线进行采样。

在异步通讯中不使用时钟信号进行数据同步，它们直接在数据信号中穿插一些同步用的信号位，
或者把主体数据进行打包，以数据帧的格式传输数据

![image](https://github.com/user-attachments/assets/62eefaba-5c8a-45c1-8090-95b23d14b873)


通讯速率
衡量通讯性能的一个非常重要的参数就是通讯速率，通常以比特率 (Bitrate) 来表示，即每秒钟传
输的二进制位数，单位为比特每秒 (bit/s)

容易与比特率混淆的概念是“波特率”(Baudrate)，它表示每秒钟传输了多少个码元。而码元是通讯信号调制的概念，通讯中常用时间间隔相同的符号
来表示一个二进制数字，这样的信号称为码元

如常见的通讯传输中，用 0V 表示数字 0，5V 表
示数字 1，那么一个码元可以表示两种状态 0 和 1，所以一个码元等于一个二进制比特位，此时
波特率的大小与比特率一致；如果在通讯传输中，有 0V、2V、4V 以及 6V 分别表示二进制数 00、
01、10、11，那么每个码元可以表示四种状态，即两个二进制比特位，所以码元数是二进制比特
位数的一半，这个时候的波特率为比特率的一半

因为很多常见的通讯中一个码元都是表示两种状态，人们常常直接以波特率来表示比特率，虽然严格来说没什么错误，但希望您能了解它们的
区别。

2、

在计算机科学里，大部分复杂的问题都可以通过分层来简化。如芯片被分为内核层和片上外设；STM32HAL 库是在寄存器与用户代码之间的软件层，

对于通讯协议，我们也以分层的方式来理解，最基本的是把它分为物理层和协议层。物理层规定通讯系统中具有机械、电子功能部分的
特性，确保原始数据在物理媒体的传输。协议层主要规定通讯逻辑，统一收发双方的数据打包、解包标准。简单来说物理层规定我们用嘴巴还是用肢体来交流，协议层则规定我们用中文还是英文来交流。

物理层

串口通讯的物理层有很多标准及变种，我们主要讲解 RS-232 标准，RS-232 标准主要规定了信号
的用途、通讯接口以及信号的电平标准。

![image](https://github.com/user-attachments/assets/831cc7eb-ace0-45c2-94cc-bca65f20a65a)

在上面的通讯方式中，两个通讯设备的“DB9 接口”之间通过串口信号线建立起连接，串口信号
线中使用“RS-232 标准”传输数据信号。由于 RS-232 电平标准的信号不能直接被控制器直接识
别，所以这些信号会经过一个“电平转换芯片”转换成控制器能识别的“TTL 标准”的电平信号，
才能实现通讯。

电平标准

根据通讯使用的电平标准不同，串口通讯可分为 TTL 标准及 RS-232 标准

![image](https://github.com/user-attachments/assets/a5771caa-49e4-4dd6-a43c-a881933c3482)

我们知道常见的电子电路中常使用 TTL 的电平标准，理想状态下，使用 5V 表示二进制逻辑 1，
使用 0V 表示逻辑 0；而为了增加串口通讯的远距离传输及抗干扰能力，它使用-15V 表示逻辑 1，
+15V 表示逻辑 0


RS-232 信号线

  在最初的应用中，RS-232 串口标准常用于计算机、路由与调制调解器 (MODEN，俗称“猫”) 之
  间的通讯，在这种通讯系统中，设备被分为数据终端设备 DTE(计算机、路由) 和数据通讯设备
  DCE(调制调解器)。

在目前的其它工业控制使用的串口通讯中，一般只使用 RXD、TXD 以及 GND 三条信号线，直
接传输数据信号

协议层

串口通讯的数据包由发送设备通过自身的 TXD 接口传输到接收设备的 RXD 接口。在串口通讯
的协议层中，规定了数据包的内容，它由启始位、主体数据、校验位以及停止位组成，通讯双方
的数据包格式要约定一致才能正常收发数据

![image](https://github.com/user-attachments/assets/a773655e-f134-403f-85a5-c7489ed47245)

波特率

串口异步通讯，异步通讯中由于没有时钟信号 (如前面讲解的 DB9 接口中是没有时钟信号的)，所以两个通讯设备之间需要约定好波特率，即每个码元的长度，以便对信
号进行解码

  通讯的起始和停止信号
      串口通讯的一个数据包从起始信号开始，直到停止信号结束。数据包的起始信号由一个逻辑 0 的
      数据位表示，而数据包的停止信号可由 0.5、1、1.5 或 2 个逻辑 1 的数据位表示，只要双方约定
      一致即可。
	  
  有效数据
      在数据包的起始位之后紧接着的就是要传输的主体数据内容，也称为有效数据，有效数据的长度
      常被约定为 5、6、7 或 8 位长
	  
  数据校验
  	  在有效数据之后，有一个可选的数据校验位。由于数据通信相对更容易受到外部干扰导致传输
	  数据出现偏差，可以在传输过程加上校验位来解决这个问题。校验方法有奇校验 (odd)、偶校验
	 (even)、0 校验 (space)、1 校验 (mark) 以及无校验 (noparity)
  
  	奇校验要求有效数据和校验位中“1”的个数为奇数，比如一个 8 位长的有效数据为：01101001，
  	此时总共有 4 个“1”，为达到奇校验效果，校验位为“1”，最后传输的数据将是 8 位的有效数据
  	加上 1 位的校验位总共 9 位。

 	偶校验与奇校验要求刚好相反，要求帧数据和校验位中“1”的个数为偶数，比如数据帧：11001010，
 	此时数据帧“1”的个数为 4 个，所以偶校验位为“0”。
 
	 0 校验是不管有效数据中的内容是什么，校验位总为“0”，1 校验是校验位总为“1”。

USART 简介

有别于 USART 还有一个 UART(UniversalAsynchronous Receiver and Transmitter)，它是在 USART 基础上裁剪掉了同步通信功能，只有异步
通信。简单区分同步和异步就是看通信时需不需要对外提供时钟输出，我们平时用的串口通信基
本都是 UART。

串行通信一般是以帧格式传输数据，即是一帧一帧的传输，每帧包含有起始信号、数据信息、停
止信息，可能还有校验信息。USART 就是对这些传输参数有具体规定，当然也不是只有唯一一
个参数值，很多参数值都可以自定义设置，只是增强它的兼容性

USART 在 STM32 应用最多莫过于“打印”程序信息，一般在硬件设计时都会预留一个 USART
通信接口连接电脑，用于在调试程序是可以把一些调试信息“打印”在电脑端的串口调试助手工
具上，从而了解程序运行是否正确、如果出错哪具体哪里出错等等。

USART 的功能框图包含了 USART 最核心内容，掌握了功能框图，对 USART 就有一个整体的把
握，在编程时就思路就非常清晰

![image](https://github.com/user-attachments/assets/6da7ecbb-3f1e-44be-8b5e-0f7188ae0ef7)

TX：发送数据输出引脚。
RX：接收数据输入引脚。
SW_RX：数据接收引脚，只用于单线和智能卡模式，属于内部引脚，没有具体外部引脚。
nRTS：请求以发送 (Request To Send)，n 表示低电平有效。如果使能 RTS 流控制，当 USART 接
收器准备好接收新数据时就会将 nRTS 变成低电平；当接收寄存器已满时，nRTS 将被设置为高
电平。该引脚只适用于硬件流控制。
nCTS：清除以发送 (Clear To Send)，n 表示低电平有效。如果使能 CTS 流控制，发送器在发送下
一帧数据之前会检测 nCTS 引脚，如果为低电平，表示可以发送数据，如果为高电平则在发送完
当前数据帧之后停止发送。该引脚只适用于硬件流控制。
SCLK：发送器时钟输出引脚。这个引脚仅适用于同步模式

![image](https://github.com/user-attachments/assets/bde3cf84-e614-420e-95c4-73a1d40a369a)


STM32F103VET6 系统控制器有三个 USART 和两个 UART，其中 USART1 和时钟来源于 APB2 总
线时钟，其最大频率为 72MHz，其他四个的时钟来源于 APB1 总线时钟，其最大频率为 36MHz。
UART 只是异步传输功能，所以没有 SCLK、nCTS 和 nRTS 功能引脚。

数据寄存器

USART 数据寄存器 (USART_DR) 只有低 9 位有效，并且第 9 位数据是否有效要取决于 USART
控制寄存器 1(USART_CR1) 的 M 位设置，当 M 位为 0 时表示 8 位数据字长，当 M 位为 1 表示 9
位数据字长，我们一般使用 8 位数据字长。

USART_DR 包含了已发送的数据或者接收到的数据。USART_DR 实际是包含了两个寄存器，一
个专门用于发送的可写 TDR，一个专门用于接收的可读 RDR。当进行发送操作时，往 USART_DR
写入数据会自动存储在 TDR 内；当进行读取操作时，向 USART_DR 读取数据会自动提取 RDR
数据。

TDR 和 RDR 都是介于系统总线和移位寄存器之间。串行通信是一个位一个位传输的，发送时把
TDR 内容转移到发送移位寄存器，然后把移位寄存器数据每一位发送出去，接收时把接收到的
每一位顺序保存在接收移位寄存器内然后才转移到 RDR。

USART 支持 DMA 传输，可以实现高速数据传输

控制器
USART 有专门控制发送的发送器、控制接收的接收器，还有唤醒单元、中断控制等等

使用USART 之前需要向 USART_CR1 寄存器的 UE 位置 1 使能 USART，UE 位用来开启供给给串口
的时钟。发送或者接收数据字长可选 8 位或 9 位，由 USART_CR1 的 M 位控制

	发送器
 
	当 USART_CR1 寄存器的发送使能位 TE 置 1 时，启动数据发送，发送移位寄存器的数据会在 TX
	引脚输出，低位在前，高位在后。如果是同步模式 SCLK 也输出时钟信号

	一个字符帧发送需要三个部分：起始位 + 数据帧 + 停止位。起始位是一个位周期的低电平，位周
	期就是每一位占用的时间；数据帧就是我们要发送的 8 位或 9 位数据，数据是从最低位开始传输
	的；停止位是一定时间周期的高电平

	停止位时间长短是可以通过 USART 控制寄存器 2(USART_CR2) 的 STOP[1:0] 位控制，可选 0.5
	个、1 个、1.5 个和 2 个停止位。默认使用 1 个停止位。2 个停止位适用于正常 USART 模式、单
	线模式和调制解调器模式。0.5 个和 1.5 个停止位用于智能卡模式。

	当选择 8 位字长，使用 1 个停止位时，具体发送字符时序图


	![image](https://github.com/user-attachments/assets/ce640a20-aa46-4599-a3cc-c308df97758f)


  中断控制


	![image](https://github.com/user-attachments/assets/7d9aa923-e939-4fa3-95bf-2d021ddea589)
 
USART 只需两根信号线即可完成双向通信，对硬件要求低，使得很多模块都预留 USART 接口来
实现与其他模块或者控制器进行数据传输，比如 GSM 模块，WIFI 模块、蓝牙模块等等。在硬件
设计时，注意还需要一根“共地线”。

硬件设计

为利用 USART 实现开发板与电脑通信，需要用到一个 USB 转 USART 的 IC，我们选择 CH340G
芯片来实现这个功能，CH340G 是一个 USB 总线的转接芯片，实现 USB 转 USART、USB 转 lrDA
红外或者 USB 转打印机接口，我们使用其 USB 转 USART 功能

我们将 CH340G 的 TXD 引脚与 USART1 的 RX 引脚连接，CH340G 的 RXD 引脚与 USART1 的
TX 引脚连接。CH340G 芯片集成在开发板上，其地线 (GND) 已与控制器的 GND 连通。

USB 转串口硬件设计

![image](https://github.com/user-attachments/assets/a3d7fe43-56fa-4df1-af25-354989d0ccd3)


非阻塞发送是一种通信方式，指的是在发送数据时，程序不需要等待数据全部发送完成，而是能立即返回继续执行其他任务。这种方式特别适用于实时性要求较高或需要并发处理的系统。

阻塞发送的特点
调用发送函数后，程序会一直停留在这个函数中，直到数据全部发送完成。
在此期间，CPU无法做其他工作。
简单易用，但效率低下，尤其是在需要发送大量数据或多任务处理的系统中。


串口中断函数
HAL_UART_Receive_IT
解析
1. 函数简介
HAL_UART_Receive_IT 是 STM32 HAL 库中实现的一种 非阻塞接收函数，通过中断模式接收指定数量的数据。

2. 函数功能
函数用于启动 UART 的数据接收过程，同时不会阻塞 CPU。
接收过程依赖 UART 的接收中断（RXNE 中断）来逐字节处理数据，直到完成所有接收任务。
当数据接收完成或发生错误时，相关回调函数（如 HAL_UART_RxCpltCallback）会被触发。
3. 参数说明
huart: 指向 UART_HandleTypeDef 类型的结构体，包含了 UART 的硬件配置和状态信息。
pData: 指向接收数据存储缓冲区的指针。
Size: 指定要接收的数据量（以字节或 16 位单位为数据单元）。


unsigned char data[5];

HAL_UART_Receive_IT(&huart1,data,5);

接收5个数据后，触发接收中断

要实现连续接收数据后触发中断，仅需在中断函数中
再次调用HAL_UART_Receive_IT即可。

void HAL_UART_RxCpltCallback(UART_HandleTypeDef *huart)
{
	
    HAL_GPIO_TogglePin(LED1_GPIO_Port,LED1_Pin);
	  HAL_UART_Receive_IT(&huart1,data,5);
}


实现判断接收数据为1时，使led翻转
void HAL_UART_RxCpltCallback(UART_HandleTypeDef *huart)
{
	if(data =='1'){
		HAL_GPIO_TogglePin(LED1_GPIO_Port,LED1_Pin);
	}
    
	  HAL_UART_Receive_IT(&huart1,&data,1);
}

参考网址

https://www.bilibili.com/video/BV1Qb421H7dL/?spm_id_from=333.337.search-card.all.click&vd_source=f78fe3c9e663e6bb9e1c73a434781f38

DMA—直接存储区访问

直接存储器存取(DMA)用来提供在外设和存储器之间或者存储器和存储器之间的高速数据传
输。无须CPU干预，数据可以通过DMA快速地移动，这就节省了CPU的资源来做其他操作。
两个DMA控制器有12个通道(DMA1有7个通道，DMA2有5个通道)，每个通道专门用来管理来自
于一个或多个外设对存储器访问的请求。还有一个仲裁器来协调各个DMA请求的优先权。

DMA主要特性

每个通道都直接连接专用的硬件DMA请求，每个通道都同样支持软件触发。这些功能通过
软件来配置

在同一个DMA模块上，多个请求间的优先权可以通过软件编程设置(共有四级：很高、高、
中等和低)，优先权设置相等时由硬件决定(请求0优先于请求1，依此类推) 。

闪存、SRAM、外设的SRAM、APB1、APB2和AHB外设均可作为访问的源和目标

![image](https://github.com/user-attachments/assets/f4927959-1479-4a2f-9f23-54e7a08efba3)

在发生一个事件后，外设向DMA控制器发送一个请求信号。DMA控制器根据通道的优先权处理
请求。当DMA控制器开始访问发出请求的外设时，DMA控制器立即发送给它一个应答信号。当
从DMA控制器得到应答信号时，外设立即释放它的请求。一旦外设释放了这个请求，DMA控制
器同时撤销应答信号

总之，每次DMA传送由3个操作组成：

	从外设数据寄存器或者从当前外设/存储器地址寄存器指示的存储器地址取数据，第一次传
	输时的开始地址是DMA_CPARx或DMA_CMARx寄存器指定的外设基地址或存储器单元

	存数据到外设数据寄存器或者当前外设/存储器地址寄存器指示的存储器地址，第一次传输
	时的开始地址是DMA_CPARx或DMA_CMARx寄存器指定的外设基地址或存储器单元

	执行一次DMA_CNDTRx寄存器的递减操作，该寄存器包含未完成的操作数目。

仲裁器

仲裁器根据通道请求的优先级来启动外设/存储器的访问。

优先权管理分2个阶段：

● 软件：每个通道的优先权可以在DMA_CCRx寄存器中设置，有4个等级：
─ 最高优先级
─ 高优先级
─ 中等优先级
─ 低优先级

● 硬件：如果2个请求有相同的软件优先级，则较低编号的通道比较高编号的通道有较高的优
先权。举个例子，通道2优先于通道4。


DMA 通道

每个通道都可以在有固定地址的外设寄存器和存储器地址之间执行DMA传输。DMA传输的数据
量是可编程的，最大达到65535。包含要传输的数据项数量的寄存器，在每次传输后递减


通道配置过程

下面是配置DMA通道x的过程(x代表通道号)：

	1. 在DMA_CPARx寄存器中设置外设寄存器的地址。发生外设数据传输请求时，这个地址将
	是数据传输的源或目标

	2. 在DMA_CMARx寄存器中设置数据存储器的地址。发生外设数据传输请求时，传输的数
	据将从这个地址读出或写入这个地址

	3. 在DMA_CNDTRx寄存器中设置要传输的数据量。在每个数据传输后，这个数值递减

	4. 在DMA_CCRx寄存器的PL[1:0]位中设置通道的优先级

	5. 在DMA_CCRx寄存器中设置数据传输的方向、循环模式、外设和存储器的增量模式、外
	设和存储器的数据宽度、传输一半产生中断或传输完成产生中断。
	
	6. 设置DMA_CCRx寄存器的ENABLE位，启动该通道。
 在DMA_CCRx寄存器中设置数据传输的方向、循环模式、外设和存储器的增量模式、外设和存储器的数据宽度、传输一半产生中断或传输完成产生中断。设置DMA_CCRx寄存器的ENABLE位，启动该通道。

循环模式

循环模式用于处理循环缓冲区和连续的数据传输(如ADC的扫描模式)。在DMA_CCRx寄存器中
的CIRC位用于开启这一功能。当启动了循环模式，数据传输的数目变为0时，将会自动地被恢
复成配置通道时设置的初值，DMA操作将会继续进行

存储器到存储器模式

DMA通道的操作可以在没有外设请求的情况下进行，这种操作就是存储器到存储器模式。
当设置了DMA_CCRx寄存器中的MEM2MEM位之后，在软件设置了DMA_CCRx寄存器中的EN
位启动DMA通道时，DMA传输将马上开始。当DMA_CNDTRx寄存器变为0时，DMA传输结
束。存储器到存储器模式不能与循环模式同时使用。

中断

每个DMA通道都可以在DMA传输过半、传输完成和传输错误时产生中断。为应用的灵活性考
虑，通过设置寄存器的不同位来打开这些中断。

![image](https://github.com/user-attachments/assets/f0b653aa-0f78-40c9-8e5e-cb2321eb8dfb)


DMA请求映像

DMA1控制器

从外设(TIMx[x=1、2、3、4]、ADC1、SPI1、SPI/I2S2、I2Cx[x=1、2]和USARTx[x=1、2、3])
产生的7个请求，通过逻辑或输入到DMA1控制器，这意味着同时只能有一个请求有效。参见下
图的DMA1请求映像。

外设的DMA请求，可以通过设置相应外设寄存器中的控制位，被独立地开启或关闭


![image](https://github.com/user-attachments/assets/95ad9f86-2456-4514-801d-7dd1e9d4f004)


DMA控制器(DMA)


![image](https://github.com/user-attachments/assets/0f886f4b-d0aa-4931-b76f-bee30da1f03b)

DMA寄存器

DMA中断状态寄存器(DMA_ISR)

DMA中断标志清除寄存器(DMA_IFCR)

DMA通道x配置寄存器(DMA_CCRx)(x = 1…7) 

DMA通道x传输数量寄存器(DMA_CNDTRx)(x = 1…7) 

DMA通道x外设地址寄存器(DMA_CPARx)(x = 1…7)

DMA通道x存储器地址寄存器(DMA_CMARx)(x = 1…7) 

以上是中文参考手册内容

以下是野火内容

DMA(Direct Memory Access)—直接存储器存取，是单片机的一个外设，它的主要功能是用来搬数
据，但是不需要占用 CPU，即在传输数据的时候，CPU 可以干其他的事情，好像是多线程一样。
数据传输支持从外设到存储器或者存储器到存储器，这里的存储器可以是 SRAM 或者是 FLASH。
DMA 控制器包含了 DMA1 和 DMA2，其中 DMA1 有 7 个通道，DMA2 有 5 个通道，这里的通道
可以理解为传输数据的一种管道。要注意的是 DMA2 只存在于大容量的单片机中。

DMA 请求
如果外设要想通过 DMA 来传输数据，必须先给 DMA 控制器发送 DMA 请求，DMA 收到请求信
号之后，控制器会给外设一个应答信号，当外设应答后且 DMA 控制器收到应答信号之后，就会
启动 DMA 的传输，直到传输完毕。
DMA 有 DMA1 和 DMA2 两个控制器，DMA1 有 7 个通道，DMA2 有 5 个通道，不同的 DMA 控
制器的通道对应着不同的外设请求，这决定了我们在软件编程上该怎么设置，具体见 DMA 请求映像表。


![image](https://github.com/user-attachments/assets/6ea01eda-c6ac-4b39-b1bb-524d40b63cce)


通道
DMA 具有 12 个独立可编程的通道，其中 DMA1 有 7 个通道，DMA2 有 5 个通道，每个通道对
应不同的外设的 DMA 请求。虽然每个通道可以接收多个外设的请求，但是同一时间只能接收一
个，不能同时接收多个

仲裁器
当发生多个 DMA 通道请求时，就意味着有先后响应处理的顺序问题，这个就由仲裁器也管理。
仲裁器管理 DMA 通道请求分为两个阶段。第一阶段属于软件阶段，可以在 DMA_CCRx 寄存器
中设置，有 4 个等级：非常高、高、中和低四个优先级。第二阶段属于硬件阶段，如果两个或以
上的 DMA 通道请求设置的优先级一样，则他们优先级取决于通道编号，编号越低优先权越高，
比如通道 0 高于通道 1。在大容量产品和互联型产品中，DMA1 控制器拥有高于 DMA2 控制器的
优先级。

DMA 数据配置
使用 DMA，最核心就是配置要传输的数据，包括数据从哪里来，要到哪里去，传输的数据的单
位是什么，要传多少数据，是一次传输还是循环传输等等。

从哪里来到哪里去
我们知道 DMA 传输数据的方向有三个：从外设到存储器，从存储器到外设，从存储器到存储器。
具体的方向 DMA_CCR 位 4 DIR 配置：0 表示从外设到存储器，1 表示从存储器到外设。这里面
涉及到的外设地址由 DMA_CPAR 配置，存储器地址由 DMA_CMAR 配置。

外设到存储器
当我们使用从外设到存储器传输时，以 ADC 采集为例。DMA 外设寄存器的地址对应的就是 ADC
数据寄存器的地址，DMA 存储器的地址就是我们自定义的变量（用来接收存储 AD 采集的数据）
的地址。方向我们设置外设为源地址。

存储器到外设
当我们使用从存储器到外设传输时，以串口向电脑端发送数据为例。DMA 外设寄存器的地址对
应的就是串口数据寄存器的地址，DMA 存储器的地址就是我们自定义的变量（相当于一个缓冲
区，用来存储通过串口发送到电脑的数据）的地址。方向我们设置外设为目标地址

存储器到存储器
当我们使用从存储器到存储器传输时，以内部 FLASH 向内部 SRAM 复制数据为例。DMA 外设寄
存器的地址对应的就是内部 FLASH（我们这里把内部 FALSH 当作一个外设来看）的地址，DMA
存储器的地址就是我们自定义的变量（相当于一个缓冲区，用来存储来自内部 FLASH 的数据）
的地址。方向我们设置外设（即内部 FLASH）为源地址。跟上面两个不一样的是，这里需要把
DMA_CCR 位 14：MEM2MEM：存储器到存储器模式配置为 1，启动 M2M 模式。

要传多少，单位是什么

当我们配置好数据要从哪里来到哪里去之后，我们还需要知道我们要传输的数据是多少，数据的
单位是什么。

以串口向电脑发送数据为例，我们可以一次性给电脑发送很多数据，具体多少由 DMA_CNDTR
配置，这是一个 32 位的寄存器，一次最多只能传输 65535 个数据。

要想数据传输正确，源和目标地址存储的数据宽度还必须一致，串口数据寄存器是 8 位的，所以
我们定义的要发送的数据也必须是 8 位。外设的数据宽度由 DMA_CCR 的 PSIZE[1:0] 配置，可
以是 8/16/32 位，存储器的数据宽度由 DMA_CCR 的 MSIZE[1:0] 配置，可以是 8/16/32 位。

在 DMA 控制器的控制下，数据要想有条不紊的从一个地方搬到另外一个地方，还必须正确设置
两边数据指针的增量模式。外设的地址指针由 DMA_CCRx 的 PINC 配置，存储器的地址指针由
MINC 配置。以串口向电脑发送数据为例，要发送的数据很多，每发送完一个，那么存储器的地
址指针就应该加 1，而串口数据寄存器只有一个，那么外设的地址指针就固定不变。具体的数据
指针的增量模式由实际情况决定。

什么时候传输完成

数据什么时候传输完成，我们可以通过查询标志位或者通过中断的方式来鉴别。每个 DMA 通道
在 DMA 传输过半、传输完成和传输错误时都会有相应的标志位，如果使能了该类型的中断后，
则会产生中断。有关各个标志位的详细描述请参考 DMA 中断状态寄存器 DMA_ISR 的详细描述。

传输完成还分两种模式，是一次传输还是循环传输，一次传输很好理解，即是传输一次之后就停
止，要想再传输的话，必须关断 DMA 使能后再重新配置后才能继续传输。循环传输则是一次传
输完成之后又恢复第一次传输时的配置循环传输，不断的重复。具体的由 DMA_CCR 寄存器的
CIRC 循环模式位控制


DMA 初始化结构体详解
HAL 库函数对每个外设都建立了一个初始化结构体 xxx_InitTypeDef(xxx 为外设名称)，结构体成
员用于设置外设工作参数，并由 HAL 库函数 xxx_Init() 调用这些设定参数进入设置外设相应的
寄存器，达到配置外设工作环境的目的。

DMA_InitTypeDef 初始化结构体

typedef struct {
uint32_t Direction; //传输方向
uint32_t PeriphInc; //外设递增
uint32_t MemInc; //存储器递增
uint32_t PeriphDataAlignment; //外设数据宽度
uint32_t MemDataAlignment; //存储器数据宽度
uint32_t Mode; //模式选择
uint32_t Priority; //优先级
} DMA_InitTypeDef;

1) Direction：传输方向选择，可选外设到存储器、存储器到外设以及存储器到存储器。它设定
DMA_SxCR 寄存器的 DIR[1:0] 位的值。ADC 采集显然使用外设到存储器模式。

2) PeripheralInc：如果配置为 DMA_PINC_ENABLE，使能外设地址自动递增功能，它设定
DMA_CCR 寄存器的 PINC 位的值；一般外设都是只有一个数据寄存器，所以一般不会使能该
位。

3) MemoryInc：如果配置为 DMA_MINC_ENABLE，使能存储器地址自动递增功能，它设定
DMA_CCR 寄存器的 MINC 位的值；我们自定义的存储区一般都是存放多个数据的，所以要使能
存储器地址自动递增功能。

4) PeriphDataAlignment：外设数据宽度，可选字节 (8 位)、半字 (16 位) 和字 (32 位)，它设定
DMA_SxCR 寄存器的 PSIZE[1:0] 位的值。ADC 数据寄存器只有低 16 位数据有效，使用半字
数据宽度。

5) Mode：DMA 传输模式选择，可选一次传输或者循环传输，它设定 DMA_SxCR 寄存器的 CIRC
位的值。我们希望 ADC 采集是持续循环进行的，所以使用循环传输模式

6) 软件设置数据流的优先级，有 4 个可选优先级分别为非常高、高、中和低，它设定 DMA_SxCR
寄存器的 PL[1:0] 位的值。DMA 优先级只有在多个 DMA 数据流同时使用时才有意义，这里我们
设置为非常高优先级就可以了。


存储器种类

存储器是用来存储程序代码和数据的部件，有了存储器计
算机才具有记忆功能


![image](https://github.com/user-attachments/assets/930da153-8a78-42a0-b27d-b35eaecac705)

存储器按其存储介质特性主要分为“易失性存储器”和“非易失性存储器”两大类。其中的“易
失/非易失”是指存储器断电后，它存储的数据内容是否会丢失的特性。

由于一般易失性存储器存取速度快，而非易失性存储器可长期保存数据，它们都在计算机中占据着重要角色。在计算机
中易失性存储器最典型的代表是内存，非易失性存储器的代表则是硬盘


RAM 存储器

RAM 是“Random Access Memory”的缩写，被译为随机存储器。所谓“随机存取”，指的是当存
储器中的消息被读取或写入时，所需要的时间与这段信息所在的位置无关。这个词的由来是因为
早期计算机曾使用磁鼓作为存储器，磁鼓是顺序读写设备，而 RAM 可随读取其内部任意地址的
数据，时间都是相同的，因此得名。实际上现在 RAM 已经专门用于指代作为计算机内存的易失
性半导体存储器。

根据 RAM 的存储机制，又分为动态随机存储器 DRAM(Dynamic RAM) 以及静态随机存储器
SRAM(Static RAM) 两种。

DRAM
动态随机存储器 DRAM 的存储单元以电容的电荷来表示数据，有电荷代表 1，无电荷代表 0，见
图 22_2。但时间一长，代表 1 的电容会放电，代表 0 的电容会吸收电荷，因此它需要定期刷新操
作，这就是“动态 (Dynamic)”一词所形容的特性。刷新操作会对电容进行检查，若电量大于满
电量的 1/2，则认为其代表 1，并把电容充满电；若电量小于 1/2，则认为其代表 0，并把电容放
电，藉此来保证数据的正确性。


	![image](https://github.com/user-attachments/assets/a0a6f473-b36d-4fb1-b76f-bb982c615cbc)


	SDRAM
	根据 DRAM 的通讯方式，又分为同步和异步两种，这两种方式根据通讯时是否需要使用时钟信
	号来区分。图 22_3 是一种利用时钟进行同步的通讯时序，它在时钟的上升沿表示有效数据。
	
	
	![image](https://github.com/user-attachments/assets/40428202-b9ad-4df5-98c1-cb6b7624a1f4)
	
	由于使用时钟同步的通讯速度更快，所以同步 DRAM 使用更为广泛，这种 DRAM 被称为
	SDRAM(Synchronous DRAM)
	
	DDR SDRAM
	为了进一步提高 SDRAM 的通讯速度，人们设计了 DDR SDRAM 存储器 (Double Data Rate
	SDRAM)。它的存储特性与 SDRAM 没有区别，但 SDRAM 只在上升沿表示有效数据，在 1 个
	时钟周期内，只能表示 1 个有数据；而 DDR SDRAM 在时钟的上升沿及下降沿各表示一个数据，
	也就是说在 1 个时钟周期内可以表示 2 位数据，在时钟频率同样的情况下，提高了一倍的速度。
	至于 DDRII 和 DDRIII，它们的通讯方式并没有区别，主要是通讯同步时钟的频率提高了。
	当前个人计算机常用的内存条是 DDRIII SDRAM 存储器，在一个内存条上包含多个 DDRIII
	SDRAM 芯片。

SRAM
静态随机存储器 SRAM 的存储单元以锁存器来存储数据，见图 22_4。这种电路结构不需要定时
刷新充电，就能保持状态 (当然，如果断电了，数据还是会丢失的)，所以这种存储器被称为“静
态 (Static)”RAM。部分批次 SRAM 芯片替换为 AS6C8016 型号时序与 ISSI 的芯片兼容

非易失性存储器
非易失性存储器种类非常多，半导体类的有 ROM 和 FLASH，而其它的则包括光盘、软盘及机械
硬盘。

ROM 存储器
ROM 是“Read Only Memory”的缩写，意为只能读的存储器。由于技术的发展，后来设计出了可
以方便写入数据的 ROM，而这个“Read Only Memory”的名称被沿用下来了，现在一般用于指代
非易失性半导体存储器，包括后面介绍的 FLASH 存储器，有些人也把它归到 ROM 类里边

MASK ROM
MASK(掩膜) ROM 就是正宗的“Read Only Memory”，存储在它内部的数据是在出厂时使用特殊
工艺固化的，生产后就不可修改，其主要优势是大批量生产时成本低。当前在生产量大，数据不
需要修改的场合，还有应用。

OTPROM
OTPROM(One Time Programable ROM) 是一次可编程存储器。这种存储器出厂时内部并没有资料，
用户可以使用专用的编程器将自己的资料写入，但只能写入一次，被写入过后，它的内容也不可
再修改。在 NXP 公司生产的控制器芯片中常使用 OTPROM 来存储密钥；在 STM32F429 芯片中
也具有一部分 OTPROM 空间

EPROM
EPROM(Erasable Programmable ROM) 是可重复擦写的存储器，它解决了 PROM 芯片只能写入一
次的问题。这种存储器使用紫外线照射芯片内部擦除数据，擦除和写入都要专用的设备。现在这
种存储器基本淘汰，被 EEPROM 取代。

EEPROM
EEPROM(Electrically Erasable Programmable ROM) 是电可擦除存储器。EEPROM 可以重复擦写，它
的擦除和写入都是直接使用电路控制，不需要再使用外部设备来擦写。而且可以按字节为单位修
改数据，无需整个芯片擦除。现在主要使用的 ROM 芯片都是 EEPROM。


FLASH 存储器
FLASH 存储器又称为闪存，它也是可重复擦写的储器，部分书籍会把 FLASH 存储器称为 FLASH
ROM，但它的容量一般比 EEPROM 大得多，且在擦除时，一般以多个字节为单位。如有的 FLASH
存储器以 4096 个字节为扇区，最小的擦除单位为一个扇区。根据存储单元电路的不同，FLASH
存储器又分为 NOR FLASH 和 NAND FLASH

由于 NOR 的地址线和数据线分开，它可以按“字节”读写数据，符合 CPU 的指令译码执行要求，
所以假如 NOR 上存储了代码指令，CPU 给 NOR 一个地址，NOR 就能向 CPU 返回一个数据让

而由于 NAND 的数据和地址线共用，只能按“块”来读写数据，假如 NAND 上存储了代码指令，
CPU 给 NAND 地址后，它无法直接返回该地址的数据，所以不符合指令译码要求。表 22‑2 中的
最后一项“是否支持 XIP”描述的就是这种立即执行的特性 (eXecute In Place)。
若代码存储在 NAND 上，可以把它先加载到 RAM 存储器上，再由 CPU 执行。所以在功能上可
以认为 NOR 是一种断电后数据不丢失的 RAM，但它的擦除单位与 RAM 有区别，且读写速度比
RAM 要慢得多。

另外，FLASH 的擦除次数都是有限的 (现在普遍是 10 万次左右)，当它的使用接近寿命的时候，
可能会出现写操作失败。由于 NAND 通常是整块擦写，块内有一位失效整个块就会失效，这被
称为坏块，而且由于擦写过程复杂，从整体来说 NOR 块块更少，寿命更长。由于可能存在坏块，
所以 FLASH 存储器需要“探测/错误更正 (EDC/ECC)”算法来确保数据的正确性。
由于两种 FLASH 存储器特性的差异，NOR FLASH 一般应用在代码存储的场合，如嵌入式控制器
内部的程序存储空间。而 NAND FLASH 一般应用在大数据量存储的场合，包括 SD 卡、U 盘以
及固态硬盘等，都是 NAND FLASH 类型的。

在本教程中会对如何使用 RAM、EEPROM、FLASH 存储器进行实例讲解













