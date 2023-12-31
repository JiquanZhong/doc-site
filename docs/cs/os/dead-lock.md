---
sidebar_position: 3
---

# 死锁

## 什么是死锁？

死锁描述多个进程因为在相互等待对方持有的资源从而进入无期限的等待的情况。由于进程被无期限地阻塞，所以程序不可能正常终止。

| 进程 | 占有资源 | 需要的资源 |
| :--: | :------: | :--------: |
|  A   |    X     |     Y      |
|  B   |    Y     |     X      |

在上面这种情况中，进程 A 占用资源 X 并且请求资源 Y，而进程 B 已经占用了资源 Y 并请求资源 X。两个进程都在等待对方释放资源，无法继续执行，陷入了死锁状态。

## 死锁产生的四个必要条件

死锁产生需要四个必要条件，任何一个条件不满足都不会产生死锁：

* 互斥条件：资源是非共享的模式，即一次只能被一个进程持有。
* 持有并等待：一个进程至少占有了一个资源，并等待另一个资源的释放
* 不可抢占：资源不能被抢占，即只能由持有的进程释放资源。
* 循环等待：存在一组等待的进程，他们互相等待下一个进程持有的资源，从而组成了一个闭合的等待链。

:::tip 注意：

解决死锁的问题，只需要破坏四个必要条件的一个即可。

必要条件在数学中的定义是：A的成立需要B成立，则B是A的必要条件。但B的成立不能推出A的成立。

:::

## 解决死锁的方法

解决死锁一般有四种方式:

* 预防: 采取某种方式对请求的资源进行限制,从而使得死锁的必要条件不成立
* 避免: 在分配资源时,提前做好预测
* 检测: 有专门的系统,在死锁发生的时候能够检测出来
* 解除: 与检测相配套的一种措施,用于将进程从死锁状态中解除出来

## 死锁的预防

死锁的预防也是从四个必要条件着手,破坏其中任意一个即可.

一般都是考虑破坏**持有并等待**和**循环等待**这两个条件,因为**互斥**和**不可抢占**在大多数业务场景下是刚需.

1. 静态分配策略: 即破坏死锁产生的**持有并等待**. 所谓静态分配, 即系统在执行前就得到了它执行所需要的所有资源. 否则就继续等待资源的完全分配. 但静态分配策略会造成资源的利用率低, 因为有些资源可能是在靠后的时间才需要的. 如果提前占用了, 就会导致其他线程一直被阻塞. 
2. 层次分配策略破坏了产生死锁的第四个条件(循环等待)。在层次分配策略下，所有的资源被分成了多个层次，一个进程得到某一次的一个资源后，它只能再申请较高一层的资源；当一个进程要释放某层的一个资源时，必须先释放所占用的较高层的资源，按这种策略，是不可能出现循环等待链的. 

## 死锁的避免

死锁预防思想是破坏四个必要条件. 但死锁的预防会造成资源利用率低的问题, 所以还需要根据系统的业务作出合适的选择. 

而死锁的避免相反, 它允许系统中**同时存在四个必要条件** ，只要掌握并发进程中与每个进程有关的资源动态申请情况，做出 **明智和合理的选择** ，仍然可以避免死锁，因为四大条件仅仅是产生死锁的必要条件。

我们可以将系统的状态分为**安全**和**不安全**两种状态. 当资源分配后可能会造成死锁的时候, 我们称系统处于不安全状态. 否则则为安全的. 比较出名的算法就是银行家算法. 

### 银行家算法

银行家算法用一句话表达就是：当一个进程申请使用资源的时候，**银行家算法** 通过先 **试探** 分配给该进程资源，然后通过 **安全性算法** 判断分配后系统是否处于安全状态，若不安全则试探分配作废，让该进程继续等待，若能够进入到安全的状态，则就 **真的分配资源给该进程**。

## 死锁的检测

这种方法对资源的分配不加以任何限制，也不采取死锁避免措施，但系统 **定时地运行一个 “死锁检测”** 的程序，判断系统内是否出现死锁，如果检测到系统发生了死锁，再采取措施去解除它。

知道了死锁检测的原理，我们可以利用下列步骤编写一个 **死锁检测** 程序，检测系统是否产生了死锁。

1. 如果进程-资源分配图中无环路，则此时系统没有发生死锁
2. 如果进程-资源分配图中有环路，且每个资源类仅有一个资源，则系统中已经发生了死锁。
3. 如果进程-资源分配图中有环路，且涉及到的资源类有多个资源，此时系统未必会发生死锁。如果能在进程-资源分配图中找出一个 **既不阻塞又非独立的进程** ，该进程能够在有限的时间内归还占有的资源，也就是把边给消除掉了，重复此过程，直到能在有限的时间内 **消除所有的边** ，则不会发生死锁，否则会发生死锁。(消除边的过程类似于 **拓扑排序**)

## 死锁的解除

当死锁检测程序检测到存在死锁发生时，应设法让其解除，让系统从死锁状态中恢复过来

常用的解除死锁的方法有以下四种：

1. **立即结束所有进程的执行，重新启动操作系统**：这种方法简单，但以前所在的工作全部作废，损失很大。
2. **撤销涉及死锁的所有进程，解除死锁后继续运行**：这种方法能彻底打破**死锁的循环等待**条件，但将付出很大代价，例如有些进程可能已经计算了很长时间，由于被撤销而使产生的部分结果也被消除了，再重新执行时还要再次进行计算。
3. **逐个撤销涉及死锁的进程，回收其资源直至死锁解除。**
4. **抢占资源**：从涉及死锁的一个或几个进程中抢占资源，把夺得的资源再分配给涉及死锁的进程直至死锁解除
