---
title: Scala��һ����������������ѭ������Ķ��壬���캯����
date: 2019-07-06 23:33:09
tags: scala
---
## �������
var �ɱ����  
val ���ɱ����  

Scala����������ǿ����Զ��Ƶ���  
val a = 10  
val b = 20  
val c = "aaa"  
val d = new User()  

Scala�Ļ�����������  
Byte Char Short Int Long Float Double Boolean .....
<!--more-->

asInstanceOf ����ת��  
```
scala> val a:Int = 10
a: Int = 10

scala> a.asInstanceOf[Double]
res7: Double = 10.0
``` 

isInstanceOf �ж�����
```
scala> val a:Double = 10d
a: Double = 10.0

scala> a.isInstanceOf[Int]
res8: Boolean = false
```

## ���庯��������
```
   def max(x: Int, y: Int): Int = {
    if (x > y) {
      x
    } else {
      y
    }
  }
```
def�����庯���Ĺؼ���  
max�����庯�������ƣ�ͨ�׼���֪���  
(x: Int, y: Int)����������ο���û�У�������֮���ö��ŷָ���ÿ���������壨�������ƣ��������ͣ�  
:Int����������ֵ���ͣ��ɼ�дScala�Զ��Ƶ����޷���ֵΪ:Unit  
{...}��������֮���Ϊ�����壬����������һ�д���Ϊ����ֵ

## ѭ��
1��10  
Range.Inclusive,������ģ���ͷ��β
```
scala> 1 to 10
res9: scala.collection.immutable.Range.Inclusive = Range(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)
```

Range(��ʼλ������λ������)  
until��������Ϊ1��Range��һ����˼  
```
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
forѭ�����
```
for (i <- Range(1,10,1)){
      println(i)
}

//���1-10֮���ż��
for (i <- Range(1,10,1) if(i%2 == 0)){
      println(i)
}
```
## ����
_:��ʾռλ������ʼ���ɱ����ʹ�ã�ʹ��ռλ�����붨���������  
private[this]�����÷������η���this����ֻ��������ڲ�ʹ��
```
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
Spark Դ���
```
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

## ���캯��
�����캯������Class���������棬��εı������������Զ������������
�������캯���ĵ�һ�б�����������캯���������������캯��
```
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
  //�������캯��
  def this(master: String, appName: String, extra: String) {
    this(master, appName)
    this.extra = extra
  }

  def printInfo(): Unit = {
    println("Master:" + this.master + " AppName:" + this.appName + " extra:" + this.extra)
  }
}

```





