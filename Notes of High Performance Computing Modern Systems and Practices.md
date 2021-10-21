# 高性能计算 现代系统与实践阅读笔记
## 第一章 绪论
### 总结
- 超级计算和高性能计算的定义：在超级计算机上进行的应用程序操作被称为超级计算，高性能计算与之同义。
- 组成超级计算机的硬件和软件系统堆栈：超级计算机的系统堆栈是由许多物理和逻辑组件组成的分层层次结构。操作系统负责控制和管理物理资源。操作系统抽象层上的软件，包括与执行用户应用程序和负载相关的资源管理。
- 持续性能和峰值性能：峰值性能是超级计算机能达到的最大性能，持续性能是运行应用时达到的实际性能，持续性能总是低于峰值性能。
- 基准测试程序：用于测试超级计算机的性能，最广为人知的之一是 HPL。
- 性能下降的来源：饥饿、延迟、开销和竞争。
- 随着摩尔定律的失效未来计算机的发展方向将依赖于其他创新，通过代替器件技术、架构甚至范式来实现。

## 第二章 HPC 架构：系统和技术
### 总结
- HPC 架构的关键特性：速度、并行性、效率、功率、可靠性、可编程性。
- 并行架构家族--弗林分类法：SISD、SIMD（整个系统的基础、也是当今异构处理器和超级计算机中更复杂控制结构的一部分）、MIMD（使用最广泛的）、MISD。
- SPMD：单程序多数据流，反应 SIMD 的实际变化。
- HPC 的支持技术：数字逻辑技术（ALU 等）、存储技术（DRAM、磁盘、磁带等）。
- 冯诺依曼顺序处理器是当今世界上所有现代超级计算机的基础。
- 流水线将多个连续性的阶段组件连接在一起，因此数据可以从一个阶段传递到另一个阶段，实现快速吞吐。
- 向量体系结构：利用流水线技术实现高速运算单元、寄存器加载和存储，以及重叠存储器访问。
- SIMD：单次执行相同的指令处理不同的数据，笔者认为是类似算法上所讲的分治策略。
- MPP：海量并行处理架构。包括许多的集成计算机处理器的单个系统。存在多种形式的处理器，通过互联方式以及处理器和存储器之间的关系来区分他们。
- 共享内存多处理器：系统中的所有处理器都可访问多个存储器组成的主存储器。SMP 在时间和带宽方面可以同等的访问所有内存。DSM（分布式共享存储）架构也共享所有的存储器，但是可以优先访问某些存储器而不是其他存储器。SMP 被称为 UMA，而DSM则表现出 NUMA 行为。
- 商品集群：利用商用或消费处理器和存储器组成的超级计算机，性价比较高，单个单元称为 COTS（商品现货），效率相较 MPP 低，但非常适合吞吐量计算，当前集群主要采用千兆以太网或InfiniBand网络。
- 阿达姆定律：S = 1/(1-f+(f/Pn)) 将可实现的交付加速比与并行加速器的增益以及可并行执行的总工作负载的比例相关联。

## 第三章 商品集群
### 总结
- 商品集群是一组集成的计算机系统，由多个组件计算机组成，单个组件计算机可以独立运行，并且可以销售给其他消费群体。所采用的集成网络是单独开发和销售的。
- 商品集群由一组处理节点、一个或多个集成节点的互联网络和辅助存储实现的。
- 节点包含作为一个独立计算机运行的所有组件。
- 由于大规模生产可实现规模经济，因此商品集群受益于相对于成本的高性能
- 并行编程的主要编程模式涉及使用与顺序语言绑定的并行库编程接口
- 常用的编程语言：C、C++、Fortran等语言
- 操作系统提供使用计算机和执行自定义应用程序所需的软件环境和服务。它由系统内核、系统库、附加系统服务，以及各种用户实用程序组成。
- 大型计算机使用资源管理系统来协调对多个执行单元的访问、内存的分配、网络选择和持久存储分配。SLURM 是常用的资源管理系统。
- 在商品集群上调试并行应用程序的简单直接的方法：为每个进程启动一个串行调试器。
- 超级计算机集群提供了几套编译器和调试工具，以支持使用集群的不同社区。各个用户环境最常使用模块系统定制。

## 第四章 基准测试程序
### 总结
- 基准测试程序是一种根据经验测量超级计算机性能的方法，它对工作负载提供了一些标准化类型，其大小或输入数据集可能变化。
- 对比了多个测试程序的原理和方向

## 第五章 资源管理的基础
### 总结
- 资源管理工具是 HPC 软件堆栈的固有部分他执行 3 个主要功能：资源分配、工作负载调度以及对分布式工作负载执行和监视的支持
- 资源分配负责根据需求将物理硬件分配到特定的用户任务。
- 资源管理器将可用的计算资源分配给用户指定的作业
- 资源管理器通常会识别计算节点、处理器核心、互联、永久存储和I/O设备以及加速器的资源类型
- 作业可以交互式或批处理执行。批处理需要在启动之前指定作业执行所需的所有必要参数和输入
- 作业可以是单个的，也可以细分为许多较小的步骤或任务
- 大多数系统使用多个作业队列，每个队列具有特定目的和一组调度约束
- 影响作业调度的常用参数：资源的可用性、优先级、分配的资源、最大作业数、请求的执行时间、已执行时间、作业依赖性、时间发生、操作人员可用性和软件许可证可用性等。
- 作业启动器采用分层机制来减轻贷款需求并利用网络拓扑最小化传输的数据量和总体启动时间
- 资源管理器必须能够终止超出其执行时间或其他资源限制的作业。而不管其当前的状态如何
- 常用的资源管理器包括：SLURM、PBS、OpenLava、Moab Cluster Suite、LoadLeveler、Univa Grid Engine、HTCondor、OAR、YARN
- 没有指定资源管理的命令格式、语言和配置的通用标准
- SLURM 是一个开源、模块化、可扩充、可扩展的资源管理器和工作负载调度软件，适用于运行 Linux 系统和其他 UNIX 操作系统的集群和超级计算机，例如神威·太湖之光。
- 使用多个备份守护进程可以消除单点故障，允许受影响的应用程序继续运行并请求资源替换失败的应用程序。
- 作业规模不一定在执行时间内固定，它们可能会增长或缩小，但不应超过规定的最大尺寸和时间限制。提供负载的调度算法，包括弹性调度、帮派调度和抢占。
- SLURM 集成了对异构组件（GPU、加速卡等）的执行支持。
- 帮派调度是一种两个或多个相似作业被分配相同的资源集，然后以交替的方式执行这些作业。
- 调度程序负责监视系统资源的状态，并决定每个作业运行的资源的子集和时间。

## 第六章 对称多处理器架构
### 总结
- SMP 也称为共享内存（SM）计算机或缓存一致性计算机。
- SMP 架构通过网络将多个处理器核心和单个共享主内存系统集成在一起
- 对称多处理属性要求缓存中保存的主存储器数据值必须一致（缓存一致性）
- 每个 SMP 都有多个 I/O 通道，可与外围设备、用户界面、数据存储、内外部网络等进行通信。
- 阿姆达定律的一个结果表明，与加速器的峰值性能增益 g 的大小无关，持续性能受原始代码可加速部分的占比 f 的限制。
- 定义处理器的特性包括每个插槽的核心数、高速缓存的大小和互连性、核心的时钟速率、每个核心中算术逻辑单元的数量和类型（ILP）等特征以及一个或多个标准化基准测试程序的交付性能。
- 流水线逻辑结构是一种利用非常细粒度的并行形式的一般方法，因为每个流水线级都在同时运行。
- “存储墙”识别出用于数据访问的处理器插槽的峰值需求率与主存储器技术可提供的吞吐量和延迟之间的不匹配。
- 内存层次结构或堆栈由内存存储技术层组成，每层在反应贷款和延迟的内存容量、成本和周期之间进行不同的权衡
- 在现代 SMP 中高速缓存通常不是单层高速储存而是多层，以实现速度和大小的最佳平衡。

## 第七章 OpenMP 的基础
### 总结
- OpenMP 可用 Fortran 和 C 等语言
- OpenMP 提供 Fork-Join 模型管理线程
- OpenMP 提供了通过系统变量调整运行参数的选项
- Visual Studio 2019 中可能需要手动在项目属性中设置 /openmp 选项以启动对 OpenMP 的支持

### 代码笔记
OpenMP 代码块
```cpp
#pragma omp parallel
{

}
```
OpenMP 代码块指定线程私有变量
```cpp
#pragma omp parallel private(value)
{

}
```
OpenMP 循环
```cpp
#pragma omp parallel
{
    #pragma omp for
    for (int i = 0; i < length; i++)
    {

    }
}
```
OpenMP 循环指定分配
```cpp
#pragma omp parallel 
{
    #pragma omp for schedule(static, chunk)
    for (int i = 0; i < length; i++)
    {

    }
}
```
OpenMP 多任务并行
```cpp
#pragma omp parallel
{
    #pragma omp sections
    {
        {
            // 第一个任务
        }
        #pragma omp section
        {
            // 第二个任务
        }
        #pragma omp section
        {
            // 第三个任务
        }
    }
}
```
OpenMP 线程安全保护
```cpp
#pragma omp parallel
{
    #pragma omp critical
    {
        // 同步运行过程
    }
}
```
OpenMP 仅限主线程执行
```cpp
#pragma omp parallel
{
    #pragma omp master
    {
        // 运行过程
    }
}
```
OpenMP 阻塞等待
```cpp
#pragma omp parallel
{
    #pragma omp barrier
}
```
OpenMP 单次执行指令（仅限一个线程运行）
```cpp
#pragma omp parallel
{
    #pragma omp single
    {
        // 运行过程
    }
}
```
OpenMP 归约（类似与 C# 的 PLINQ 最后的 Sum Count 之类的操作）
```cpp
#pragma omp parallel
{
    #pragma omp reduction(op : result)
    {
        result = result op expression // op 即 操作符（+、-、*、/、&、^、|），express 即表达式
        // 即该行语句对应 PLINQ 或者 Stream API 中的 Lambda 表达式
    }
}
```

### 代码练习
> 所有代码均使用 MSVC 142 进行编译，并在 Windows 10 平台上运行通过

输出多个“Hello, world!”与当前线程编号:
```cpp
#include <omp.h> // OpenMP 库
#include <iostream> // 标准输入输出

int main() // 应用程序主入口点
{
#pragma omp parallel // OpenMP 分枝起点
	{
		printf_s("Hello World... from thread = %d\n", omp_get_thread_num());
	} // 分枝终点
}
```
并行向量加法：
```cpp
#include <omp.h>
#include <iostream>

int main()
{
	const int max = 20;
	int i;
	double a[max], b[max], result[max];

	for (i = 0; i < max; i++)
	{
		a[i] = 1.0 * i;
		b[i] = 2.0 * i;
	}

#pragma omp parallel
	{
#pragma omp for // 并行 for 循环
		for (i = 0; i < max; i++)
		{
			result[i] = a[i] + b[i];
		}
	}

	printf_s("Test result[19] = %g/n", result[19]);
}
```
简化版本：
```cpp
#include <omp.h>
#include <iostream>

int main()
{
	const int max = 20;
	int i;
	double a[max], b[max], result[max];

	for (i = 0; i < max; i++)
	{
		a[i] = 1.0 * i;
		b[i] = 2.0 * i;
	}

#pragma omp parallel for
	for (i = 0; i < max; i++)
	{
		result[i] = a[i] + b[i];
	}

	printf_s("Test result[19] = %g/n", result[19]);
}
```
设置线程私有变量与查看当前运行线程 id
```cpp
#include <omp.h>
#include <iostream>

int main()
{
	const int max = 20;
	int i;
	double a[max], b[max], result[max];

	for (i = 0; i < max; i++)
	{
		a[i] = 1.0 * i;
		b[i] = 2.0 * i;
	}

	int threadid;

#pragma omp parallel private(threadid) // 设置线程私有变量
	{
		threadid = omp_get_thread_num();
#pragma omp for
		for (i = 0; i < max; i++)
		{
			result[i] = a[i] + b[i];
			printf_s("Thread Id: %d Index: %d\n", threadid, i);
		}
	}

	printf_s("Test result[19] = %g/n", result[19]);
}
```
设置每个线程执行的次数
```cpp
#include <omp.h>
#include <iostream>

int main()
{
	const int max = 20;
	int i;
	double a[max], b[max], result[max];

	for (i = 0; i < max; i++)
	{
		a[i] = 1.0 * i;
		b[i] = 2.0 * i;
	}

	int threadid;
	int chunk = 3; // 初始化执行次数

#pragma omp parallel private(threadid)
	{
		threadid = omp_get_thread_num();
#pragma omp for schedule(static, chunk) // 通过 schedule 语句设定每个线程执行的数量，通过静态划分
		for (i = 0; i < max; i++)
		{
			result[i] = a[i] + b[i];
			printf_s("Thread Id: %d Index: %d\n", threadid, i);
		}
	}

	printf_s("Test result[19] = %g/n", result[19]);
}
```

## 第七章 MPI的基础
### 总结
- MPI 是一个社区驱动的规范，还在不断发展，目前版本为 4.0（2021），官网为 [MPI Forum](https://www.mpi-forum.org/)
- MPI 是一系列 API 规范而不是一个库
- 在 Windows 上使用 Microsoft MPI
- MPICH 是第一个实现 MPI 标准的简缩版
- MPI 的关键要素是点对点通信和聚合通信
- MPI 可以进行非阻塞通信
- Microsoft MPI 参考文档：https://docs.microsoft.com/en-us/message-passing-interface/microsoft-mpi
### 代码笔记
MPI 初始化
```cpp
MPI_Init(const int* argc, char*** argv)
```
MPI 终止
```cpp
MPI_Finalize()
```
MPI 获取当前进程 ID
```cpp
MPI_Comm_rank(MPI_Comm comm, int* rank)
```
MPI 获取总进程数
```cpp
MPI_Comm_size(MPI_Comm comm, int* size)
```
MPI 发送、接收消息
```cpp
MPI_Send(const void* buf, int count, MPI_Datatype datatype, int dest, int tag, MPI_Comm comm)
MPI_Recv(void* buf, int count, MPI_Datatype datatype, int source, int tag, MPI_Comm comm, MPI_Status* status)
```
MPI 进程同步
```cpp
MPI_Barrier(MPI_Comm comm)
```
MPI 广播、分散、收集
```cpp
MPI_Bcast(void* buffer, int count, MPI_Datatype datatype, int root,MPI_Comm comm);
MPI_Scatter(const void* sendbuf, int sendcount, MPI_Datatype sendtype, void* recvbuf, int recvcount, MPI_Datatype recvtype, int root, MPI_Comm comm);
MPI_Gather(const void* sendbuf, int sendcount, MPI_Datatype sendtype,void* recvbuf, int recvcount, MPI_Datatype recvtype, int root, MPI_Comm comm);
```
MPI 归约
```cpp
MPI_Reduce(const void* sendbuf, void* recvbuf, int count, MPI_Datatype datatype, MPI_Op op, int root, MPI_Comm comm);
```
MPI 全局归约
```cpp
MPI_Allreduce(const void* sendbuf, void* recvbuf, int count, MPI_Datatype datatype, MPI_Op op, MPI_Comm comm);
```
MPI 全局到全局
```cpp
MPI_Alltoall(const void* sendbuf, int sendcount, MPI_Datatype sendtype, void* recvbuf, int recvcount, MPI_Datatype recvtype, MPI_Comm comm);
```
MPI 非阻塞通信
```cpp
MPI_Isend(const void* buf, int count, MPI_Datatype datatype, int dest, int tag, MPI_Comm comm, MPI_Request* request);
MPI_Irecv(void* buf, int count, MPI_Datatype datatype, int source, int tag, MPI_Comm comm, MPI_Request* request);
```

### 代码练习
> 所有代码均使用 MSVC 142 进行编译，基于Microsoft MPI 10.1.1，并在 Windows 10 平台上运行通过

初始化并显示进程 ID 和进程大小
```cpp
#include <iostream>
#include <mpi.h> // MPI 头文件

int main(int argc, char **argv)
{
	int rank, size;
	MPI_Init(&argc, &argv); // MPI 初始化
	MPI_Comm_rank(MPI_COMM_WORLD/*获取全局对象枚举*/, &rank); // 获取进程 ID
	MPI_Comm_size(MPI_COMM_WORLD, &size); // 获取总进程数
	printf_s("Hello from rank %d out of %d processes in MPI_COMM_WORLD\n", rank, size);
	MPI_Finalize();
}
```
发送并接收消息
```cpp
#include <iostream>
#include <mpi.h>

int main(int argc, char **argv)
{
	int rank, size;
	MPI_Init(&argc, &argv);
	MPI_Comm_rank(MPI_COMM_WORLD, &rank);
	MPI_Comm_size(MPI_COMM_WORLD, &size);

	int message[2];
	int dest, src;
	int tag = 0;
	MPI_Status status;

	if (size == 1)
	{
		// 在进程总数小于1时取消通信
		printf_s("This example need more than one thread.");
		MPI_Finalize();
		return -1;
	}

	if (rank != 0)
	{
		// 当前进程不为 0 号进程时发送数据
		message[0] = rank;
		message[1] = size;
		dest = 0;
		MPI_Send(message, 2, MPI_INT, dest, tag, MPI_COMM_WORLD);
	}
	else
	{
		// 当前进程为 0 号进程时遍历进程接收数据
		for (src = 1; src < size; src++)
		{
			MPI_Recv(message, 2, MPI_INT, src, MPI_ANY_TAG, MPI_COMM_WORLD, &status);
			printf_s("Hello from process %d of %d\n", message[0], message[1]);
		}
	}

	MPI_Finalize();
}
```
非阻塞发送和接收
```cpp
#include <iostream>
#include <mpi.h>

int main(int argc, char **argv)
{
	int rank, size;
	MPI_Init(&argc, &argv);
	MPI_Comm_rank(MPI_COMM_WORLD, &rank);
	MPI_Comm_size(MPI_COMM_WORLD, &size);

	int a, b;
	int dest, src;
	int tag = 0;
	MPI_Status status;
	MPI_Request send_request, recv_request;

	if (size != 2)
	{
		// 本例子只有两个进程故先判断当前总进程数
		printf_s("This example need 2 threads only.");
		MPI_Finalize();
		return -1;
	}

	if (rank == 0)
	{
		// 设定一个 a 作为发送值
		a = 1234;
		MPI_Isend(&a, 1, MPI_INT, 1, tag, MPI_COMM_WORLD, &send_request); // 设置非阻塞发送
		MPI_Irecv(&b, 1, MPI_INT, 1, tag, MPI_COMM_WORLD, &recv_request); // 设置非阻塞接收

		MPI_Wait(&send_request, &status); // 等待发送
		MPI_Wait(&recv_request, &status); // 等待接收
		printf_s("Process %d received value %d.\n", rank, b);
	}
	else
	{
		a = 5678;
		MPI_Isend(&a, 1, MPI_INT, 0, tag, MPI_COMM_WORLD, &send_request);
		MPI_Irecv(&b, 1, MPI_INT, 0, tag, MPI_COMM_WORLD, &recv_request);

		MPI_Wait(&send_request, &status);
		MPI_Wait(&recv_request, &status);
		printf_s("Process %d received value %d.\n", rank, b);
	}

	MPI_Finalize();
}
```