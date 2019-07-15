---
title: Scala第二讲（继承，特质，包和引入）
date: 2019-07-15 23:00:07
tags: scala
---

## 继承
##### 使用extends关键字继承父类

<!--more-->

```scala
package com.huangzh.bigdata.scala01

object Scala02 {
  def main(args: Array[String]): Unit = {
    var person: Employee = new Employee
    person.name = "hzh"
    person.salary = 1000000.0
    print(person.name + " salary:" + person.salary)
  }
}

class Person {
  var salary = 0.0
}

class Employee extends Person {
  var name: String = _
}

```
##### 重写方法  

重写一个非抽象方法必须使用override修饰符  
调用超类的方法与Java一样使用super关键字  
```scala
package com.huangzh.bigdata.scala01

object Scala02 {
  def main(args: Array[String]): Unit = {
    var person: Employee = new Employee
    person.name = "hzh"
    person.salary = 1000000.0
    println(person.name + " salary:" + person.salary)
    person.showSalary
    println(person.toString)
  }
}

class Person {
  var salary = 0.0

  def showSalary: Unit = {
    println("My Salary:" + salary);
  }
}

class Employee extends Person {
  var name: String = _

  override def showSalary: Unit = {
    println("Secret!!!!")
  }

  override def toString: String = s"${super.toString}{name=$name}"
}

```
##### 类型转换和检查 

与类型检查和转换相比，模式匹配的功能更加强大，后期会详细说明  

| Scala                | Java              |
| -------------------- | ----------------- |
| obj.isInstanceOf[Cl] | obj instanceof Cl |
| obj.asInstanceOf[Cl] | (Cl) obj          |
| classOf[Cl]          | Cl.class          |

##### 受保护字段和方法

和Java一样可以使用protected，这样的成员可以被子类访问，但不能从其他位置看到
```scala
package com.huangzh.bigdata.scala01

object Scala02 {
  def main(args: Array[String]): Unit = {
    var person: Employee = new Employee
    person.name = "hzh"
    person.salary = 1000000.0
    println(person.name + " salary:" + person.salary)
    person.showSalary
    person.age //age无法被使用，编译报错
    println(person.toString)
  }
}

class Person {
  var salary = 0.0
  protected val age:Int = 0
  def showSalary: Unit = {
    println("My Salary:" + salary);
  }
}

class Employee extends Person {
  var name: String = _
  override protected val age: Int = 20
  override def showSalary: Unit = {
    println("Secret!!!!")
  }

  override def toString: String = s"${super.toString}{name=$name}"
}

```
##### 超类的构造  

前一讲说过所有附属构造器都必须调用主构造器或者其他附属构造器，而只有主构造器才能调用附属构造器
```scala
package com.huangzh.bigdata.scala01

object Scala02 {
  def main(args: Array[String]): Unit = {
    var person: Employee = new Employee(24,"zwx")
    person.name = "hzh"
    person.salary = 1000000.0
    println(person.name + " salary:" + person.salary)
    person.showSalary
    println(person.toString)
  }
}

class Person(age:Int) {
  var salary = 0.0

  def showSalary: Unit = {
    println("My Salary:" + salary);
  }
}

class Employee(age:Int) extends Person(age) {
  def this(age:Int,empName:String){
    this(age)
    name = "None"
    println("EmpName:"+empName+" Name:"+name+" Age:"+age)
  }
  var name: String = _
  override def showSalary: Unit = {
    println("Secret!!!!")
  }

  override def toString: String = s"${super.toString}{name=$name}"
}
```
##### 重写字段  

def只能重写另一个def  
val只能重写另一个val或者def  
var只能重写另一个抽象的var  
```scala
abstract class A {
  def id: Int
  def name: String
  var age: Int
}

class A1 extends A {
  override def id: Int = 1
  override val name: String = "huangzh"
  override var age: Int = 24
}
```
##### 匿名子类

Scala可以直接在new对象时使用代码块创建一个匿名子类
```scala
def main(args: Array[String]): Unit = {
    var person2 = new Person(24){
      val name:String = "hzh"
      def showName:Unit ={
        println(name);
      }
    }
    person2.showName
  }
```
##### 抽象类

和Java一样，使用abstract关键字标记不能被实例化的类  
抽象方法：没有具体实现  
抽象字段：没有具体实例  
实现抽象方法和抽象字段可以不使用override标识  
子类必须实现所有抽象方法和抽象字段

## 特质
未完待续...