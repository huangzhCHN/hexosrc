---
title: Scala第一讲（变量，函数，循环，类的定义，构造函数）
date: 2019-07-06 23:33:09
tags: scala
---
## 定义变量
var 可变变量  
val 不可变变量  

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

+ 默认参数和带名参数

```scala
 //参数可以设置默认值
 def max(x: Int = 1, y: Int = 2): Int = {
    if (x > y) {
      x
    } else {
      y
    }
 }
 //调用时可以指定参数名
 max(y=10,x=1)
```
+ 变长参数

```scala
 def sum(args:Int*)={
    var result = 0
    for(arg <- args){
      result+=arg
    }
    result
  }

```
调用函数时，必须传入单个参数，不能传入一个整数区间，例如：  
sum(1 to 10)&nbsp;&nbsp;&nbsp;✖  
sum(1 to 10:_*)&nbsp;&nbsp;&nbsp;✔  使用\_\*将他转换为参数序列

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
###### for循环输出
```scala
for (i <- Range(1,10,1)){
      println(i)
}

//输出1-10之间的偶数
for (i <- Range(1,10,1) if(i%2 == 0)){
      println(i)
}
```
Scala并没有提供break和continue语句来退出循环，若要使用  
1.使用Boolean类型控制
2.使用嵌套函数（递归函数），你可以从函数当中return
3.使用Breaks对象中的break方法：
```scala
package com.huangzh.bigdata.scala01

import scala.util.control.Breaks.{break, breakable}

object BreakUtil {
  def main(args: Array[String]): Unit = {
    //输出前0-5的数，结果0，1，2，3，4，5
    breakable {
      for (i <- 0 until 10) {
        println(i)
        if (i == 5) {
          break()
        }
      }
    }


    //跳过i=5的数，结果0，1，2，3，4，6，7，8，9
    for (i <- 0 until 10) {
      breakable {
        if (i == 5) {
          break()
        }
        println(i)
      }
    }
  }
}
```

###### 高级for循环
Scala中的for循环要比Java的功能丰富的多
+  你可以以变量 <- 表达式的形式定义多个生成器，例如：
```scala
for (i <- 1 to 3; j <- 1 to 3) {
      print(f"${10 * i + j}%3d")
}

//结果：
 11 12 13 21 22 23 31 32 33
```
+  每个生成器都可以带上守卫，例如：
```scala
for (i <- 1 to 3 if i > 1; j <- 1 to 3 if i != j) {
      print(f"${10 * i + j}%3d")
}

//结果：
 21 23 31 32
```
+  你可以使用任意多的定义，引入可以在循环中使用的变量：
```scala
for (i <- 1 to 3 ;from = 4 - i;j <- from to 3){
      print(f"${10 * i + j}%3d")
}

//结果：
 13 22 23 31 32 33
```
+  如果for循环的循环体以yield开始，则该循环会构造出一个集合，生成的集合与它第一个生成器是类型兼容的：
```scala
var a = for (i <- 1 to 10 ) yield i
println(a)
//结果：
 Vector(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)

var b = for (i <- "Hello") yield (i+1).toChar
println(b)
//结果：
Ifmmp
```


在Scala中循环的使用不会像其他语言那么频繁，通常我们会使用高阶函数来实现，在后续的章节中会详细描述


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

