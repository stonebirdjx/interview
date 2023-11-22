<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [为什么字符串不允许修改？](#%E4%B8%BA%E4%BB%80%E4%B9%88%E5%AD%97%E7%AC%A6%E4%B8%B2%E4%B8%8D%E5%85%81%E8%AE%B8%E4%BF%AE%E6%94%B9)
- [[]byte转换成string一定会拷贝内存吗？](#byte%E8%BD%AC%E6%8D%A2%E6%88%90string%E4%B8%80%E5%AE%9A%E4%BC%9A%E6%8B%B7%E8%B4%9D%E5%86%85%E5%AD%98%E5%90%97)
- [Slice扩容原理](#slice%E6%89%A9%E5%AE%B9%E5%8E%9F%E7%90%86)
- [slice copy](#slice-copy)
- [为什么重复解锁要panic](#%E4%B8%BA%E4%BB%80%E4%B9%88%E9%87%8D%E5%A4%8D%E8%A7%A3%E9%94%81%E8%A6%81panic)
- [写操作是如何阻止写操作的](#%E5%86%99%E6%93%8D%E4%BD%9C%E6%98%AF%E5%A6%82%E4%BD%95%E9%98%BB%E6%AD%A2%E5%86%99%E6%93%8D%E4%BD%9C%E7%9A%84)
- [写操作是如何阻止读操作的](#%E5%86%99%E6%93%8D%E4%BD%9C%E6%98%AF%E5%A6%82%E4%BD%95%E9%98%BB%E6%AD%A2%E8%AF%BB%E6%93%8D%E4%BD%9C%E7%9A%84)
- [读操作是如何阻止写操作的](#%E8%AF%BB%E6%93%8D%E4%BD%9C%E6%98%AF%E5%A6%82%E4%BD%95%E9%98%BB%E6%AD%A2%E5%86%99%E6%93%8D%E4%BD%9C%E7%9A%84)
- [为什么写锁定不会被饿死](#%E4%B8%BA%E4%BB%80%E4%B9%88%E5%86%99%E9%94%81%E5%AE%9A%E4%B8%8D%E4%BC%9A%E8%A2%AB%E9%A5%BF%E6%AD%BB)
- [函数传递指针真的比传值效率高吗](#%E5%87%BD%E6%95%B0%E4%BC%A0%E9%80%92%E6%8C%87%E9%92%88%E7%9C%9F%E7%9A%84%E6%AF%94%E4%BC%A0%E5%80%BC%E6%95%88%E7%8E%87%E9%AB%98%E5%90%97)
- [make和new的区别](#make%E5%92%8Cnew%E7%9A%84%E5%8C%BA%E5%88%AB)
- [了解空指针吗](#%E4%BA%86%E8%A7%A3%E7%A9%BA%E6%8C%87%E9%92%88%E5%90%97)
- [如果用range修改切片元素的值，会发生什么](#%E5%A6%82%E6%9E%9C%E7%94%A8range%E4%BF%AE%E6%94%B9%E5%88%87%E7%89%87%E5%85%83%E7%B4%A0%E7%9A%84%E5%80%BC%E4%BC%9A%E5%8F%91%E7%94%9F%E4%BB%80%E4%B9%88)
- [怎么用go去实现一个set集合](#%E6%80%8E%E4%B9%88%E7%94%A8go%E5%8E%BB%E5%AE%9E%E7%8E%B0%E4%B8%80%E4%B8%AAset%E9%9B%86%E5%90%88)
- [map怎么顺序读取](#map%E6%80%8E%E4%B9%88%E9%A1%BA%E5%BA%8F%E8%AF%BB%E5%8F%96)
- [结构体比较](#%E7%BB%93%E6%9E%84%E4%BD%93%E6%AF%94%E8%BE%83)
- [reflect.DeepEqual](#reflectdeepequal)
- [有时候会遇到一些空的结构体，这个目的是什么](#%E6%9C%89%E6%97%B6%E5%80%99%E4%BC%9A%E9%81%87%E5%88%B0%E4%B8%80%E4%BA%9B%E7%A9%BA%E7%9A%84%E7%BB%93%E6%9E%84%E4%BD%93%E8%BF%99%E4%B8%AA%E7%9B%AE%E7%9A%84%E6%98%AF%E4%BB%80%E4%B9%88)
- [go程和线程的区别](#go%E7%A8%8B%E5%92%8C%E7%BA%BF%E7%A8%8B%E7%9A%84%E5%8C%BA%E5%88%AB)
- [defer函数遇到return以后是怎么执行的](#defer%E5%87%BD%E6%95%B0%E9%81%87%E5%88%B0return%E4%BB%A5%E5%90%8E%E6%98%AF%E6%80%8E%E4%B9%88%E6%89%A7%E8%A1%8C%E7%9A%84)
- [GMP模型](#gmp%E6%A8%A1%E5%9E%8B)
- [Golang内存模型](#golang%E5%86%85%E5%AD%98%E6%A8%A1%E5%9E%8B)
- [Golang垃圾回收](#golang%E5%9E%83%E5%9C%BE%E5%9B%9E%E6%94%B6)
- [golang内存逃逸](#golang%E5%86%85%E5%AD%98%E9%80%83%E9%80%B8)
- [golang map实现原理](#golang-map%E5%AE%9E%E7%8E%B0%E5%8E%9F%E7%90%86)
- [golang slice实现原理](#golang-slice%E5%AE%9E%E7%8E%B0%E5%8E%9F%E7%90%86)
- [golang chan实现原理](#golang-chan%E5%AE%9E%E7%8E%B0%E5%8E%9F%E7%90%86)
- [Golang中的乐观锁与悲观锁](#golang%E4%B8%AD%E7%9A%84%E4%B9%90%E8%A7%82%E9%94%81%E4%B8%8E%E6%82%B2%E8%A7%82%E9%94%81)
- [golang 读写锁底层实现](#golang-%E8%AF%BB%E5%86%99%E9%94%81%E5%BA%95%E5%B1%82%E5%AE%9E%E7%8E%B0)
- [golang可以==比较的有](#golang%E5%8F%AF%E4%BB%A5%E6%AF%94%E8%BE%83%E7%9A%84%E6%9C%89)
- [golang匿名函数和闭包的区别](#golang%E5%8C%BF%E5%90%8D%E5%87%BD%E6%95%B0%E5%92%8C%E9%97%AD%E5%8C%85%E7%9A%84%E5%8C%BA%E5%88%AB)
- [两个nil直接比较会报错](#%E4%B8%A4%E4%B8%AAnil%E7%9B%B4%E6%8E%A5%E6%AF%94%E8%BE%83%E4%BC%9A%E6%8A%A5%E9%94%99)
- [计算机硬件系统中的两种存储数据的方式](#%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%A1%AC%E4%BB%B6%E7%B3%BB%E7%BB%9F%E4%B8%AD%E7%9A%84%E4%B8%A4%E7%A7%8D%E5%AD%98%E5%82%A8%E6%95%B0%E6%8D%AE%E7%9A%84%E6%96%B9%E5%BC%8F)
- [reflect为什么慢](#reflect%E4%B8%BA%E4%BB%80%E4%B9%88%E6%85%A2)
- [golang 字符串高效拼接](#golang-%E5%AD%97%E7%AC%A6%E4%B8%B2%E9%AB%98%E6%95%88%E6%8B%BC%E6%8E%A5)
- [for 和range的性能比较](#for-%E5%92%8Crange%E7%9A%84%E6%80%A7%E8%83%BD%E6%AF%94%E8%BE%83)
- [内存对齐对性能的影响](#%E5%86%85%E5%AD%98%E5%AF%B9%E9%BD%90%E5%AF%B9%E6%80%A7%E8%83%BD%E7%9A%84%E5%BD%B1%E5%93%8D)
- [优雅关闭channel](#%E4%BC%98%E9%9B%85%E5%85%B3%E9%97%ADchannel)
- [sync.Pool复用对象](#syncpool%E5%A4%8D%E7%94%A8%E5%AF%B9%E8%B1%A1)
- [sync.Once 的原理](#synconce-%E7%9A%84%E5%8E%9F%E7%90%86)
- [sync.Cond条件变量](#synccond%E6%9D%A1%E4%BB%B6%E5%8F%98%E9%87%8F)
- [sync.Map原理](#syncmap%E5%8E%9F%E7%90%86)
- [33、sync.Mutex和sync.RWMutex](#33syncmutex%E5%92%8Csyncrwmutex)
- [拷贝大切片一定比小切片代价大吗](#%E6%8B%B7%E8%B4%9D%E5%A4%A7%E5%88%87%E7%89%87%E4%B8%80%E5%AE%9A%E6%AF%94%E5%B0%8F%E5%88%87%E7%89%87%E4%BB%A3%E4%BB%B7%E5%A4%A7%E5%90%97)
- [35、pprof 性能分析](#35pprof-%E6%80%A7%E8%83%BD%E5%88%86%E6%9E%90)
- [gdb调试](#gdb%E8%B0%83%E8%AF%95)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# 为什么字符串不允许修改？

但Go的实现中，string不包含内存空间，只有一个内存的指针，这样做的好处是string变得非常轻量，可以很方便的进行传递而不用担心内存拷贝。

因为string通常指向字符串字面量，而字符串字面量存储位置是只读段（数据段），而不是堆或栈上，所以才有了string不可修改的约定。

# []byte转换成string一定会拷贝内存吗？

赋值情况下使用unsafe包可以不内存拷贝

```go
func main() {
    a :="aaa"
    ssh := *(*reflect.StringHeader)(unsafe.Pointer(&a))
    b := *(*[]byte)(unsafe.Pointer(&ssh))  
    fmt.Printf("%v",b)
}
```

不赋值（临时使用的情况下）不会内存拷贝

- 字符串拼接，如"<" + "string(b)" + ">"；
- 字符串比较：string(b) == "foo"

因为是临时把byte切片转换成string，也就避免了因byte切片同容改成而导致string引用失败的情况，所以此时可以不必拷贝内存新建一个string。

> string 转 []byte 也是如此

# Slice扩容原理

基础类型会先内存对齐、检查容量，然后查表(sizeclass.go) 。

分单个appned和 整个切片append。

```go
package main

import "fmt"

// 单个append
// 1024 * 1.25 = 1280
// 1024 * 1.25 * 4 = 5120, 查表(sizeclass.go) 5376/4 = 1344 (真时容量)
// 1024 * 1.25 * 8 = 10240,查表(sizeclass.go)  10240/8 = 1280

// 批量append的先检查容量 查表
// 1024 * 8 = 8200  查表 9472 / 8= 1184
func main() {
	var int32s []int32
	for i := 0; i < 1025; i++ {
		int32s = append(int32s, int32(i))
		fmt.Println(len(int32s), cap(int32s))
	}

	var arr1 = make([]int64,1024,1280)
	var arr2 []int64
	arr2 = append(arr2,arr1...)
	fmt.Println(len(arr2), cap(arr2)) // 1024 1184（并不是1280）
}
```

> 所以小于1024是双倍扩容，大于1024是1.25倍扩容描述是不准确，只有当内存对齐的情况下才是。

# slice copy 

```go
copy(dst []type, src []type)
// 底层是调用 runtime.memmove
```

copy 的长度是最短切片长度（不是容量），比如dst的长度为0，容量大于src长度，也是copy不了的。

# 为什么重复解锁要panic

仔细想想Unlock的逻辑就可以理解，这实际上很难做到。Unlock过程分为将Locked置为0，然后判断Waiter值，如果值>0，则释放信号量。

如果多次Unlock()，那么可能每次都释放一个信号量，这样会唤醒多个协程，多个协程唤醒后会继续在Lock()的逻辑里抢锁，势必会增加Lock()实现的复杂度，也会引起不必要的协程切换。

> 加锁和解锁最好出现在同一个层次的代码块中，比如同一个函数。重复解锁会引起panic，应避免这种操作的可能性。
>
> 加锁后立即使用defer对其解锁，可以有效的避免死锁（但要尽量保持锁的粒度小）。

# 写操作是如何阻止写操作的

读写锁包含一个互斥锁(Mutex)，写锁定必须要先获取该互斥锁，如果互斥锁已被协程A获取（或者协程A在阻塞等待读结束），意味着协程A获取了互斥锁，那么协程B只能阻塞等待该互斥锁。

所以，写操作依赖互斥锁阻止其他的写操作。

> RWMutex 结构体中 写操作 是一个mutex

# 写操作是如何阻止读操作的

这个是读写锁实现中最精华的技巧。

RWMutex.readerCount是个整型值，用于表示读者数量，不考虑写操作的情况下，每次读锁定将该值+1，每次解除读锁定将该值-1，所以readerCount取值为[0, N]，N为读者个数，实际上最大可支持2^30个并发读者。

当写锁定进行时，会先将readerCount减去2^30，从而readerCount变成了负值，此时再有读锁定到来时检测到readerCount为负值，便知道有写操作在进行，只好阻塞等待。

> 所以，写操作将readerCount变成负值来阻止读操作的。

# 读操作是如何阻止写操作的

读锁定会先将RWMutext.readerCount加1，此时写操作到来时发现读者数量不为0，会阻塞等待所有读操作结束。

> 所以，读操作通过readerCount来将来阻止写操作的。

# 为什么写锁定不会被饿死

写操作等待期间可能还有新的读操作持续到来，如果写操作等待所有读操作结束，很可能被饿死。然而，通过RWMutex.readerWait可完美解决这个问题。

写操作到来时，会把RWMutex.readerCount值拷贝到RWMutex.readerWait中，用于标记排在写操作前面的读者个数。

前面的读操作结束后，除了会递减RWMutex.readerCount，还会递减RWMutex.readerWait值，当RWMutex.readerWait值变为0时唤醒写操作。

# 函数传递指针真的比传值效率高吗

传递指针可以减少底层值的拷贝，可以提高效率，但是如果拷贝的数据量小，由于指针传递会产生逃逸，可能会使用堆，也可能会增加GC的负担，所以传递指针不一定是高效的。

> golang都是值传递，引用类型或者指针也只是加了一层引用而已

# make和new的区别

make适用于应用类型channel，map，slice，返回申请空间后的应用对象

new适用于值类型int，float、array，struct等，返回初始零值的指针对象



# 了解空指针吗

当一个指针被定义后没有分配到任何变量时，它的值为 nil。

nil 指针也称为空指针。

```go
var ptr *int
```



# 如果用range修改切片元素的值，会发生什么

range时会拷贝切片的副本，修改切片不会影响遍历。

range map时修改map会影响遍历。



# 怎么用go去实现一个set集合

set集合无重复数据，使用空结构struct{}几乎不占用内心。unsafe.Sizeof(struct{})为0。将map的val设置为空结构体，即可得到一个高效的set集合。



# map怎么顺序读取

将map的key存入slice，对slice排序。对map取key。

将map的key存入slice，对slice排序。用sort.Slice排序，然后对slice取值。



# 结构体比较

如果struct的所有成员都可以比较，则该struct就可以通过==或!=进行比较是否相同，比较时逐个项进行比较，如果每一项都相等，则两个结构体才相等，否则不相等，只能比较是否相对，不能比较大小

相同类型的结构体才能进行比较，结构体是否相同不但与属性类型有关，还与属性顺序相关；

<font color="red">值类型才能比较，引用类型不能比较</font>



# reflect.DeepEqual

DeepEqual函数用来判断两个值是否深度一致。



# 有时候会遇到一些空的结构体，这个目的是什么

空结构体不占空间，可以占位，可以做不做消息处理的消息传递，节约空间，提升性能。



# go程和线程的区别

内存消耗更少：Goroutine所需要的内存通常只有2kb，而线程则需要1Mb（500倍）。

创建与销毁的开销更小：go程是用户级由go程自己管理，线程是系统级，需要向系统申请。

切换开销更小：这是goroutine于线程的主要区别，也是golang能够实现高并发的主要原因。线程的调度方式是抢占式的，如果一个线程的执行时间超过了分配给它的时间片，就会被其它可执行的线程抢占。



# defer函数遇到return以后是怎么执行的

return 不是原子操作，执行过程是: 保存返回值(若有)-->执行defer（若有）-->执行ret跳转。



# GMP模型

Goroutine主要概念如下：

- G（Goroutine）: 即Go协程，每个go关键字都会创建一个协程。
- M（Machine）： 工作线程，在Go中称为Machine。
- P(Processor): 处理器（Go中定义的一个摡念，不是指CPU），包含运行Go代码的必要资源，也有调度goroutine的能力。

M必须拥有P才可以执行G中的代码，P含有一个包含多个G的队列，P可以调度G交由M执行。

程序中可以使用`runtime.GOMAXPROCS()`设置P的个数，一般是一个P一个CPU。

策略有：队列轮转、系统调用、 工作量窃取（空闲的P会将其他P中的G偷取一部分过来，一般每次偷取一半）



# Golang内存模型

golang使用tcmalloc申请内存。

预申请的内存划分为spans、bitmap、arena三部分。其中**arena即为所谓的堆区**，应用中需要的内存从这里分配。其中spans和bitmap是为了管理arena区而存在的。

arena的大小为512G，为了方便管理把arena区域划分成一个个的page，每个page为8KB,一共有512GB/8KB个页；

spans区域存放span的指针，每个指针对应一个page，所以span区域的大小为(512GB/8KB)*指针大小8byte = 512M

bitmap区域大小也是通过arena计算出来，不过主要用于GC。

> *spans 管理 （8的倍数byte对象），超过32KB的对象由特殊的class管理。*

1. Golang程序启动时申请一大块内存，并划分成spans、bitmap、arena区域
2. arena区域按页划分成一个个小块
3. span管理一个或多个页
4. mcentral管理多个span供线程申请使用
5. mcache作为线程私有资源，资源来源于mcentral

# Golang垃圾回收

垃圾回收的核心就是把未引用到的回收，还给操作系统，或者提供后序内存分配使用。

gcmarkBits标记

三色标记法：

- 白色：对象未被标记，gcmarkBits对应的位为0（该对象将会在本次GC中被清理）
- 灰色：对象还在标记队列中等待
- 黑色：对象已被标记，gcmarkBits对应的位为1（该对象不会在本次GC中被清理）

初始状态下所有对象都是白色的。接下来就开始分析灰色对象，分析A时，A没有引用其他对象很快就转入黑色。最终，黑色的对象会被保留下来，白色对象会被回收掉。

Golang中的STW（Stop The World）就是停掉所有的goroutine，专心做垃圾回收，待垃圾回收结束后再恢复goroutine。

优化算法：写屏障、辅助GC

回收方式：超过阈值回收、定时回收（2分钟）、手动回收runtime.GC()

# golang内存逃逸

内存逃逸是指栈上内存逃逸到了堆上，通过编译参数-gcflags=-m 分析内存是否逃逸

逃逸场景

**指针逃逸**：方法/函数返回指针时，内存会逃逸到堆上

**栈空间不足逃逸**：函数内变量申请的资源过多（如make([]int,10000)），实际上当栈空间不足以存放当前对象时或无法判断当前切片长度时会将对象分配到堆中。

栈空间由处理器决定一般是2-4M，极少数是32兆。

**动态类型逃逸**：interface{}传参时，编译期间很难确定其参数的具体类型，也容易引起内存逃逸

**闭包引用对象逃逸**：闭包引用了函数的局部变量时，会发生内存逃逸。

# golang map实现原理

map使用哈希表作为底层实现，数据结构是hash数组+桶内的key-value数组+溢出的桶链表。

每个桶可以存储8个键值对。通过地址链的方式解决hash冲突。

增量扩容（2倍）：当平均每个bucket存储超过6.5个key，会增量扩容。

等量扩容（内部整理移动）：所谓等量扩容，实际上并不是扩大容量，buckets数量不变，重新做一遍类似增量扩容的搬迁动作，把松散的键值对重新排列一次，以使bucket的使用率更高，进而保证更快的存取。

# golang slice实现原理

底层也是一个数组，容量满了时会扩容。

```go
type slice struct {
	array unsafe.Pointer
	len   int
	cap   int
}
```

如果原Slice容量小于1024，则新Slice容量将扩大为原来的2倍；
如果原Slice容量大于等于1024，则新Slice容量将扩大为原来的1.25倍；

# golang chan实现原理

原理：以通信的方式共享内存。底层是安全队列FIFO，不设置缓冲就是同步队列，设置了就是带缓冲的队列。

# Golang中的乐观锁与悲观锁

sync/atomic

​	Golang中有一个 atomic 包，可以在不形成临界区和创建互斥量的情况下完成并发安全的值替换操作，这个包应用的便是乐观锁的原理。不过这个包只支持int32/int64/uint32/uint64/uintptr这几种数据类型的一些基础操作（增减、交换、载入、存储等）

sync

​	Golang中的sync包，提供了各种锁，如果使用了这个包，基本上就以悲观锁的工作模式了。Mutex是悲观锁、RWMutex是乐观锁。

# golang 读写锁底层实现

读写锁内部仍有一个互斥锁，用于将两个写操作隔离开来，其他的几个都用于隔离读操作和写操作。

**RLock()实现逻辑**

- 增加读操作计数，即readerCount++
- 阻塞等待写操作结束(如果有的话)

**RUnlock()实现逻辑**

- 减少读操作计数，即readerCount--
- 唤醒等待写操作的协程（如果有的话）

# golang可以==比较的有

- 值类型可以直接比较

- 复合类型（数组和结构体），只有每个元素(成员)可比较，而且类型和值都相等时，两个复合元素才相等

- 相同类型的channel可以比较。

- slice、map、函数不能比较。但是可以用reflect.DeepEqual或者cmp包来比较

- 类型再定义(type A string)不可比较，是两种不同的类型

- 类型别名(type A = string)可比较，是同一种类型。

  

# golang匿名函数和闭包的区别

匿名函数没有函数名，只有函数体，它只有在被调用的时候才会被初始化。

匿名函数一般被当作一种类型被赋值给类型为函数类型的变量，经常用于实现回调函数和闭包等功能。

闭包一般按正常的函数定义，闭包  = 环境引用 + 函数

> *当匿名函数实现闭包功能时和闭包并无什么差异* ，要注意内存逃逸

匿名函数作用就是语音简洁，提取重复代码块儿，方便使用与维护。

# 两个nil直接比较会报错

两个nil直接比较会报错

# 计算机硬件系统中的两种存储数据的方式

大端字节序、小端字节序

大端模式：高位字节排放在内存的低地址端，低位字节排放在内存的高地址端;这样的存储模式有点儿类似于把数据当作字符串顺序处理：地址由小向大增加，而数据从高位往低位放。和我们”从左到右“阅读习惯一致。

小端模式：低位字节排放在内存的低地址端，高位字节排放在内存的高地址端；这种存储模式将地址的高低和数据位权有效地结合起来，高地址部分权值高，低地址部分权值低，和我们的逻辑方法一致

go 通过unsafe转成byte，在判断是否和期望值一致

```go
package main

import (
	"fmt"
	"unsafe"
)

func IsLittleEndian() bool {
	var value int32 = 1 // 占4byte 转换成16进制 0x00 00 00 01 
	// 大端(16进制)：00 00 00 01
	// 小端(16进制)：01 00 00 00
	pointer := unsafe.Pointer(&value)
	pb := (*byte)(pointer)
	if *pb != 1 {
		return false
	}
	return true
}

func main() {
	fmt.Println(IsLittleEndian())
}

// 运行结果：ture
```

使用encoding/binary转换

```go
// use encoding/binary
// bigEndian littleEndian
func BigEndianAndLittleEndianByLibrary()  {
	var value uint32 = 10
	by := make([]byte,4)
	binary.BigEndian.PutUint32(by,value)
	fmt.Println("转换成大端后 ",by)
	fmt.Println("使用大端字节序输出结果：",binary.BigEndian.Uint32(by))
	little := binary.LittleEndian.Uint32(by)
	fmt.Println("大端字节序使用小端输出结果：",little)
}
// 结果：
转换成大端后  [0 0 0 10]
使用大端字节序输出结果： 10
大端字节序使用小端输出结果： 167772160
```

> *grpc特意指定了使用大端字节序*

# reflect为什么慢

标准库 reflect为 Go 语言提供了运行时动态获取对象的类型和值以及动态创建对象的能力。反射可以帮助抽象和简化代码，提高开发效率。

1、涉及到内存分配以及后续的GC；

2、reflect实现里面有大量的枚举，也就是for循环，比如类型之类的。

通过反射获取结构体的字段有两种方式，一种是 `FieldByName`，另一种是 `Field`(按照下标)。

在反射的内部，字段是按顺序存储的，因此按照下标访问查询效率为 O(1)，而按照 `Name` 访问，则需要遍历所有字段，查询效率为 O(N)。结构体所包含的字段(包括方法)越多，那么两者之间的效率差距则越大。

`FieldByName` 相比于 `Field` 有一个数量级的性能劣化。那在实际的应用中，就要避免直接调用 `FieldByName`。我们可以利用字典将 `Name` 和 `Index` 的映射缓存起来。避免每次反复查找，耗费大量的时间。

```go
func BenchmarkReflect_FieldByNameCacheSet(b *testing.B) {
	typ := reflect.TypeOf(Config{})
	cache := make(map[string]int)
	for i := 0; i < typ.NumField(); i++ {
		cache[typ.Field(i).Name] = i
	}
	ins := reflect.New(typ).Elem()
	b.ResetTimer()
	for i := 0; i < b.N; i++ {
		ins.Field(cache["Name"]).SetString("name")
		ins.Field(cache["IP"]).SetString("ip")
		ins.Field(cache["URL"]).SetString("url")
		ins.Field(cache["Timeout"]).SetString("timeout")
	}
}
```

# golang 字符串高效拼接

使用 `+` 和 `fmt.Sprintf` 的效率是最低的，`strings.Builder`、`bytes.Buffer` 和 `[]byte` 的性能差距不大。

一般推荐使用 `strings.Builder` 来拼接字符串。

`strings.Builder` 和 `bytes.Buffer` 底层都是 `[]byte` 数组，但 `strings.Builder` 性能比 `bytes.Buffer` 略快约 10% 。一个比较重要的区别在于，`bytes.Buffer` 转化为字符串时重新申请了一块空间，存放生成的字符串变量，而 `strings.Builder` 直接将底层的 `[]byte` 转换成了字符串类型返回了回来



# for 和range的性能比较

range 可以用来很方便地遍历数组(array)、切片(slice)、字典(map)和信道(chan)、字符串

range 在迭代过程中返回的是迭代值的拷贝，如果每次迭代的元素的内存占用很低，那么 for 和 range 的性能几乎是一样，例如 `[]int`。

但是如果迭代的元素内存占用较高，例如一个包含很多属性的 struct 结构体，那么 for 的性能将显著地高于 range，有时候甚至会有上千倍的性能差异。**对于这种场景，建议使用 for，如果使用 range，建议只迭代下标，通过下标访问迭代值，这种使用方式和 for 就没有区别了。**

如果想使用 range 同时迭代下标和值，则需要将切片/数组的元素改为指针，才能不影响性能。

# 内存对齐对性能的影响

一个结构体实例所占据的空间等于各字段占据空间之和，再加上内存对齐的空间大小。

```go
package main

import (
	"fmt"
	"unsafe"
)

type Args struct {
	num1 int
	num2 int
}

type Flag struct {
	num1 int16
	num2 int32
}

func main() {
	fmt.Println(unsafe.Sizeof(Args{}))
	fmt.Println(unsafe.Sizeof(Flag{}))
}
```

- `Args` 由 2 个 int 类型的字段构成，在 64位机器上，一个 int 占 8 字节，因此存储一个 `Args` 实例需要 16 字节。
- `Flag` 由一个 int32 和 一个 int16 的字段构成，成员变量占据的字节数为 4+2 = 6，但是 `unsafe.Sizeof` 返回的结果为 8 字节，多出来的 2 字节是内存对齐的结果。

CPU 访问内存时，并不是逐个字节访问，而是以字长（word size）为单位访问。

> *比如 32 位的 CPU ，字长为 4 字节，那么 CPU 访问内存的单位也是 4 字节。*
>
> *64位CPU访问8字节*

`unsafe` 标准库提供了 `Alignof` 方法，可以返回一个类型的对齐值，也可以叫做对齐系数或者对齐倍数。

```go
unsafe.Alignof(Args{}) // 8
unsafe.Alignof(Flag{}) // 4
```

- `Args{}` 的对齐倍数是 8，`Args{}` 两个字段占据 16 字节，是 8 的倍数，无需占据额外的空间对齐。
- `Flag{}` 的对齐倍数是 4，因此 `Flag{}` 占据的空间必须是 4 的倍数，因此，6 内存对齐后是 8 字节。

**合理布局减少内存占用**

```go
type demo1 struct {
	a int8
	b int16
	c int32
}

type demo2 struct {
	a int8
	c int32
	b int16
}

func main() {
	fmt.Println(unsafe.Sizeof(demo1{})) // 8
	fmt.Println(unsafe.Sizeof(demo2{})) // 12
}
```

# 优雅关闭channel

channel只能被生产者关闭，可以使用 sync.Once 或互斥锁(sync.Mutex)确保 channel 只被关闭一次。

# sync.Pool复用对象

> *保存和复用临时对象，减少内存分配，降低 GC 压力。*

sync.Pool 是 Golang 内置的对象池技术，可用于缓存临时对象，避免因频繁建立临时对象所带来的消耗以及对 GC 造成的压力。

并发安全

```go
type Test struct {
	A int
}

func main() {
	pool := sync.Pool{
		New: func() interface{} {
			return &Test{
				A: 1,
			}
		},
	}

	testObject := pool.Get().(*Test)
	println(testObject.A) // print 1

	pool.Put(testObject)
}
```

- `Get()` 用于从对象池中获取对象，因为返回值是 `interface{}`，因此需要类型转换。
- `Put()` 则是在对象使用完毕后，返回对象池。

1. 利用 GMP 的特性，为每个 P 创建了一个本地对象池 poolLocal，尽量减少并发冲突。
2. 每个 poolLocal 都有一个 private 对象，优先存取 private 对象，可以避免进入复杂逻辑。
3. 在 Get 和 Put 期间，利用 `pin` 锁定当前 P，防止 goroutine 被抢占，造成程序混乱。
4. 在获取对象期间，利用对象窃取的机制，从其他 P 的本地对象池以及 victim 中获取对象。
5. 充分利用 CPU Cache 特性，提升程序性能。

# sync.Once 的原理

首先：保证变量仅被初始化一次，需要有个标志来判断变量是否已初始化过，若没有则需要初始化。

第二：线程安全，支持并发，无疑需要互斥锁来实现。

```go
type Once struct {
    done uint32
    m    Mutex
}
```

# sync.Cond条件变量

`sync.Cond` 经常用在多个 goroutine 等待，一个 goroutine 通知（事件发生）的场景。如果是一个通知，一个等待，使用互斥锁或 channel 就能搞定了。

```go
type Cond
func NewCond(l Locker) *Cond
func (c *Cond) Broadcast() //全部 Wait 的 goroutine 都唤醒。
func (c *Cond) Signal() //只唤醒一个最先 Wait 的 goroutine。
func (c *Cond) Wait() //等待被唤醒。唤醒期间会解锁并切走 goroutine。
```

1. sync.Cond不能拷贝，否则将会造成`panic("sync.Cond is copied")`错误
2. Wait 的调用一定要放在 Lock 和 UnLock 中间，否则将会造成panic("sync: unlock of unlocked mutex") 
3. Wait 调用的条件检查一定要放在 for 循环中，代码如上。这是因为当 Boardcast 唤醒时，有可能其他 goroutine 先于当前 goroutine 唤醒并抢到锁，导致轮到当前 goroutine 抢到锁的时候，条件又不再满足了。因此，需要将条件检查放在 for 循环中。
4. Signal 和 Boardcast 两个唤醒操作不需要加锁。

 ```go
package main

import (
	"log"
	"sync"
	"time"
)

var done = false

func read(name string, c *sync.Cond) {
	c.L.Lock()
	for !done {
		c.Wait()
	}
	log.Println(name, "starts reading")
	c.L.Unlock()
}

func write(name string, c *sync.Cond) {
	log.Println(name, "starts writing")
	time.Sleep(time.Second)
	c.L.Lock()
	done = true
	c.L.Unlock()
	log.Println(name, "wakes all")
	c.Broadcast()
}

func main() {
	cond := sync.NewCond(&sync.Mutex{})

	go read("reader1", cond)
	go read("reader2", cond)
	go read("reader3", cond)
	write("writer", cond)

	time.Sleep(time.Second * 3)
}
 ```

# sync.Map原理

> *无须初始化，直接声明即可。*

- 通过 read 和 dirty 两个字段将读写分离，读的数据存在只读字段 read 上，将最新写入的数据则存在 dirty 字段上
- 读取时会先查询 read，不存在再查询 dirty，写入时则只写入 dirty
- **读取 read 并不需要加锁，而读或写 dirty 都需要加锁**
- 另外有 misses 字段来统计 read 被穿透的次数（被穿透指需要读 dirty 的情况），超过一定次数则将 dirty 数据同步到 read 上
- 对于删除数据则直接通过标记来延迟删除

```
type Map struct {
    // 加锁作用，保护 dirty 字段
    mu Mutex
    // 只读的数据，实际数据类型为 readOnly
    read atomic.Value
    // 最新写入的数据
    dirty map[interface{}]*entry
    // 计数器，每次需要读 dirty 则 +1
    misses int
}
```

- `Load`：读取指定 key 返回 value
- `Store`： 存储（增或改）key-value
- `Delete`： 删除指定 key

- `Range`：遍历所有键值对，参数是回调函数
- `LoadOrStore`：读取数据，若不存在则保存再读取

# 33、sync.Mutex和sync.RWMutex

mutex

```go
type Mutex struct {
    state int32
    sema  uint32
}
```

互斥即不可同时运行。即使用了互斥锁的两个代码片段互相排斥，只有其中一个代码片段执行完成后，另一个才能执行。

Go 标准库中提供了 sync.Mutex 互斥锁类型及其两个方法：

- Lock 加锁
- Unlock 释放锁

我们可以通过在代码前调用 Lock 方法，在代码后调用 Unlock 方法来保证一段代码的互斥执行，也可以用 defer 语句来保证互斥锁一定会被解锁。在一个 Go 协程调用 Lock 方法获得锁后，其他请求锁的协程都会阻塞在 Lock 方法，直到锁被释放。

> *互斥锁和解锁都需要在同一go程完成，不要跨go程解锁*

rwmutex

```go
type RWMutex struct {
	w           Mutex  //用于控制多个写锁，获得写锁首先要获取该锁，如果有一个写锁在进行，那么再到来的写锁将会阻塞于此
	writerSem   uint32 //写阻塞等待的信号量，最后一个读者释放锁时会释放信号量
	readerSem   uint32 //读阻塞的协程等待的信号量，持有写锁的协程释放锁后会释放信号量
	readerCount int32  //记录读者个数
	readerWait  int32  //记录写阻塞时读者个数
}
```

**RLock()实现逻辑**

- 增加读操作计数，即readerCount++
- 阻塞等待写操作结束(如果有的话)

**RUnlock()实现逻辑**

- 减少读操作计数，即readerCount--
- 唤醒等待写操作的协程（如果有的话）

# 拷贝大切片一定比小切片代价大吗

切片中的第一个字是指向切片底层数组的指针，这是切片的存储空间，第二个字段是切片的长度，第三个字段是容量。将一个 slice 变量分配给另一个变量只会复制三个机器字。所以 **拷贝大切片跟小切片的代价应该是一样的**。

```go
type SliceHeader struct {
 	Data uintptr
 	Len  int
 	Cap  int
}
```

# 35、pprof 性能分析

需要安装 Graphviz 

- runtime/pprof：采集程序（非 Server）的运行数据进行分析
- net/http/pprof：采集 HTTP Server 的运行时数据进行分析

可以做

- CPU Profiling：CPU 分析，按照一定的频率采集所监听的应用程序 CPU（含寄存器）的使用情况，可确定应用程序在主动消耗 CPU 周期时花费时间的位置
- Memory Profiling：内存分析，在应用程序进行堆分配时记录堆栈跟踪，用于监视当前和历史内存使用情况，以及检查内存泄漏
- Block Profiling：阻塞分析，记录 goroutine 阻塞等待同步（包括定时器通道）的位置
- Mutex Profiling：互斥锁分析，报告互斥锁的竞争情况

# gdb调试

\# 关闭内联优化，方便调试 

go build -gcflags "-N -l" demo.go

```javascript
gdb main
```

(gdb) info breakpoints // 查看所有断点。

(gdb) r // 启动进程，触发第一个断点。

(gdb) info goroutines // 查看 goroutines 信息。

(gdb) goroutine 1 bt // 查看指定序号的 goroutine 调用堆栈

(gdb) c / / 继续执行，触发下一个断点。

(gdb) bt // 查看当前调⽤堆栈，可以与当前 goroutine 调用堆栈对比。

(gdb) info frame // 堆栈帧信息。

(gdb) info locals // 查看局部变量。

(gdb) p s // 以 Pretty-Print 方式查看变量。

(gdb) clear //清除所有设置在函数上的断点。

(gdb) help all //可以看到所有的命令



