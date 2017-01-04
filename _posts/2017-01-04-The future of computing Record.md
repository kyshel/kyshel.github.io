## 《代码的未来》Record

## Prediction
1. 摩尔定律已接近极限
2. 计算机价格并不会大幅降低
3. 计算机朝多CPU多核心方向发展，未来编程语言专注于如何充分利用CPU
4. RAM、HDD消失，大规模的缓存取而代之
5. C/S两端结构不断变化，S端由一个服务器变为多个服务器相连的云计算系统

## Program Language

### DSL Domain Specific Language 领域特定语言

DSL是为特定领域所专门设计的词汇和语法。举例子：

* 外部DSL
  * XML
  * JSON
  * YAML
  * SQL
  * Regular expression
* 内部DSL
  * Rake file

### 元编程

* Meta
* Lisp
  * Macro

### 内存管理
### 异常处理
### 闭包

## New Trend

* Go
* Dart
* CoffeScript
* Lua
 
编程语言及开发者的国籍

语　　言 |开　发　者 |国　　籍
-|-|-
Fortran |John Bacus |美国
C |Dennis Richie |美国
Pascal |Niklaus Wirth |瑞士
Simula |Kristen Nygaard |挪威
C++ |Bjorn Stroustrup |丹麦
ML |Robin Milner |英国
Java |James Gosling |加拿大
Smalltalk |Alan Kay |美国
Perl |Larry Wall |美国
Python |Guido van Rossum |荷兰
PHP |Rasmus Lerdof |丹麦
Ruby |松本行弘 |日本
Eiffel |Bertrand Meyer |法国
Erlang |Joe Armstrong |瑞典
Lua |Roberto Ierusalimschy |巴西




## Cloud Compute Era

### Communication between process

- Process: Running Program, Unique between others, No state save or thransfer, no share memory space
- Thread: Multi threads can running in one process, they can cooperate, share memory space

Unix-Like ways to communicate:
- Pipe
- Message
- Semaphore
- Memory Shared
- TCP Socket
- UDP Socket
- UNIX Domain Socket

Socket Coding with C & Ruby

## Data Save

数据存储方式经典的是Key-Value存储，就像关系型数据库。

### NoSQL

It's Not No need SQL, it's Not Only SQL.

- Key-Value DB 
- Document DB
  * CouchDB
  * MongoDB
- Object DB

### Memcached

数据库中的数据大多放在磁盘上，当对数据库进行查询时，操作开销很大，而memcached将查询结果缓存在内存中，这样可以高性能。


## Multi-Core

1. 作者认为摩尔定律已经快接近极限，CPU已经不再单纯提高频率，而是朝着多核、多线程、多流水线发展。
2. Pipe是20世纪中期出现的东西，然而多核CPU的出现却使Pipe更加高效。
3. 一味提高CPU计算能力并不能突破计算过程中的瓶颈，要找到瓶颈究竟发生在哪里。 利用多台计算机，组成分布式计算环境可以改善性能。
4. 提高效率的方法：
    * burden-less
      * 升配置 
      * 空间换时间  
        步骤分解，结果分步保存
      * 自动化  
        用脚本进行重复工作
    * urgent-first
    	把自己看成一个CPU，为事情分配Priority  

    * distribute
      * Split Task    
        人是单核的，多个人组起一个团队可以看成多核，任务可以分隔开进行并行处理
      * Communicate Cost  
        人与人之间交流要花时间，这种时间一般花费很大
      * Reliability  
        就像分布式环境一样，一台电脑发生故障的概率很低，但把很多台看成一个整体，这个整体出问题的概率会很大，这又扯到了概率方面的问题。
        人也一样，人多了，出现问题的可能性也会更高。
5. Node.js 事件驱动编程 

## 总结

《代码的未来》出版日期为2013年，距今过去了4年，内容还不算过时。总体内容写的比较泛，DSL、数据存储、提高工作效率几部分还不错，不过我总是感觉这本书在推销Ruby，可是用的着推销吗啊哈哈 ;>



