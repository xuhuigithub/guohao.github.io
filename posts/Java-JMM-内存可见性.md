---
title: Java-JMM-内存可见性
date: 2017-10-28 16:27:21
tags: [Java,JMM]
---

# Java多线程可见性
**可见性** 一个线程对共享变量的修改能够及时被其他线程看到。

**共享变量** 如果一个变量在多个线程的工作内存中都存在副本，那么这个变量就是这些线程的共享变量。

## Java内存模型
**Java内存模型** 描述了Java程序中共享变量的访问规则，以及在JVM中变量存储到内存和读取这样的底层细节。

1. 所有的变量都存储在主内存中
2. 每个线程都有自己的独立工作内存，工作内存中存储着共享变量的副本。

![](https://ws4.sinaimg.cn/large/006tNc79ly1fkxtz03w61j30lq0jimxr.jpg)

### 共享变量读写规定
1. 线程对共享变量的操作必须在自己的工作内存中进行，不能直接从主内存中读写。
2. 不同线程之间无法直接读写彼此的共享变量，必须通过主内存进行。

因此，线程1中对共享变量x的修改如果想让线程2看到，必须经过以下步骤。

1. 线程1将共享变量x的值写入工作内存，刷新至主内存。
2. 线程2从主内存中读取x的值，更新到线程2的工作内存。

### 共享变量可见性的重要性
共享变量如果不可见会造成线程不安全，使程序无法得到预期结果。

运行下面的Demo代码，可以发现最后的结果不是我们预期的200000，而是随机小于200000的值，这说明共享变量val在多线程之间是不可见的。

```java
public class VisibleDemo implements Runnable {
    private int val = 0;

    public static void main(String[] args) {
        VisibleDemo demo = new VisibleDemo();
        Thread t1 = new Thread(demo);
        Thread t2 = new Thread(demo);
        t1.start();
        t2.start();
        System.out.println(demo.val);
    }

    @Override
    public void run() {
        for (int i = 0; i < 100000; i++) {
            val++;
        }
    }
}
```

### 保证可见性
1. 线程对共享变量的修改必须及时刷新至主内存。
2. 线程读取共享变量前必须从主内存中更新值。

## synchroized
### Synchorized对可见性的保证
1. 线程解锁前，必须把共享变量的值同步至主内存。
2. 线程加锁后，将清空工作内存中共享变量的值。当使用时必须从主内存中读取共享变量值。

### synchroized执行过程
![](https://ws1.sinaimg.cn/large/006tNc79ly1fkxukzxgccj307w0b8q3c.jpg)

### 指令重排序
编译器为了提高程序性能而对代码的优化，可能导致代码执行的书写顺序不同。

指令重排序分为两种：

1. 编译优化的重排序（编译器优化）
2. 指令集并行重排序（处理器优化）
3. 内存系统的重排序 （处理器优化）

**as-if-serial** 重排序后的代码应该与顺序执行的代码结果一致，单线程环境下Java满足as-if-serial，多线程中程序交错执行，可能会造成内存可见性问题


## Volatile
volatile的读和写建立了一个happens-before关系，类似于申请和释放一个互斥锁。底层通过在访问共享变量时加入内存屏障和禁止重排序优化实现。
**内存屏障** 本质是总线锁，当读内存的时候，所有缓存失效，必须从内存中重新读取值。

1. 在写工作内存的共享变量副本时，会在后面添加store强制刷新至主内存
2. 在读之前加入read指令，强制读取主内存值

这里的读和写都是指执行引擎在执行代码时取操作数的读写。

### volatile只保证可见性，不保证原子性
```
int volatile x;
x++;
```

上述代码不能保证线程安全，因为x++不是原子操作，x++实际上执行了三个语句。
```
get x value;
calculate x+1
assign x=x+1
```

### volatile使用场合
1. 对变量值的写入不依赖变量的之前值，如`boolean`
2. 该共享变量没有包含在其它变量的不变式中，这一点容易被忽略。比如
    
    ```
    int volatile a;
    int volatile b;
    
    if( a > b){
      // do something
    }
    ```
    
    上述代码不是线程安全的，因为a和b的值可能被不同线程修改，无法保证（a>b）和之后的操作原子执行。


### final
由于final的值不会变化，所以多线程访问是安全的。
