# 在 Linux 环境（Ubuntu）下安装 Slurm 和 OpenMPI 软件

## 安装 Slurm

1. 从软件源安装 slurm-wlm（每个节点都需要装的执行工具）、slurm-client（客户机装的提交命令的工具）、munge（节点间通信插件）

   ```bash
   sudo apt install slurm-wlm slurm-client munge
   ```

   

2. 编写 slurm.conf 文件或者使用 configurator.html 生成（需要下载源代码并通过执行 make html 生成）

   ```ini
   # 控制节点名称
   ControlMachine=AOBA-ALIENWARE
   # 控制节点 IP
   ControlAddr=127.0.1.1
   CacheGroups=0
   JobCredentialPrivateKey=/usr/local/etc/slurm/slurm.key
   JobCredentialPublicCertificate=/usr/local/etc/slurm/slurm.cert
   GroupUpdateTime=2
   MailProg=/bin/true
   MpiDefault=none
   ProctrackType=proctrack/linuxproc
   ReturnToService=1
   SlurmctldPort=6817
   SlurmdPidFile=/var/run/slurmd.%n.pid
   SlurmdPort=6818
   SlurmdSpoolDir=/var/spool/slurmd.%n
   # slurm 执行用户
   SlurmUser=slurm
   # slurmd 守护程序执行用户
   SlurmdUser=root
   StateSaveLocation=/var/spool/slurmctld/state
   SwitchType=switch/none
   TaskPlugin=task/none
   BatchStartTimeout=2
   EpilogMsgTime=1
   InactiveLimit=0
   KillWait=2
   MessageTimeout=2
   MinJobAge=2
   SlurmctldTimeout=2
   SlurmdTimeout=2
   Waittime=0
   SchedulerTimeSlice=5
   SchedulerType=sched/backfill
   SchedulerPort=7321
   SelectType=select/linear
   AccountingStorageType=accounting_storage/filetxt
   AccountingStorageLoc=/var/log/slurm/accounting
   AccountingStoreJobComment=YES
   ClusterName=mycluster
   JobCompLoc=/var/log/slurm/job_completions
   JobCompType=jobcomp/filetxt
   JobAcctGatherFrequency=2
   JobAcctGatherType=jobacct_gather/linux
   SlurmctldDebug=3
   SlurmdDebug=3
   SlurmdLogFile=/var/log/slurm-llnl/slurmd.%n.log
   # 节点信息
   # NodeName 名称、Procs 处理器分配数、NodeAddr 地址、Port 端口、State 初始状态
   NodeName=AOBA-ALIENWARE Procs=4 NodeAddr=127.0.1.1 Port=17001 State=UNKNOWN
   # 执行模式
   # PartitionName 名称、Nodes 使用节点、Default 默认、MaxTime 最大使用时间、State 初始状态
   PartitionName=mypartition Nodes=AOBA-ALIENWARE Default=YES MaxTime=INFINITE State=UP
   ```
   
   
   
3. 复制 slurm.conf 到 /etc/slurm-llnl/ 文件夹下（多节点使用 scp 分发到每个节点）
   ```bash
   sudo cp slurm.conf /etc/slurm-llnl/slurm.conf
   ```
   
4. 测试配置文件

   ```bash
   # 测试计算节点守护程序 slurmd
   sudo slurmd -D
   # 测试控制节点守护程序 slurmctld
   sudo slurmctld -D
   ```

   如果出现错误例如 File or Directory not found 等，一般是文件夹未建立，复制文件夹路径，使用 mkdir 建立，例如

   ```bash
   sudo mkdir '/var/spool/slurmctld/state'
   ```

5. 重新启动服务（本文使用 service 服务）

   ```bash
   # 控制节点守护程序
   sudo service slurmctld restart
   # 计算节点守护程序
   sudo service slurmd restart
   # 通信插件
   sudo service munge restart
   ```

6. 使用 sinfo 查看当前资源信息

   ```bash
   sinfo
   #正常工作会显示如下信息
   #PARTITION    AVAIL  TIMELIMIT  NODES  STATE NODELIST
   #mypartition*    up   infinite      1   idle AOBA-ALIENWARE
   ```

## 安装 OpenMPI

1. 从软件源安装 OpenMPI

   ```bash
   sudo apt install openmpi
   ```

2. 编写测试程序

   见文章 Notes of High Performance Computing Modern Systems and Practices 中 OpenMPI 章节中的测试程序

## Slurm 和 OpenMPI 协作工作测试

1. 编写批处理任务脚本 job.sh

   ```bash
   #!/bin/bash
   #SBATCH -N 1
   #SBATCH --ntasks 4
   #SBATCH --output test.out
   
   ## 通过 -N 指令指定节点数
   ## 通过 --ntasks 指定处理器需求数
   ## 通过 --output 指定输出文件
   ## 通过 --time 指定启动时间
   ## mpirun 运行编译好的可执行程序
   mpirun -np 4 ./test.exe
   ```

2. 通过 sbatch 运行脚本

   ```bash
   sbatch job.sh
   ```

   

3. 通过 squeue 查看运行状态

4. 使用 cat test.out 查看输出文件
