## C程序中常见与内存有关的错误

### 间接引用坏指针

在进程的虚拟地址空间中有较大的洞,没有映射到任何有意义的数据.如果我们试图间接引用一个指向这些洞的指针, 那么操作系统就会已段异常中止.而且,虚拟内存的某些区域是只读的.试图写这些区域将以保护异常中止这个程序.
间接引用一个常见示例是经典的scanf错误.

```
/*@summary间接引用坏指针
 *假设想要使用scanf从stdin 读入一个整数到一个变量,正确方法是传递一个格式串和一个变量地址
 */
void Indirect_references_to_bad_pointers()
{
    int val = 0;

    /*在此种情况下scanf将val解释为一个地址,并试图将一个字写入这个位置.最好的情况程序以异常终止
      ,最糟糕情况,val内容对应了虚拟内存的地址,于是对这块区域进行了覆盖.
    */
    //scanf("%d", val);

    scanf("%d", &val);
}
```

### 读未初始化的内存

虽然bss内存位置总是被加载初始化为0, 但对于堆内存并不是这样,一个常见错误就是假设堆内存被初始化为0

```
void read_uninit_mem()
{
    int a[2][2] = {{0, 0}, {0, 0}};
    int x[] = {0, 0};


    //动态内存没有被初始化,没有显示的设置为0
    int *pi = (int *)malloc(4 * sizeof(int));  
    printf("pi sum = %d\n", *pi);
    for(int i = 0; i < 2; ++i)
    {
        for(int j = 0; j < 2; ++j )
        {
            printf("pi sum = %d\n", pi[i]);
            //pi[i] += a[i][j] * x[j];
        }
    }

    printf("pi sum = %d\n", *pi);
}
```

### 允许栈缓冲区溢出

如果一个程序不检查输入串长度就写入栈中的目标缓冲区,那么这个程序就会有缓冲区溢出错误(buffer overflow bug)

```
/*
* gets 函数复制一个任意长度的串到缓冲区.为了纠正这个错误.我们必须使用fgets函数
* 这个函数限制了输入串的大小,gets()函数不适用
*/
void stack_buffer_flow()
{
    char buf[5];
    // gets(buf);  //stack buffer overflow bug  ,编译都没通过
    fgets(buf, 5, stdin);

    puts(buf);
    return;
}
```

### 假设指针和他们指向的对象是相同大小的

```
/*
* 这里创建由n个指针组成的数组,每个指针都指向一个包含m个int的数组.
* 然而在bug line 将sizeof(int*) 写成了sizeof(int),代码实际创建的是int组成的数组.
* 这段代码只有在int和指向int的指针大小相同的机器上运行良好
*/
void make_array()
{
    int n = 2;
    int m = 2;

    //int **ptr_i = (int **)malloc(n * sizeof(int));  //bug line
    printf("pointer size = %d\n", sizeof(int*));
    int **ptr_i = (int **)malloc(n * sizeof(int*));
    for(int i = 0; i < n; ++i)
    {
        ptr_i[i] = (int *)malloc(m * sizeof(int));
        *ptr_i[i] = 10;
    }

    for(int i = 0; i < n; ++i)
    {
        free(ptr_i[i]);
        ptr_i[i] = NULL;
    }
    free(ptr_i);
    ptr_i = NULL;
}
```

### 造成错位错误

错位错误是另一种很常见的造成覆盖错误的来源

```
/*这里创建了n个元素的指针数组,却在bug line 初始化了n+1 个元素,覆盖了数组后面的某个内存位置 
* 执行程序中止报错 double free or corruption
*There are at least two possible situations:

you are deleting the same entity twice
you are deleting something that wasn't allocated
For the first one I strongly suggest NULL-ing all deleted pointers.
*/
void off_by_one()
{
    int n = 2;
    int m = 2;

    int **ptr_i = (int **)malloc(n * sizeof(int*));
    for(int i = 0; i <= n + m; ++i)            //bug line
    { 
        ptr_i[i] = (int *)malloc(m * sizeof(int));
        *ptr_i[i] = 10;
        printf("%d", *ptr_i[i]);

    }

    for(int i = 0; i < n; ++i)
    {
        free(ptr_i[i]);
        ptr_i[i] = NULL;
    }
    free(ptr_i);
    ptr_i = NULL;

}
```

### 引用指针,而不是它所指向的对象

如果不太注意C操作符的优先级和结合性,我们会错误的操作指针,而不是指针所指向的对象.

```
/*
* 原目的是对指针所指元素的值进行--,然而一元运算符-- 和 * 优先级相同,从右向左结合
* 所以bug line 实际对指针本身--,而不是对整数的值--.幸运的话程序立即执行失败,但是更有可能
* 发生,程序执行很久后才产生一个不正确的结果,这里的原则是当对优先级和结合性有疑问时,就是用括号
* 
*/

void bad_pointer_option()
{
    int ai[2] = {1, 2};
    //int *pi = &ai;    cannot convert 'int (*)[2]' to 'int*' in initialization
    int *pi = &ai[0];


    int p = *pi--; //  bug line
    printf("&ai[0] = %p, &p = %p\n", &ai[0], &p);
    printf("p = %d, *p-- = %d \n", p, *pi--);
    //(*pi)--; //debug 
}
```

### 误解指针预算

指针的算术操作是以他们指向的对象的大小为单位进行的,而这种大小单位并不一定是字节.

```
/*下面函数目的是扫描一个int数组,指向val位置,
* 因为每次循环,指针都被加了4, 所以函数就会扫描数组中每四个整数
*/
void bad_pointer_operation()
{
    int pArry[4] = {1, 2, 3, 4};
    int target = 4; 
    int *p = pArry;

    printf("val = %d \n", *p);

    while(*p && *p != target)
    {
        //p += sizeof(int);   //bug line  shuld be pArray++
        p++;
        printf("val = %d \n", *p);
    }
}
```

### 引用不存在的变量

返回了一个指向局部变量的指针.尽管指针指向了一个合法的地址,但是他已经不在指向一个合法变量了.当以后
在程序中调用其他函数时,内存将重用他们的栈帧,再后来如果程序分配某个值给*p,那么实际上他可能正在修改另一个函数 的栈帧中的一个条目,从而潜在带来的灾难性的

```
/*
* 返回了一个局部变量的指针
* Segmentation fault
*/
int *return_stack_val()
{
    int val = 10;
    return &val;
}   
```

### 引用空闲堆块中的数据

引用已经被释放了的堆块中的数据.

```
/*
* 示例分配了一个整数数组,然后释放他,继续使用,
* 当程序在line b 引用x[i] 时,数组x可能是某个其他已分配堆块的一部分了,因此
* 其内容被重写了,和他其他内存有关的错误一样,这个错误只会在程序执行的后面,当我们注意到
* y中的值被破坏时才会显示出来
*/
void heapref()
{
    int *x;
    int n = 2;
    x = (int*)malloc(n * sizeof(int));
    x[0] = 10; 
    free(x);
    int *y = (int *)malloc(sizeof(int));   //line a
    for(int i = 0; i < n; ++i)
    {
        y[i] = x[i]++;                  //line b
        printf("y = %d\n", *y);
    }                            

}
```

### 引起内存泄漏

内存泄漏是缓慢的杀手,当程序不小心忘记释放已分配的块,而在堆内存里创建了垃圾时,会发生这种问题.如果经常调用leek,那么渐渐的,堆里就会充满了垃圾,最糟糕的情况下,会占用整个虚拟地址空间.对于像守护进程和服务器这样的程序,内存泄漏是特别严重的,根据定义这些程序是不会终止的

Tags:
  c/c++, coredump, valgrind, 内存