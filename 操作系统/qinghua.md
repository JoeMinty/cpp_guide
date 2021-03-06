功能：当缺页中断发生，需要调入新的页面而内存已满时，选择内存当中哪个物理页面被置换

目标：尽可能地减少页面的换进换出次数（即缺页中断的次数）。具体来说，把未来不再使用的或短期内较少使用的页面换出，通常只能在局部性原理指导下依据过去的统计数据进行预测

页面锁定（frame locking）：用于描述必须常驻内存的操作系统的关键部分或时间关键（time-critical）的应用进程。实现的方式是：在页表中添加锁定标志位（lock bit）

最优页面置换算法
基本思路：当一个缺页中断发生时，对于保存在内存当中的每一个逻辑页面，计算在它的下一次访问之前，还需等待的时间，从中选择时间最长的那个，作为被置换的页

先进先出算法
基本思路：选择在内存中驻留时间最长的页面并淘汰。具体来说，系统维护着一个链表，记录了所有位于内存当中的逻辑页面，从链表的排序看，链首页面的驻留时间最长，链尾页面的驻留时间最短。当发生一个缺页中断时，把链首页面淘汰出局，并把新的页面添加到链表尾部

性能较差，调出的页面可能是经常要访问的页面，并且有belady现象

最近最久未使用算法（LRU Least-Recently-Used）
基本思路：当一个缺页中断发生时，选择最久未被使用的那个页面淘汰

lru实现方法？

时钟页面置换算法
lru的近似，对FIFO的一种改进

基本思路：
- 需要用到页表项当中的访问位，当一个页面被装入内存时，把该位初始化为0，然后如果这个页面被访问（读/写），则把该位置为1（硬件完成）
- 把各个页面组织成环形链表（类似钟表面），把指针指向最老的页面（最先进来）
- 当发生一个缺页中断时，考察指针所指向的最老页面，若它的访问位为0，立即淘汰，若访问位为1，则置为0，然后指针往下移动一格，如此下去，直到找到被淘汰的页面，然后把指针移动到它的下一格

二次机会法
修改clock算法，使它允许脏耶总是在一次时钟头扫描中保留下来，同时使用脏位和使用位来指导置换

最不常用法（LFU Least-Frequently-Used）
基本思路：当一个缺页中断发生时，选择访问次数最少的那个页面，并淘汰

实现方法：对每个页面设置一个访问计数器，每当一个页面被访问时，该页面的访问计数器加1，在发生缺页中断时，淘汰计数值最小的那个页面

Belady现象：在采用FIFO算法时，有时会出现分配的物理页面数增加，缺页率反而提高的异常现象
Belady现象的原因：FIFO算法的置换特征与进程访问内存的动态特征是矛盾的，与置换算法的目标是不一致的（即替换较少使用的页面），因此，被它置换出去的页面并不一定是经常不会访问的

常驻集：是指在当前时刻，进程实际驻留在内存当中的页面集合

工作集大小的变化：进程开始执行后，随着访问新页面逐步建立较稳定的工作级。当内存访问的局部性区域的位置大致稳定时，工作集大小也大致稳定，局部性区域的位置改变时，工作集快速扩张和收缩过渡到下一个稳定值

工作集是进程在运行过程中固有的性质，而常驻集取决于系统分配给进程的物理页数目，以及所采用的页面置换算法

如果一个进程的整个工作集都在内存当中，即常驻集>=工作集，那么进程将很顺利的运行，而不会造成太多的缺页中断（直到工作集发生剧烈变动，从而过渡到另一个状态）

当进程常驻集的大小达到某个数目之后，再给它分配更多的物理页面，缺页率也不会明显下降

缺页率页面置换算法
可变分配策略：常驻集大小可变。例如：每个进程在刚开始运行的时候，先根据程序大小给它分配一定数目的物理页面，然后再进程运行过程中，再动态地调整常驻集的大小。
可采用全局页面置换的方式，当发生一个缺页中断时，被置换的页面可以是在其他进程当中，各个并发进程竞争地使用物理页面
优缺点：性能较好，但增加了系统开销
具体实现：可以使用缺页率算法（PFF，page fault frequency）来动态调整常驻集的大小
缺页率表示“缺页次数 / 内存访问次数”（比率）或“缺页的平均时间间隔的倒数”
影响缺页率的因素
1、页面置换算法
2、分配给进程的物理页面数目
3、页面本身的大小
4、程序的编写方法

一个交替的工作级计算明确的试图最小化页缺失
- 当缺页率高的时候-增加工作集
- 当缺页率低的时候-减少工作集

抖动问题（thrashing）
如果分配给一个进程的物理页面太少，不能包含整个的工作集，即常驻集 < 工作集，那么进程将会造成很多的缺页中断，需要频繁地在内存与外存之间替换页面，从而使进程的运行速度变得很慢，这种状态称为“抖动”

产生抖动的原因：随着驻留内存的进程数目增加，分配给每个进程的物理页面数不断减小，缺页率不断上升。所以os要选择一个适当的进程数目和进程需要的帧数，以便在并发水平和缺页率之间达到一个平衡

进程描述
定义：一个具有一定独立功能的程序在一个数据集合上的一次动态执行过程
组成：进程包含了正在运行的一个程序的所有状态信息
- 程序的代码
- 程序处理的数据
- 程序计数器中的值，指示下一条将运行的指令
- 一组通用的寄存器的当前值，堆、栈
- 一组系统资源（如打开的文件）

进程和程序的区别：
- 进程是动态的，程序是静态的：程序是有序代码的集合；进程是程序的执行，进程有内核态/用户态
- 进程是暂时的，程序是永久的：进程是一个状态变化的过程，程序可长久保存
- 进程和程序的组成不同：进程的组成包括程序，数据和进程控制块（即进程状态信息）

特点：
- 动态性：可动态创建、结束进程
- 并发性：进程可以被独立调度并占用处理机运行
- 独立性：不同进程的工作互不影响
- 制约性：因访问共享数据/资源或进程间同步而产生制约

进程控制块（PCB process control block）：描述进程的数据结构
操作系统为每个进程都维护了一个pcb，用来保存与该进程有关的各种状态信息

操作系统管理控制进程运行所用的信息集合，描述进程的基本情况以及运行变化的过程，pcb是经常存在的唯一标识

进程的创建：为该进程生成一个pcb
进程的终止：回收它的pcb
进程的组织管理：通过对pcb的组织管理来实现

pcb含有以下三大类信息：
- 进程标识信息
如本进程的标识，本进程的产生者标识（父进程标识）；用户标识
- 处理机状态信息保存区
保存进程的运行现场信息
  - 用户可见寄存器：用户程序可以使用的数据，地址等寄存器
  - 控制和状态寄存器：如程序计数器（PC），程序状态字（PSW）
  - 栈指针：过程调用/系统调用/中断处理和返回时需要用到它
- 进程控制信息
  - 调度和状态信息：用于操作系统调度进程并占用处理机使用
  - 进程间通信信息：为支持进程间的与通信相关的各种标识，信号，信件等，这些信息存在接收方的进程控制块中
  - 存储管理信息：包含有指向本进程映像存储空间的数据结构
  - 进程所用资源：说明由进程打开，使用的系统资源，如打开的文件等
  - 有关数据结构连接信息：进程可以连接到一个进程队列中，或连接到相关的其他进程的PCB

进程状态
进程的生命期管理
- 创建
引起进程创建的3个主要事件
1.系统初始化时
2.用户请求创建一个新进程
3.正在运行的进程执行了创建进程的系统调用

- 运行
内核选择一个就绪的进程，让它占用处理机并执行

- 等待
在以下情况，进程等待（阻塞）
1.请求并等待系统服务，无法马上完成
2.启动某种操作无法马上完成
3.需要的数据没有到达

进程只能自己阻塞自己，因为只有进程自身才能知道何时需要等待某种事件的发生

- 唤醒
唤醒进程的原因
1.被阻塞进程需要的资源可被满足
2.被阻塞进程等待的事件到达
3.将该进程的PCB插入到就绪队列

进程只能被别的进程或操作系统唤醒

- 结束
在以下四种情况下，进程结束
1.正常退出（资源的）
2.错误退出（资源的）
3.致命错误（强制性的）
4.被其他进程所杀（强制性的）

进程状态变化模型
进程的三种基本状态：进程在生命结束前处于且仅处于三种基本状态之一
不同系统设置的进程状态数目不同
- 运行状态 （Running）：当一个进程正在处理机上运行时
- 就绪状态（Ready）：一个进程获得了除处理机之外的一切所需资源，一旦得到处理机即可运行
- 等待状态（又称阻塞状态 Blocked）：一个进程正在等待某一事件而暂停运行时。如等待某资源，等待输入/输出完成
进程其他的基本状态
- 创建状态（New）：一个进程正在被创建，还没被转到就绪状态之前的状态
- 结束状态 （Exit）：一个进程正在从系统中消失时的状态，这是因为进程结束或者由于其他原因所导致

进程挂起
进程在挂起状态时，意味着进程没有占用内存空间。处在挂起状态的进程映像在磁盘上

挂起状态
- 阻塞挂起状态（Blocked-suspend）：进程在外存并等待某事件的出现
- 就绪挂起状态（Ready-suspend）：进程在外存，但只要进入内存，即可运行

挂起（suspend）：把一个进程从内存转到外存，可能有以下几种情况
- 阻塞到阻塞挂起：没有进程处于就绪状态或就绪进程要求更多内存资源时，会进行这种转换，已提交新进程或运行就绪进程
- 就绪到就绪挂起：当有高优先级阻塞（系统认为会很快就绪的）进程和低优先就绪进程时，系统会选择挂起低优先级就绪进程
- 运行到就绪挂起：对抢先式分时系统，当有高优先级阻塞挂起进程因事件出现而进入就绪挂起时，系统可能会把运行进程转到就绪挂起状态

在外存时的状态转换
- 阻塞挂起到就绪挂起：当有阻塞挂起进程因相关事件出现时，系统会把阻塞挂起进程转换为就绪挂起进程

与挂起相关的状态转移
解挂/激活（Active）：把一个进程从外存转到内存；可能有以下几种情况：
- 就绪挂起到就绪：没有就绪进程或挂起就绪进程优先级高于就绪进程时，会进行这种转换
- 阻塞挂起到阻塞：当一个进程释放足够多内存时，系统会把一个高优先级阻塞挂起（系统认为会很快出现所等待的事件）进程转换为阻塞进程

状态队列
- 由操作系统来维护一组队列，用来表示系统当中所有进程的当前状态
- 不同状态分别用不同的队列来表示（就绪队列，各种类型的阻塞队列）
- 每个进程的PCB都根据它的状态加入到相应的队列中，当一个进程的状态发生变化时， 它的PCB从一个状态队列脱离出来，加入到另外一个队列

线程（Thread）管理
定义：进程当中的一条执行流程

线程的优点：
- 一个进程可以同时存在多个线程
- 各个线程之间可以并发的执行
- 各个线程之间可以共享地址空间和文件等资源

线程的缺点：
一个线程崩溃，会导致其所属进程的所有线程崩溃

线程能减少并发执行的时间和空间开销
- 线程的创建时间比进程短
- 线程的终止时间比进程短
- 同一进程内的线程切换时间比进程短
- 由于同一进程的各线程间共享内存和文件资源，可直接进行不通过内核的通信

线程的实现
用户线程：在用户空间实现
内核线程：在内核中实现
轻量级进程：在内核中实现，支持用户线程

上下文切换
停止当前运行进程（从运行状态改变成其他状态）并且调度其他进程（转变成运行态）
- 必须在切换之前存储很多部分的进程上下文
- 必须能够在之后恢复它们，所以经常不能显示它曾经被暂停过
- 必须快速（上下文转换是非常频繁的）

需要存储什么上下文？
- 寄存器（pc，sp，...），cpu状态，……
- 一些时候可能会费时，所以我们应该尽可能避免

进程控制--加载和执行过程
