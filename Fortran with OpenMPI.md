# Fortran 的简单入门和使用 OpenMPI

## Fortran 与 C-like 语言的区别简单总结

- 无大括号，使用关键字画出范围：

  C++：

  ```c++
  int main() {
      
  }
  ```

  Fortran：

  ```fortran
  program test
  implicit none
  end program test
  ```

- 有默认定义变量类型保留，需要手动关闭，a - c 默认为实型（real），i - k 默认为整型（integer），手动关闭方法：

  ```fortran
  program test
  implicit none
  !加入上面的语句后编译器会取消掉默认的隐式类型保留
  end program test
  ```
  
- 循环只有 do-while 循环，且控制字符与 C-like 不同，do 循环类似与 for 循环，是通过数字迭代的循环，do while 循环类似与 do while 循环是判断某个布尔值来执行的。cycle 关键字的作用类似与 continue，exit 关键字的作用类似用 break。

- 定义变量的区别是 Fortran 在类型和变量间需要加 ` :: `。

- Fortran 的数组对应现实世界的矢量或者矩阵，可以直接使用线性代数中的矩阵的相关运算。

- select-case 结构对应 C-like 中的 swith-case 结构。

- 指针类型定义需要在相关类型后面添加 `, pointer`，在目标变量后面添加 `,target`，使用 `pointer => target` 方式绑定指针和目标，多个指针可以指向一个目标。

- 定义函数前需要在函数前添加 function 关键字。

- 调用外部包的或子程序时应在名称前添加 call 关键字。

## 调用 OpenMPI

1. 添加头文件

   ```fortran
   PROGRAM hello_world_mpi
   include 'mpif.h'
   ```

2. 编写测试程序

   ```fortran
   PROGRAM hello_world_mpi
   include 'mpif.h'
   
   integer process_Rank, size_Of_Cluster, ierror, tag
   
   call MPI_INIT(ierror)
   call MPI_COMM_SIZE(MPI_COMM_WORLD, size_Of_Cluster, ierror)
   call MPI_COMM_RANK(MPI_COMM_WORLD, process_Rank, ierror)
   
   print *, 'Hello World from process: ', process_Rank, 'of ', size_Of_Cluster
   
   call MPI_FINALIZE(ierror)
   END PROGRAM
   ```

3. 编译

   GNU Fortran Compiler

   ```bash
   mpif90 hello_world_mpi.f90 -o hello_world_mpi.exe
   ```

   Intel Fortran Compiler

   ```bash
   mpiifort hello_world_mpi.f90 -o hello_world_mpi.exe
   ```

4. 通过 mpirun 测试运行

   ```bash
   mpirun -np 4 ./hello_world_mpi.exe
   ```

5. 通过 slurm 运行

   job.sh：

   ```bash
   #!/bin/bash
   #SBATCH -N 1
   #SBATCH --ntasks 4
   #SBATCH --output test.out
   
   mpirun -np 4 ./hello_world_mpi.exe
   ```

   运行：

   ```bash
   sbatch job.sh
   ```

   
