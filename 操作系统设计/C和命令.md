# 《操作系统》
> 由于有各种操作系统得课程，我取长补短，形成《操作系统》
>
> 以后只学这一本就可以入门操作系统了

## 关于C

| 文件名称 | 作用     |
| -------- | -------- |
| a.o      | 目标文件 |

 **编译 -> 汇编语言(a.S) -> 汇编 -> 二进制**



* 有趣的图形界面

  其实也是调用了那些固定的系统调用
  
  strace就可以看到了
  
  



## 工具

* **readelf**	二进制程序分析器

  ```shell
  readelf -h ./app
  ```

  显示文件头内容，比如大小端模式(**little**)，起始地址，ELF等等

* **objdump** 源码回溯器 

  ```shell
  objdump -d ./app
  ```

* **strace** 	系统调用跟踪器(使用ptrace 系统调用实现)

  ``` shell
  strace -f gcc a.c -o app
  ```

  **-f**	用于跟踪子进程，因为gcc会调用很多其他的子进程

​		

##### gcc的子进程如下

| 程序名   | 功能                        |
| :------- | --------------------------- |
| ccl      | 编译器(**.c -> .S**)        |
| as       | 汇编(**.S -> relocatable**) |
| collect2 | 收集构造函数信息            |
| ld       | 链接(**link**)              |

```c
#include<stdio.h>
__attribute__((constructor)) void hello(){
         printf("hello\n");
}

 __attribute__((destructor)) void bye(){
         printf("bye\n");
}
int main(){}     
//输出了hello,bye

void libc_start(){
    call_constructor();
    main(argc,argv);
    call_destructor();
}
```

