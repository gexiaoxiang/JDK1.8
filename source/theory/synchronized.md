# <center>synchronized</center>

### 简介

synchronized是由一对monitorenter和monitorexit指令来实现同步的，在JDK6之前，monitor的实现是依靠操作系统内部的互斥锁来实现的，所以需要进行用户态和内核态的切换，所以此时的同步操作是一个重量级的操作，性能很低。

但是，JDK6带来了新的变化，提供了三种monitor的实现方式，分别是偏向锁，轻量级锁和重量级锁，即锁会先从偏向锁再根据情况逐步升级到轻量级锁和重量级锁。

### 实现原理

synchronized 就是使用 monitorenter 和 monitorexit 这两个指令来实现的。根据JVM规范的要求，在执行monitorenter指令的时候，首先要去尝试获取对象的锁，如果这个对象没有被锁定，或者当前线程已经拥有了这个对象的锁，就把锁的计数器加1，相应地，在执行monitorexit的时候会把计数器减1，当计数器减小为0时，锁就释放了。

### 特性

+ 原子性、可见性、有序性

+ 可重入：同一个线程外层函数获取到锁之后，内层函数可以直接使用该锁 

+ 非公平

+ 不可中断性 ：如果这个锁被B线程获取，如果A线程想要获取这把锁，只能选择等待或者阻塞，直到B线程释放这把锁，如果B线程一直不释放这把锁，那么A线程将一直等待 

  ### 应用方式 

+ 普通同步方法（实例方法），锁是当前实例对象 ，进入同步代码前要获得当前实例的锁 

+ 静态同步方法，锁是当前类的class对象 ，进入同步代码前要获得当前类对象的锁 

+ 同步方法块，锁是括号里面的对象，对给定对象加锁，进入同步代码库前要获得给定对象的锁 

  ### **缺陷** 

  **效率低：**锁的释放情况少，只有代码执行完毕或者异常结束才会释放锁；试图获取锁的时候不能设定超时，不能中断一个正在使用锁的线程，相对而言，Lock可以中断和设置超时 

  **不够灵活**：加锁和释放的时机单一，每个锁仅有一个单一的条件（某个对象），相对而言，读写锁更加灵活

  **无法知道是否成功获得锁，**相对而言，Lock可以拿到状态，如果成功获取锁，....，如果获取失败，.....

  所以后续有Lock类对**synchronized**弥补。

   

   

   