---
title: Scala第一讲（变量，函数，循环，类的定义，构造函数）
date: 2019-07-06 23:33:09
tags: scala
---
## 定义变量
var 可变变量  
val 不可变变量  

<!--more-->

Scala里变量类型是可以自动推导的  
val a = 10  
val b = 20  
val c = "aaa"  
val d = new User()  

Scala的基本数据类型  
Byte Char Short Int Long Float Double Boolean .....

asInstanceOf 类型转换  
```shell
scala> val a:Int = 10
a: Int = 10

scala> a.asInstanceOf[Double]
res7: Double = 10.0
```

isInstanceOf 判断类型
```shell
scala> val a:Double = 10d
a: Double = 10.0

scala> a.isInstanceOf[Int]
res8: Boolean = false
```

## 定义函数、方法
```scala
   def max(x: Int, y: Int): Int = {
    if (x > y) {
      x
    } else {
      y
    }
  }
```
def：定义函数的关键字  
max：定义函数的名称，通俗见名知意的  
(x: Int, y: Int)：函数的入参可以没有，多个入参之间用逗号分隔，每个参数定义（参数名称：参数类型）  
:Int：函数返回值类型，可简写Scala自动推导，无返回值为:Unit  
{...}：大括号之间的为方法体，方法体的最后一行代码为返回值

## 循环
1到10  
Range.Inclusive,闭区间的，有头有尾
```shell
scala> 1 to 10
res9: scala.collection.immutable.Range.Inclusive = Range(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)
```

Range(起始位，结束位，步长)  
until：跟步长为1的Range是一个意思  

```shell
scala> Range(1,10)
res10: scala.collection.immutable.Range = Range(1, 2, 3, 4, 5, 6, 7, 8, 9)

scala> Range(1,10,1)
res11: scala.collection.immutable.Range = Range(1, 2, 3, 4, 5, 6, 7, 8, 9)

scala> Range(10,1,-1)
res12: scala.collection.immutable.Range = Range(10, 9, 8, 7, 6, 5, 4, 3, 2)

scala> Range(1,10,0)
java.lang.IllegalArgumentException: step cannot be 0.
  at scala.collection.immutable.Range.<init>(Range.scala:86)
  at scala.collection.immutable.Range$.apply(Range.scala:439)
  ... 32 elided
  
scala> 1 until 10
res15: scala.collection.immutable.Range = Range(1, 2, 3, 4, 5, 6, 7, 8, 9)
```
for循环输出
```scala
for (i <- Range(1,10,1)){
      println(i)
}

//输出1-10之间的偶数
for (i <- Range(1,10,1) if(i%2 == 0)){
      println(i)
}
```
## 对象
_:表示占位符，初始化可变变量使用，使用占位符必须定义变量类型  
private[this]：设置访问修饰符，this代表只能在类的内部使用
```scala
class SparkConf {
  private[this] var master: String = _
  private[this] var appName: String = ""

  def setMaster(master: String): SparkConf = {
    this.master = master
    this
  }

  def setAppNmae(appName: String): SparkConf = {
    this.appName = appName
    this
  }

  def printInfo(): Unit = {
    println("Master:" + this.master + "appName:" + this.appName)
  }
}
```
Spark 源码版
```scala
package com.huangzh.bigdata.scala01

import java.util.concurrent.ConcurrentHashMap

object SparkConfApp {
  def main(args: Array[String]): Unit = {
    var sparkConf = new SparkConf().setMaster("yarn").setAppNmae("World Count")
    sparkConf.printInfo()
  }
}

class SparkConf {
  private[this] var setting = new ConcurrentHashMap[String,String]()

  def setMaster(master: String): SparkConf = {
    set("spark.master",master)
  }

  def setAppNmae(appName: String): SparkConf = {
    set("spark.app.name",appName)
  }

  private[this] def set(key:String,value:String): SparkConf ={
    this.setting.put(key,value)
    this
  }

  def printInfo(): Unit = {
    println("Master:" + this.setting.get("spark.master") + " AppName:" + this.setting.get("spark.app.name"))
  }
}

```

## 构造函数
主构造函数跟在Class的类名后面，入参的变量会在类中自动定义对象属性
附属构造函数的第一行必须调用主构造函数或其他附属构造函数
```scala
package com.huangzh.bigdata.scala01

object SparkConfApp {
  def main(args: Array[String]): Unit = {
    var sparkConf = new SparkConf("yarn", "WorldCount")
    sparkConf.printInfo()

    var sparkConf2 = new SparkConf("yarn", "WorldCount", "/home/jars")
    sparkConf2.printInfo()
  }
}

class SparkConf(master: String, appName: String) {
  var extra: String = _
  //附属构造函数
  def this(master: String, appName: String, extra: String) {
    this(master, appName)
    this.extra = extra
  }

  def printInfo(): Unit = {
    println("Master:" + this.master + " AppName:" + this.appName + " extra:" + this.extra)
  }
}

```



