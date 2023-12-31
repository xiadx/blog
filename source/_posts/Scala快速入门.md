---
title: Scala快速入门
date: 2019-09-13 08:00:00
tags: Scala
categories: Scala
---
Scala快速入门

## Scala官网

* [Scala官网](https://www.scala-lang.org/)
* [Scala安装指南](https://docs.scala-lang.org/getting-started-intellij-track/getting-started-with-scala-in-intellij.html)
* [Scala入门指南](https://docs.scala-lang.org/tour/tour-of-scala.html)

## Scala基础

```scala
package basic

object BasicDemo {

  def main(args: Array[String]): Unit = {
    println(1 + 1)
    println((1 + 1) * 2.0)
    println("hello".toCharArray)
    println(((1 + 1) * 2.0).toString)
    println("hello".toUpperCase)
    println("HELLO".toLowerCase)
  }

}

```

```
/* Output:
2
4.0
[C@11531931
4.0
HELLO
hello
*///:~

```

```scala
package basic

object BasicDemo {

  def main(args: Array[String]): Unit = {
    val r = 1 + 1
    println(2 * r)
    var a = 1; a = 2
    println(a)
    val n: String = null
    val m: Any = "leo"
    val n1, n2: String = null
    val m1, m2 = 100
    println(n)
    println(m)
    println(n1)
    println(n2)
    println(m1)
    println(m2)
  }

}

```

```
/* Output:
4
2
null
leo
null
null
100
100
*///:~

```

```scala
package basic

object BasicDemo {

  def main(args: Array[String]): Unit = {
    println(1.toString)
    println(1.to(10))
    println("Hello".intersect("World"))
    println(1 + 1)
    println(1.+(1))
    println(1 to 10)
    var counter = 1; counter += 1
    println(counter)
  }

}

```

```
/* Output:
1
Range(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)
lo
2
2
Range(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)
2
*///:~

```

```scala
package basic

object BasicDemo {

  def main(args: Array[String]): Unit = {
    import scala.math._
    println(sqrt(2))
    println(pow(2, 4))
    println(min(3, Pi))
    println("Hello World".distinct)
    println("Hello World"(6))
    println("Hello World".apply(6))
    println(Array(1, 2, 3, 4))
    println(Array.apply(1, 2, 3, 4))
  }

}

```

```
/* Output:
1.4142135623730951
16.0
3.0
Helo Wrd
W
W
[I@6442b0a6
[I@60f82f98
*///:~

```

```scala
package basic

object BasicDemo {

  def main(args: Array[String]): Unit = {
    var age = 30
    println(if (age > 18) 1 else 0)
    val isAdult1 = if (age > 18) 1 else 0
    println(isAdult1)
    var isAdult2 = -1
    if (age > 18) isAdult2 = 1 else isAdult2 = 0
    println(isAdult2)
    println(if (age > 18) "adult" else 0)
    age = 12
    println(if (age > 18) "adult")
    println(if (age > 18) "adult" else ())
    println(if (age > 18) "adult" else if (age > 12) "teenager" else "children")
  }

}

```

```
/* Output:
1
1
1
adult
()
()
children
*///:~

```

```scala
package basic

object BasicDemo {

  def main(args: Array[String]): Unit = {
    var x, y, z = 0
    if (x < 10) { y = y + 1; z = z + 1 }
    println(x, y, z)
    if (x < 10) {
      y = y + 1
      z = z + 1
    }
    println(x, y, z)
    var w = if (x < 10) { y = y + 1; z + 1 }
    println(x, y, z, w)
  }

}

```

```
/* Output:
(0,1,1)
(0,2,2)
(0,3,2,3)
*///:~

```

```scala
package basic

object BasicDemo {

  def main(args: Array[String]): Unit = {
    print("Hello World"); println("Hello World")
    printf("Hi, my name is %s, I'm %d years old.\n", "Leo", 30)
  }

}

```

```
/* Output:
Hello WorldHello World
Hi, my name is Leo, I'm 30 years old.
*///:~

```

```scala
package basic

object BasicDemo {

  def main(args: Array[String]): Unit = {
    import scala.io.StdIn
    val name = StdIn.readLine("Welcome to Game House. Please tell me your name: ")
    print("Thanks. Then please tell me your age: ")
    val age = StdIn.readInt()
    if (age > 18) {
      printf("Hi, %s, you are %d years old, so you are legal to come here!", name, age)
    } else {
      printf("Sorry, boy, %s, you are only %d years old. you are illegal to come here!", name, age)
    }
  }

}

```

```
/* Output:
Welcome to Game House. Please tell me your name: leo
Thanks. Then please tell me your age: 20
Hi, leo, you are 20 years old, so you are legal to come here!
*///:~

```

```scala
package basic

object BasicDemo {

  def main(args: Array[String]): Unit = {
    println(1 + "")
    println(1.toString + "")
    var n = 10
    while (n > 0) {
      print(n + " ")
      n -= 1
    }
    println()
    n = 5
    for (i <- 1 to n) print(i + " ")
    println()
    for (i <- 1 until n) print(i + " ")
    println()
    import scala.util.control.Breaks._
    breakable {
      var n = 10
      for (c <- "Hello World") {
        if (n == 5) break
        print(c)
        n -= 1
      }
    }
  }

}

```

```
/* Output:
1
1
10 9 8 7 6 5 4 3 2 1 
1 2 3 4 5 
1 2 3 4 
Hello
*///:~

```

```scala
package basic

object BasicDemo {

  def main(args: Array[String]): Unit = {
    for (i <- 1 to 9; j <- 1 to i) {
      print(j + " * " + i + " = " + i * j + " ")
      if (i == j) println()
    }
  }

}

```

```
/* Output:
1 * 1 = 1 
1 * 2 = 2 2 * 2 = 4 
1 * 3 = 3 2 * 3 = 6 3 * 3 = 9 
1 * 4 = 4 2 * 4 = 8 3 * 4 = 12 4 * 4 = 16 
1 * 5 = 5 2 * 5 = 10 3 * 5 = 15 4 * 5 = 20 5 * 5 = 25 
1 * 6 = 6 2 * 6 = 12 3 * 6 = 18 4 * 6 = 24 5 * 6 = 30 6 * 6 = 36 
1 * 7 = 7 2 * 7 = 14 3 * 7 = 21 4 * 7 = 28 5 * 7 = 35 6 * 7 = 42 7 * 7 = 49 
1 * 8 = 8 2 * 8 = 16 3 * 8 = 24 4 * 8 = 32 5 * 8 = 40 6 * 8 = 48 7 * 8 = 56 8 * 8 = 64 
1 * 9 = 9 2 * 9 = 18 3 * 9 = 27 4 * 9 = 36 5 * 9 = 45 6 * 9 = 54 7 * 9 = 63 8 * 9 = 72 9 * 9 = 81 
*///:~

```

```scala
package basic

object BasicDemo {

  def main(args: Array[String]): Unit = {
    for (i <- 1 to 10 if i % 2 == 0) print(i + " ")
    
    val v = for (i <- 1 to 10) yield i
    println(v)
    for (i <- v) {
      print(i + " ")
    }
  }

}

```

```
/* Output:
2 4 6 8 10 Vector(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)
1 2 3 4 5 6 7 8 9 10 
*///:~

```

## Scala函数

```scala
package basic

object FunctionDemo {

  def main(args: Array[String]): Unit = {
    def sayHello(name: String, age: Int) = {
      if (age > 18) { printf("Hi %s, you are a big boy\n", name); age }
      else { printf("Hi %s, you are a little boy\n", name); age }
    }
    sayHello("leo", 30)
  }

}

```

```
/* Output:
Hi leo, you are a big boy
*///:~

```

```scala
package basic

object FunctionDemo {

  def main(args: Array[String]): Unit = {
    def sayHello(name: String) = println("Hello, " + name)
    sayHello("Leo")
  }

}

```

```
/* Output:
Hello, Leo
*///:~

```

```scala
package basic

object FunctionDemo {

  def main(args: Array[String]): Unit = {
    def sum(n: Int) = {
      var sum = 0
      for (i <- 1 to n) sum += i
      sum
    }
    println(sum(100))
  }

}

```

```
/* Output:
5050
*///:~

```

```scala
package basic

object FunctionDemo {

  def main(args: Array[String]): Unit = {
    def fab(n: Int): Int = {
      if (n <= 1) 1
      else fab(n - 1) + fab(n - 2)
    }
    println(fab(5))
  }

}

```

```
/* Output:
8
*///:~

```

```scala
package basic

object FunctionDemo {

  def main(args: Array[String]): Unit = {
    def sayHello(firstName: String, middleName: String = "William", lastName: String = "Croft") = firstName + " " + middleName + " " + lastName
    println(sayHello("Leo"))
  }

}

```

```
/* Output:
Leo William Croft
*///:~

```

```scala
package basic

object FunctionDemo {

  def main(args: Array[String]): Unit = {
    def sayHello(firstName: String, middleName: String, lastName: String) = firstName + " " + middleName + " " + lastName
    println(sayHello(firstName = "Mick", lastName = "Nina", middleName = "Jack"))
    println(sayHello("Mick", middleName = "Jack", lastName = "Nina"))
  }

}

```

```
/* Output:
Mick Jack Nina
Mick Jack Nina
*///:~

```

```scala
package basic

object FunctionDemo {

  def main(args: Array[String]): Unit = {
    def sayHello(name: String, age: Int = 20): Unit = {
      println("Hello, " + name + ", your age is " + age)
    }
    sayHello("Leo")
  }

}

```

```
/* Output:
Hello, Leo, your age is 20
*///:~

```

```scala
package basic

object FunctionDemo {

  def main(args: Array[String]): Unit = {
    def sum(nums: Int*) = {
      var res = 0
      for (num <- nums) res += num
      res
    }
    println(sum(1, 2, 3, 4, 5))
  }

}

```

```
/* Output:
15
*///:~

```

```scala
package basic

object FunctionDemo {

  def main(args: Array[String]): Unit = {
    def sum(nums: Int*): Int = {
      if (nums.length == 0) 0
      else nums.head + sum(nums.tail: _*)
    }
    println(sum(1 to 5: _*))
  }

}

```

```
/* Output:
15
*///:~

```

```scala
package basic

object FunctionDemo {

  def main(args: Array[String]): Unit = {
    def sayHello1(name: String) = "Hello, " + name
    def sayHello2(name: String): Unit = {
      print("Hello, " + name)
    }
    println(sayHello1("Leo"))
    sayHello2("Leo")
  }

}

```

```
/* Output:
Hello, Leo
Hello, Leo
*///:~

```

```scala
package basic

object FunctionDemo {

  def main(args: Array[String]): Unit = {
    import scala.io.Source._
    // 即使文件不存在，也不会报错，只有第一个使用变量时会报错，证明了表达式计算的lazy特性。
    lazy val lines = fromFile("spark.txt").mkString
  }

}

```

```
/* Output:

*///:~

```

```scala
package basic

object FunctionDemo {

  def main(args: Array[String]): Unit = {
    import java.io._
    try {
      throw new IllegalArgumentException("x should not be negative")
    } catch {
      case _: IllegalArgumentException => println("Illegal Argument!")
    } finally {
      println("Release Resources!")
    }
    try {
      throw new IOException("User Defined Exception")
    } catch {
      case e1: IllegalArgumentException => println("Illegal Argument!")
      case e2: IOException => println("IO Exception!")
    }
  }

}

```

```
/* Output:
Illegal Argument!
Release Resources!
IO Exception!
*///:~

```

## Array、ArrayBuffer

### ArrayDemo

```scala
package basic

object ArrayDemo {

  def main(args: Array[String]): Unit = {
    val a = new Array[Int](10)
    println(a(0))
    a(0) = 1
    println(a)
    val b = Array("hello", "world")
    b(0) = "hi"
    println(b)
    val c = Array("leo", 30)
    println(c)

    val d = Array(1, 2, 3, 4, 5)
    for (i <- 0 until d.length) {
      print(d(i) + " ")
    }
    println()
    for (i <- 0 until (d.length, 2)) {
      print(d(i) + " ")
    }
    println()
    for (i <- (0 until d.length).reverse) {
      print(d(i) + " ")
    }
    println()
    for (i <- d) {
      print(i + " ")
    }
    println()
    val e = Array(4, 2, 3, 1, 5)
    val s = e.sum
    val m = e.max
    println("sum -> " + s)
    println("max -> " + m)
    scala.util.Sorting.quickSort(e)
    println(e.mkString(","))
    println(e.mkString("<", ",", ">"))
    println(e.toString)
  }

}

```

```
/* Output:
0
[I@11531931
[Ljava.lang.String;@5e025e70
[Ljava.lang.Object;@48140564
1 2 3 4 5 
1 3 5 
5 4 3 2 1 
1 2 3 4 5 
sum -> 15
max -> 5
1,2,3,4,5
<1,2,3,4,5>
[I@6b2fad11
*///:~

```

### ArrayBufferDemo 

```scala
package basic

object ArrayBufferDemo {

  def main(args: Array[String]): Unit = {
    import scala.collection.mutable.ArrayBuffer
    val b = ArrayBuffer[Int]()
    b += 1
    b += (2, 3, 4, 5)
    b ++= Array(6, 7, 8, 9, 10)
    println(b)
    b.trimEnd(5)
    println(b)
    b.insert(5, 6)
    println(b)
    b.insert(6, 7, 8, 9, 10)
    println(b)
    b.remove(1)
    println(b)
    b.remove(1, 3)
    println(b)
    val a = b.toArray
    println(a)
    println(a.toBuffer)
  }

}

```

```
/* Output:
ArrayBuffer(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)
ArrayBuffer(1, 2, 3, 4, 5)
ArrayBuffer(1, 2, 3, 4, 5, 6)
ArrayBuffer(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)
ArrayBuffer(1, 3, 4, 5, 6, 7, 8, 9, 10)
ArrayBuffer(1, 6, 7, 8, 9, 10)
[I@380fb434
ArrayBuffer(1, 6, 7, 8, 9, 10)
*///:~

```

```scala
package basic

object ArrayBufferDemo {

  def main(args: Array[String]): Unit = {
    import scala.collection.mutable.ArrayBuffer
    val a = Array(1, 2, 3, 4, 5)
    val b = for (e <- a) yield e * e
    println(b.mkString(","))
    val c = ArrayBuffer[Int]()
    c += (1, 2, 3, 4, 5)
    val d = for (e <- c) yield e * e
    println(d.mkString(","))
    val e = for (e <- c if e %2 == 0) yield e * e
    println(a.filter(_ % 2 == 0).map(2 * _).mkString(","))
    println(a.filter { _ % 2 == 0 } map { 2 * _ } mkString { "," })
  }

}

```

```
/* Output:
1,4,9,16,25
1,4,9,16,25
4,8
4,8
*///:~

```

```scala
package basic

object ArrayBufferDemo {

  def main(args: Array[String]): Unit = {
    import scala.collection.mutable.ArrayBuffer
    val a = ArrayBuffer[Int]()
     a += (1, 2, 3, 4, 5, -1, -3, -5, -9)
     var foundFirstNegative = false
     var arrayLength = a.length
     var index = 0
     while (index < arrayLength) {
       if (a(index) >= 0) {
         index += 1
       } else {
         if (!foundFirstNegative) { foundFirstNegative = true; index += 1 }
         else { a.remove(index); arrayLength -= 1 }
       }
     }
     println(a)
  }

}

```

```
/* Output:
ArrayBuffer(1, 2, 3, 4, 5, -1)
*///:~

```

```scala
package basic

object ArrayBufferDemo {

  def main(args: Array[String]): Unit = {
    import scala.collection.mutable.ArrayBuffer
    val a = ArrayBuffer[Int]()
    a += (1, 2, 3, 4, 5, -1, -3, -5, -9)
    var foundFirstNegative = false
    val keepIndexes = for (i <- 0 until a.length if !foundFirstNegative || a(i) >= 0) yield {
      if (a(i) < 0) foundFirstNegative = true
      i
    }
    for (i <- 0 until keepIndexes.length) { a(i) = a(keepIndexes(i)) }
    a.trimEnd(a.length - keepIndexes.length)
    println(a)
  }

}

```

```
/* Output:
ArrayBuffer(1, 2, 3, 4, 5, -1)
*///:~

```

## Map、Tuple

### MapDemo

```scala
package basic

object MapDemo {

  def main(args: Array[String]): Unit = {
    val ages1 = Map("Leo" -> 30, "Jen" -> 25, "Jack" -> 23)
    //ages1("Leo") = 31
    println(ages1)
    val ages2 = scala.collection.mutable.Map("Leo" -> 30, "Jen" -> 25, "Jack" -> 23)
    ages2("Leo") = 31
    println(ages2)
    val ages3 = Map(("Leo", 30), ("Jen", 25), ("Jack", 23))
    println(ages3)
    val ages4 = new scala.collection.mutable.HashMap[String, Int]
    val leoAge1 = ages1("Leo")
    val leoAge2 = if (ages1.contains("leo")) ages1("leo") else 0
    val leoAge3 = ages1.getOrElse("leo", 0)
    println(leoAge1, leoAge2, leoAge3)
    ages4("Leo") = 31
    ages4 += ("Mike" -> 35, "Tom" -> 40)
    ages4 -= "Mike"
    val ages5 = ages1 + ("Mike" -> 36, "Tom" -> 40)
    val ages6 = ages5 - "Tom"
    println(ages4)
    println(ages5)
    println(ages6)
    for ((key, value) <- ages1) println(key + " " + value)
    for (key <- ages1.keySet) println(key)
    for (value <- ages1.values) println(value)
    val ages7 = for ((key, value) <- ages1) yield (value, key)
    for ((k, v) <- ages7) println(k, v)
  }

}

```

```
/* Output:
Map(Leo -> 30, Jen -> 25, Jack -> 23)
Map(Jen -> 25, Jack -> 23, Leo -> 31)
Map(Leo -> 30, Jen -> 25, Jack -> 23)
(30,0,0)
Map(Tom -> 40, Leo -> 31)
Map(Mike -> 36, Tom -> 40, Leo -> 30, Jack -> 23, Jen -> 25)
Map(Mike -> 36, Leo -> 30, Jack -> 23, Jen -> 25)
Leo 30
Jen 25
Jack 23
Leo
Jen
Jack
30
25
23
(30,Leo)
(25,Jen)
(23,Jack)
*///:~

```

### TupleDemo

```scala
package basic

object TupleDemo {

  def main(args: Array[String]): Unit = {
    val t = ("Leo", 30)
    println(t._1, t._2)
    val names = Array("Leo", "Jack", "Mike")
    val ages = Array(30, 24, 26)
    val nameAges = names.zip(ages)
    for ((name, age) <- nameAges) println(name + " -> " + age)
  }

}

```

```
/* Output:
(Leo,30)
Leo -> 30
Jack -> 24
Mike -> 26
*///:~

```

## SortedMap、LinkedHashMap

### SortedMapDemo

```scala
package basic

object SortedMapDemo {

  def main(args: Array[String]): Unit = {
    val ages = scala.collection.immutable.SortedMap("Leo" -> 30, "Alice" -> 15, "Jen" -> 25)
    for ((k, v) <- ages) println(k, v)
  }

}

```

```
/* Output:
(Alice,15)
(Jen,25)
(Leo,30)
*///:~

```

### LinkedHashMapDemo

```scala
package basic

import scala.collection.mutable

object LinkedHashMapDemo {

  def main(args: Array[String]): Unit = {
    val ages = new mutable.LinkedHashMap[String, Int]
    ages("Leo") = 30
    ages("Alice") = 15
    ages("Jen") = 25
    for ((k, v) <- ages) println(k, v)
  }

}

```

```
/* Output:
(Leo,30)
(Alice,15)
(Jen,25)
*///:~

```

## Class

### ClassDemo

```scala
package basic

object ClassDemo {

  def main(args: Array[String]): Unit = {
    val hw = new HelloWorld
    hw.sayHello()
    println(hw.getName)
  }

  class HelloWorld {

    private var name = "Leo"

    def sayHello() { println("Hello, " + name) }

    def getName = name

  }

}

```

```
/* Output:
Hello, Leo
Leo
*///:~

```

### GetterSetterDemo

```scala
package basic

object GetterSetterDemo {

  def main(args: Array[String]): Unit = {
    val leo = new Student
    println(leo.name)
    leo.name = "leo1"
    println(leo.name)
  }

  class Student {

    var name = "Leo"

  }

}

```

```
/* Output:
Leo
leo1
*///:~

```

### CustomGetterSetterDemo

```scala
package basic

object CustomGetterSetterDemo {

  def main(args: Array[String]): Unit = {
    val leo = new Student
    println(leo.name)
    leo.name = "leo1"
    println(leo.name)
  }

  class Student {

    private var myName = "leo"

    def name = "your name is " + myName

    def name_=(newValue: String): Unit = {
      println("you cannot edit your name!")
    }

  }

}

```

```
/* Output:
your name is leo
you cannot edit your name!
your name is leo
*///:~

```

### FieldGetterDemo

```scala
package basic

object FieldGetterDemo {

  def main(args: Array[String]): Unit = {
    val leo = new Student
    println(leo.name)
    leo.updateName("jack")
    println(leo.name)
    leo.updateName("leo1")
    println(leo.name)
  }

  class Student {

    private var myName = "leo"

    def updateName(newName: String): Unit = {
      if (newName == "leo1") myName = newName
      else println("not accept this new name!")
    }

    def name = "your name is " + myName

  }

}

```

```
/* Output:
your name is leo
not accept this new name!
your name is leo
your name is leo1
*///:~

```

### PrivateThisDemo

```scala
package basic

object PrivateThisDemo {

  def main(args: Array[String]): Unit = {
    val leo = new Student
    leo.age = 30
    val jack = new Student
    jack.age = 18
    println(leo.older(jack))
  }

  class Student {

    private var myAge = 0
//      private[this] var myAge = 0

    def age_=(newValue: Int): Unit = {
      if (newValue > 0) myAge = newValue
      else print("illegal age!")
    }

    def age = myAge

    def older(s: Student) = {
      myAge > s.myAge
    }

  }

}

```

```
/* Output:
true
*///:~

```

### JavaGetterSetterDemo

```scala
package basic

import scala.beans.BeanProperty

object JavaGetterSetterDemo {

  def main(args: Array[String]): Unit = {
    val s = new Student
    s.setName("leo")
    println(s.getName)
    val t = new Teacher("jack")
    println(t.getName)
  }

  class Student {

    @BeanProperty var name: String = _

  }

  class Teacher(@BeanProperty var name: String)

}

```

```
/* Output:
leo
jack
*///:~

```

### AuxiliaryConstructorDemo

```scala
package basic

object AuxiliaryConstructorDemo {

  def main(args: Array[String]): Unit = {
    val s1 = new Student
    println(s1.getName)
    println(s1.getAge)
    val s2 = new Student("leo")
    println(s2.getName)
    println(s2.getAge)
    val s3 = new Student("leo", 30)
    println(s3.getName)
    println(s3.getAge)
  }

  class Student {

    private var name = ""
    private var age = 0

    def this(name: String) {
      this()
      this.name = name
    }

    def this(name: String, age: Int) {
      this(name)
      this.age = age
    }

    def getName = name

    def setName(newName: String): Unit = {
      this.name = newName
    }

    def getAge = age

    def setAge(newAge: Int): Unit = {
      this.age = age
    }

  }

}

```

```
/* Output:
0
leo
0
leo
30
*///:~

```

### MainConstructor

```scala
package basic

object MainConstructor {

  def main(args: Array[String]): Unit = {
    val s = new Student("leo", 30)
    val t = new Teacher
  }

  class Student(val name: String, val age: Int) {

    println("your name is " + name + ", your age is " + age)

  }

  class Teacher(val name: String = "jack", val age: Int = 35) {

    println("your name is " + name + ", your age is " + age)

  }

}

```

```
/* Output:
your name is leo, your age is 30
your name is jack, your age is 35
*///:~

```

### InnerClassDemo

```scala
package basic

import scala.collection.mutable.ArrayBuffer

object InnerClassDemo {

  def main(args: Array[String]): Unit = {
    val c1 = new Class
    val s1 = c1.getStudents("leo")
    c1.students += s1
    val c2 = new Class
    val s2 = c2.getStudents("leo")
//    c1.students += s2
    c2.students += s2
    println(c1.students)
    println(c2.students)
  }

  class Class {

    class Student(val name: String) {}

    val students = new ArrayBuffer[Student]

    def getStudents(name: String) = {
      new Student(name)
    }

  }

}

```

```
/* Output:
ArrayBuffer(basic.InnerClassDemo$Class$Student@7a79be86)
ArrayBuffer(basic.InnerClassDemo$Class$Student@34ce8af7)
*///:~

```

## Object

### ObjectDemo

```scala
package basic

object ObjectDemo {

  def main(args: Array[String]): Unit = {
    val p = Person
    println(p.getEyeNum)
    println(Person.getEyeNum)
  }

  object Person {

    private var eyeNum = 2

    println("this person object")

    def getEyeNum = eyeNum

  }

}

```

```
/* Output:
this person object
2
2
*///:~

```

### CompanionObjectDemo

```scala
package basic

object CompanionObjectDemo {

  def main(args: Array[String]): Unit = {
    val p = new Person("leo", 30)
    p.sayHello
  }

  object Person {

    private var eyeNum = 2

    println("this person object")

    def getEyeNum = eyeNum

  }

  class Person(val name: String, val age: Int) {

    def sayHello = println("Hi, " + name + ", I guess you are " +
      age + " years old!" + ", and usually you must have " + Person.eyeNum + " eyes.")

  }

}

```

```
/* Output:
this person object
Hi, leo, I guess you are 30 years old!, and usually you must have 2 eyes.
*///:~

```

### AbstractClassDemo

```scala
package basic

object AbstractClassDemo {

  def main(args: Array[String]): Unit = {
    val h = HelloImpl
    h.sayHello("world")
  }

  abstract class Hello(var message: String) {

    def sayHello(name: String): Unit

  }

  object HelloImpl extends Hello("hello") {

    override def sayHello(name: String) = {
      println(message + ", " + name)
    }

  }

}

```

```
/* Output:
hello, world
*///:~

```

### ApplyDemo

```scala
package basic

object ApplyDemo {

  def main(args: Array[String]): Unit = {
    val p = Person("leo")
    println(p.name)
  }

  class Person(val name: String)

  object Person {

    def apply(name: String) = new Person(name)

  }

}

```

```
/* Output:
leo
*///:~

```

### HelloWorldDemo

```scala
package basic

object HelloWorldDemo {

  def main(args: Array[String]): Unit = {
    println("Hello World")
  }

}

```

```
/* Output:
Hello World
*///:~

```

### HelloWorld

```scala
package basic

object HelloWorld extends App {

  println("Hello World")

}

```

```
/* Output:
Hello World
*///:~

```

### EnumerationDemo

```scala
package basic

object EnumerationDemoI {

  def main(args: Array[String]): Unit = {
    println(Season.SPRING)
    println(Season.values)
    for (e <- Season.values) println(e)
  }

  object Season extends Enumeration {

    val SPRING, SUMMER, AUTUMN, WINTER = Value

  }

}

```

```
/* Output:
SPRING
Season.ValueSet(SPRING, SUMMER, AUTUMN, WINTER)
SPRING
SUMMER
AUTUMN
WINTER
*///:~

```


```scala
package basic

object EnumerationDemoII {

  def main(args: Array[String]): Unit = {
    println(Season(0))
    println(Season.withName("spring"))
    for (e <- Season.values) println(e)
  }

  object Season extends Enumeration {

    val SPRING = Value(0, "spring")
    val SUMMER = Value(1, "summer")
    val AUTUMN = Value(2, "autumn")
    val WINTER = Value(3, "winter")

  }

}

```

```
/* Output:
spring
spring
spring
summer
autumn
winter
*///:~

```

## Extends

### ExtendsDemo

```scala
package basic

object ExtendsDemo {

  def main(args: Array[String]): Unit = {
    val s = new Student
    println(s.getScore)
    println(s.getName)
  }

  class Person {

    private var name = "leo"

    def getName = name

  }

  class Student extends Person {

    private var score = "A"

    def getScore = score

  }

}

```

```
/* Output:
A
leo
*///:~

```

### OverrideDemo

```scala
package basic

object OverrideDemo {

  def main(args: Array[String]): Unit = {
    val s = new Student
    println(s.getScore)
    println(s.getName)
  }

  class Person {

    private var name = "leo"

    def getName = name

  }

  class Student extends Person {

    private var score = "A"

    def getScore = score

    override def getName = "Hi, I'm " + super.getName

  }

}

```

```
/* Output:
A
Hi, I'm leo
*///:~

```

### OverrideFieldDemo

```scala
package basic

object OverrideFieldDemo {

  def main(args: Array[String]): Unit = {
    val s = new Student
    println(s.name)
    println(s.age)
  }

  class Person {

    val name: String = "person"

    def age: Int = 0

  }

  class Student extends Person {

    override val name: String = "leo"

    override def age: Int = 30

  }

}

```

```
/* Output:
leo
30
*///:~

```

### InstanceOfDemo

```scala
package basic

object InstanceOfDemo {

  def main(args: Array[String]): Unit = {
    val p: Person = new Student
    var s: Student = null
    println(s)
    if (p.isInstanceOf[Student]) s = p.asInstanceOf[Student]
    println(s)
  }

  class Person

  class Student extends Person

}

```

```
/* Output:
null
basic.InstanceOfDemo$Student@11531931
*///:~

```

### ClassOfDemo

```scala
package basic

object ClassOfDemo {

  def main(args: Array[String]): Unit = {
    val p: Person = new Student
    println(p.isInstanceOf[Person])
    println(p.getClass == classOf[Person])
    println(p.getClass == classOf[Student])
  }

  class Person

  class Student extends Person

}

```

```
/* Output:
true
false
true
*///:~

```

### ProtectedDemo

```scala
package basic

object ProtectedDemo {

  def main(args: Array[String]): Unit = {
    val s1 = new Student
    s1.sayHello
    val s2 = new Student
    s2.sayHello
    s1.makeFriends(s2)
  }

  class Person {

    protected var name: String = "leo"
    //  protected[this] var hobby: String = "game"
    protected var hobby: String = "game"

  }

  class Student extends Person {

    def sayHello = println("Hello, " + name)

    def makeFriends(s: Student): Unit = {
      println("my hobby is " + hobby + ", your hobby is " + s.hobby)
    }

  }

}

```

```
/* Output:
Hello, leo
Hello, leo
my hobby is game, your hobby is game
*///:~

```

### ParentConstructor

```scala
package basic

object ParentConstructor {

  def main(args: Array[String]): Unit = {
    val s1 = new Student("leo")
    println(s1.name, s1.age, s1.score)
    val s2 = new Student(30)
    println(s2.name, s2.age, s2.score)
  }

  class Person(val name: String, val age: Int)

  class Student(name: String, age: Int, var score: Double) extends Person(name, age) {

    def this(name: String) {
      this(name, 0, 0)
    }

    def this(age: Int) {
      this("leo", age, 0)
    }

  }

}

```

```
/* Output:
(leo,0,0.0)
(leo,30,0.0)
*///:~

```

### AnonymousInnerClassDemo

```scala
package basic

object AnonymousInnerClassDemo {

  def main(args: Array[String]): Unit = {
    val p = new Person("leo") {
      override def sayHello = "Hi, I'm " + name
    }
    greeting(p)
  }

  def greeting(p: Person {def sayHello: String}): Unit = {
    println(p.sayHello)
  }

  class Person(protected val name: String) {

    def sayHello = "Hello, I'm " + name

  }

}

```

```
/* Output:
Hi, I'm leo
*///:~

```

### AbstractClassDemo

```scala
package basic

object AbstractClassDemo {

  def main(args: Array[String]): Unit = {
    val h = HelloImpl
    h.sayHello("world")
  }

  abstract class Hello(var message: String) {

    def sayHello(name: String): Unit

  }

  object HelloImpl extends Hello("hello") {

    override def sayHello(name: String) = {
      println(message + ", " + name)
    }

  }

}

```

```
/* Output:
hello, world
*///:~

```

### AbstractFieldDemo

```scala
package basic

object AbstractFieldDemo {

  def main(args: Array[String]): Unit = {
    val s = new Student
    println(s.name)
  }

  abstract class Person {

    val name: String

  }

  class Student extends Person {

    val name: String = "leo"

  }

}

```

```
/* Output:
leo
*///:~

```

## Pattern Match

### PatternMatchDemo

```scala
package basic

object PatternMatchDemo {

  def main(args: Array[String]): Unit = {
    val p: Person = new Student
    p match {
      case per: Person => println("it's Person's object")
      case _ => println("unknown type")
    }
  }

  class Person

  class Student extends Person

}
```

```
/* Output:
it's Person's object
*///:~

```

```scala
package basic

object PatternMatchDemoI {

  def main(args: Array[String]): Unit = {
    def judgeGrade(grade: String): Unit = {
      grade match {
        case "A" => println("Excellent")
        case "B" => println("Good")
        case "C" => println("Just so so")
        case _ => println("You need work harder")
      }
    }
    judgeGrade("A")
    judgeGrade("D")
  }

}

```

```
/* Output:
Excellent
You need work harder
*///:~

```

```scala
package basic

object PatternMatchDemoII {

  def main(args: Array[String]): Unit = {
    def judgeGrade(name: String, grade: String): Unit = {
      grade match {
        case "A" => println(name + ", you are excellent")
        case "B" => println(name + ", you are good")
        case "C" => println(name + ", your are just so so")
        case _ if name == "leo" => println(name + ", you are a good boy, come on")
        case _ => println(name + ", you need to work harder")
      }
    }
    judgeGrade("jack", "A")
    judgeGrade("leo", "D")
    judgeGrade("jen", "D")
  }

}

```

```
/* Output:
jack, you are excellent
leo, you are a good boy, come on
jen, you need to work harder
*///:~

```

```scala
package basic

object PatternMatchDemoIII {

  def main(args: Array[String]): Unit = {
    import java.io._
    def processException(e: Exception): Unit = {
      e match {
        case e1: IllegalArgumentException => println("you have illegal arguments! exception is: " + e1)
        case e2: FileNotFoundException => println("cannot find the file you need read or write! exceptiotn is: " + e2)
        case e3: IOException => println("you got an error while you were doing IO operation! exception is: " + e3)
        case _: Exception => println("cannot know which exception you have!")
      }
    }
    processException(new IllegalArgumentException("illegal argument!"))
    processException(new IOException("io exception!"))
  }

}

```

```
/* Output:
you have illegal arguments! exception is: java.lang.IllegalArgumentException: illegal argument!
you got an error while you were doing IO operation! exception is: java.io.IOException: io exception!
*///:~

```

```scala
package basic

object PatternMatchDemoIV {

  def main(args: Array[String]): Unit = {
//    def greeting(arr: Array[String]): Unit = {
//      arr match {
//        case Array("Leo") => println("Hi, Leo!")
//        case Array(girl1, girl2, girl3) => println("Hi, girls, nice to meet you. " + girl1 + " and " + girl2 + " and " + girl3)
//        case Array("Leo", _*) => println("Hi, Leo, please introduce your friends to me.")
//        case _ => println("hey who are you?")
//      }
//    }
//    greeting(Array("Leo"))
//    greeting(Array("Jen", "Marry", "Penny"))
//    greeting(Array("Leo", "Jack"))
//    greeting(Array("Jack"))

    def greeting(list: List[String]): Unit = {
      list match {
        case "Leo" :: Nil => println("Hi, Leo!")
        case girl1 :: girl2 :: girl3 :: Nil => println("Hi, girls, nice to meet you. " + girl1 + " and " + girl2 + " and " + girl3)
        case "Leo" :: tail => println("Hi, Leo, please introduce your friends to me.")
        case _ => println("hey, who are you!")
      }
    }
    greeting(List("Leo"))
    greeting(List("Jen", "Marry", "Penny"))
    greeting(List("Leo", "Jack"))
    greeting(List("Jack"))
  }

}

```

```
/* Output:
Hi, Leo!
Hi, girls, nice to meet you. Jen and Marry and Penny
Hi, Leo, please introduce your friends to me.
hey, who are you!
*///:~

```

```scala
package basic

object PatternMatchDemoV {

  def main(args: Array[String]): Unit = {
    class Person
    case class Teacher(name: String, subject: String) extends Person
    case class Student(name: String, classroom: String) extends Person
    def judgeIdentify(p: Person): Unit = {
      p match {
        case Teacher(name, subject) => println("Teacher, name is " + name + ", subject is " + subject)
        case Student(name, classroom) => println("Student, name is " + name + ", classroom is " + classroom)
        case _ => println("Illegal access, please go out of the school!")
      }
    }
    judgeIdentify(new Teacher("Leo", "mathematics"))
    judgeIdentify(new Student("Jack", "class"))
    judgeIdentify(new Person)
  }

}

```

```
/* Output:
Teacher, name is Leo, subject is mathematics
Student, name is Jack, classroom is class
Illegal access, please go out of the school!
*///:~

```

```scala
package basic

object PatternMatchDemoVI {

  def main(args: Array[String]): Unit = {
    val grades = Map("Leo" -> "A", "Jack" -> "B", "Jen" -> "C")
    def getGrade(name: String): Unit = {
      val grade = grades.get(name)
      grade match {
        case Some(grade) => println("your grade is " + grade)
        case None => println("sorry, your grade information is not in the system")
      }
    }
    getGrade("Leo")
    getGrade("Marry")
  }

}

```

```
/* Output:
your grade is A
sorry, your grade information is not in the system
*///:~

```

## Trait

### TraitDemo

```scala
package basic

object TraitDemoI {

  def main(args: Array[String]): Unit = {
    val p1 = new Person("leo")
    val p2 = new Person("jack")
    p1.sayHello("jack")
    p2.sayHello("leo")
    p1.makeFriends(p2)
  }

  trait HelloTrait {

    def sayHello(name: String)

  }

  trait MakeFriendsTrait {

    def makeFriends(p: Person)

  }

  class Person(val name: String) extends HelloTrait with MakeFriendsTrait {

    def sayHello(otherName: String) = println("Hello, " + otherName + ", I'm " + name)

    def makeFriends(p: Person) = println("Hello " + p.name + ", I'm " + name + ", I want to make friends with you.")

  }

}

```

```
/* Output:
Hello, jack, I'm leo
Hello, leo, I'm jack
Hello jack, I'm leo, I want to make friends with you.
*///:~

```

```scala
package basic

object TraitDemoII {

  def main(args: Array[String]): Unit = {
    val p1 = new Person("leo")
    val p2 = new Person("jack")
    p1.makeFriends(p2)
  }

  trait Logger {

    def log(message: String) = println(message)

  }

  class Person(val name: String) extends Logger {

    def makeFriends(p: Person): Unit = {
      println("Hi, I'm " + name + ", I'm glad to make friends with you, " + p.name)
      log("makeFriends method is invoked with parameter Person[name=" + p.name + "]")
    }

  }

}

```

```
/* Output:
Hi, I'm leo, I'm glad to make friends with you, jack
makeFriends method is invoked with parameter Person[name=jack]
*///:~

```

```scala
package basic

object TraitDemoIII {

  def main(args: Array[String]): Unit = {
    val s = new Student("leo")
    s.sayHello
  }

  trait Person {

    val eyeNum: Int = 2

  }

  class Student(val name: String) extends Person {

    def sayHello = println("Hi, I'm " + name + ", I have " + eyeNum + " eyes.")

  }

}

```

```
/* Output:
Hi, I'm leo, I have 2 eyes.
*///:~

```

```scala
package basic

object TraitDemoIV {

  def main(args: Array[String]): Unit = {
    val p1 = new Person("leo")
    val p2 = new Person("jack")
    p1.makeFriends(p2)
  }

  trait SayHello {

    val msg: String

    def sayHello(name: String) = println(msg + ", " + name)

  }

  class Person(val name: String) extends SayHello {

    val msg: String = "hello"

    def makeFriends(p: Person): Unit = {
      sayHello(p.name)
      println("I'm " + name + ", I want to make friends with you!")
    }

  }

}

```

```
/* Output:
hello, jack
I'm leo, I want to make friends with you!
*///:~

```

```scala
package basic

object TraitDemoIX {

  def main(args: Array[String]): Unit = {
    val s = new Student
    println(s.getClass)
  }

  class Person { println("Person's constructor!") }

  trait Logger { println("Logger's constructor!") }

  trait MyLogger extends Logger { println("MyLogger's constructor!") }

  trait TimeLogger extends Logger { println("TimeLogger's constructor!") }

  class Student extends Person with MyLogger with TimeLogger {

    println("Student's constructor!")

  }

}

```

```
/* Output:
Person's constructor!
Logger's constructor!
MyLogger's constructor!
TimeLogger's constructor!
Student's constructor!
class basic.TraitDemoIX$Student
*///:~

```

```scala
package basic

object TraitDemoIV {

  def main(args: Array[String]): Unit = {
    val p1 = new Person("leo")
    val p2 = new Person("jack")
    p1.makeFriends(p2)
  }

  trait SayHello {

    val msg: String

    def sayHello(name: String) = println(msg + ", " + name)

  }

  class Person(val name: String) extends SayHello {

    val msg: String = "hello"

    def makeFriends(p: Person): Unit = {
      sayHello(p.name)
      println("I'm " + name + ", I want to make friends with you!")
    }

  }

}

```

```
/* Output:
hello, jack
I'm leo, I want to make friends with you!
*///:~

```

```scala
package basic

object TraitDemoIX {

  def main(args: Array[String]): Unit = {
    val s = new Student
    println(s.getClass)
  }

  class Person { println("Person's constructor!") }

  trait Logger { println("Logger's constructor!") }

  trait MyLogger extends Logger { println("MyLogger's constructor!") }

  trait TimeLogger extends Logger { println("TimeLogger's constructor!") }

  class Student extends Person with MyLogger with TimeLogger {

    println("Student's constructor!")

  }

}

```

```
/* Output:
Person's constructor!
Logger's constructor!
MyLogger's constructor!
TimeLogger's constructor!
Student's constructor!
class basic.TraitDemoIX$Student
*///:~

```

```scala
package basic

object TraitDemoV {

  def main(args: Array[String]): Unit = {
    val p1 = new Person("leo")
    p1.sayHello
    val p2 = new Person("jack") with MyLogger
    p2.sayHello
  }

  trait Logged {

    def log(msg: String) {}

  }

  trait MyLogger extends Logged {

    override def log(msg: String) { println("log: " + msg) }

  }

  class Person(val name: String) extends Logged {

    def sayHello { println("Hi, I'm " + name); log("sayHello is invoked!") }

  }

}

```

```
/* Output:
Hi, I'm leo
Hi, I'm jack
log: sayHello is invoked!
*///:~

```

```scala
package basic

object TraitDemoVI {

  def main(args: Array[String]): Unit = {
    val p = new Person("leo")
    p.sayHello
  }

  trait Handler {

    def handle(data: String) {}

  }

  trait DataValidHandler extends Handler {

    override def handle(data: String): Unit = {
      println("check data: " + data)
      super.handle(data)
    }

  }

  trait SignatureValidHandler extends Handler {

    override def handle(data: String): Unit = {
      println("check signature: " + data)
      super.handle(data)
    }

  }

  class Person(val name: String) extends SignatureValidHandler with DataValidHandler {

    def sayHello = { println("Hello, " + name); handle(name) }

  }

}

```

```
/* Output:
Hello, leo
check data: leo
check signature: leo
*///:~

```

```scala
package basic

object TraitDemoVII {

  def main(args: Array[String]): Unit = {
    println(classOf[Logger])
    println(classOf[MyLogger])
  }

  trait Logger {

    def log(msg: String)

  }

  trait MyLogger extends Logger {

    abstract override def log(msg: String) { super.log(msg) }

  }

}

```

```
/* Output:
interface basic.TraitDemoVII$Logger
interface basic.TraitDemoVII$MyLogger
*///:~

```

```scala
package basic

object TraitDemoVIII {

  def main(args: Array[String]): Unit = {
    val p1 = new Person("leo")
    val p2 = new Person("jack")
    println(p1.getClass)
    println(p2.getClass)
  }

  trait Valid {

    def getName: String

    def valid: Boolean = {
      getName == "leo"
    }

  }

  class Person(val name: String) extends Valid {

    println(valid)

    def getName = name

  }

}

```

```
/* Output:
true
false
class basic.TraitDemoVIII$Person
class basic.TraitDemoVIII$Person
*///:~

```

```scala
package basic

object TraitDemoX {

  def main(args: Array[String]): Unit = {
    class Person1
    val p1 = new {
      val msg: String = "init"
    } with Person1 with SayHello
    class Person2 extends {
      val msg: String = "init"
    } with SayHello
    val p2 = new Person2
    println(p1.getClass)
    println(p2.getClass)
  }

  trait SayHello {

    val msg: String
    println(msg.toString)

  }

}

```

```
/* Output:
init
init
class basic.TraitDemoX$$anon$1
class basic.TraitDemoX$Person2$1
*///:~

```

```scala
package basic

object TraitDemoXI {

  def main(args: Array[String]): Unit = {
    val p = new Person
    println(p.getClass)
  }

  trait SayHello {

    lazy val msg: String = null
    println(msg.toString)

  }

  class Person extends SayHello {

    override lazy val msg: String = "init"

  }

}

```

```
/* Output:
init
class basic.TraitDemoXI$Person
*///:~

```

```scala
package basic

object TraitDemoXII {

  def main(args: Array[String]): Unit = {
    val p = new Person("leo")
    p.sayHello
  }

  class MyUtil {

    def printMessage(msg: String) = println(msg)

  }

  trait Logger extends MyUtil {

    def log(msg: String) = printMessage("log: " + msg)

  }

  class Person(val name: String) extends Logger {

    def sayHello: Unit = {
      log("Hi, I'm " + name)
      printMessage("Hi, I'm " + name)
    }

  }

}

```

```
/* Output:
log: Hi, I'm leo
Hi, I'm leo
*///:~

```

## Functional Program

### FunctionalProgramDemo

```scala
package basic

object FunctionalProgramDemo {

  def main(args: Array[String]): Unit = {
    val l1 = List("Leo", "Jen", "Peter", "Jack").map("name is " + _)
    val l2 = List("Hello World", "You Me").flatMap(_.split(" "))
    val l3 = List("I", "have", "a", "beautiful", "house")
    val l4 =  List("Leo", "Jen", "Peter", "Jack").zip(List(100, 90, 75, 83))
    println(l1)
    println(l2)
    println(l3)
    println(l4)
  }

}

```

```
/* Output:
List(name is Leo, name is Jen, name is Peter, name is Jack)
List(Hello, World, You, Me)
List(I, have, a, beautiful, house)
List((Leo,100), (Jen,90), (Peter,75), (Jack,83))
*///:~

```

```scala
package basic

object FunctionalProgramDemoI {

  def main(args: Array[String]): Unit = {
    val line1 = scala.io.Source.fromFile("README.md").mkString
    val line2 = scala.io.Source.fromFile("README.md").mkString
    val lines = List(line1, line2)
    val count = lines.flatMap(_.split("\n")).flatMap(_.split(" ")).map((_, 1)).map(_._2).reduceLeft(_ + _)
    println(count)
  }

}

```

```
/* Output:
16
*///:~

```

### FunctionAssignDemo

```scala
package basic

object FunctionAssignDemo {

  def main(args: Array[String]): Unit = {
    val sayHelloFunc = sayHello _
    sayHelloFunc("leo")
  }

  def sayHello(name: String) { println("Hello, " + name) }

}

```

```
/* Output:
Hello, leo
*///:~

```

### AnonymousFunctionDemo

```scala
package basic

object AnonymousFunctionDemo {

  def main(args: Array[String]): Unit = {
    val sayHelloFunc = (name: String) => println("Hello, " + name)
    sayHelloFunc("Leo")
  }

}

```

```
/* Output:
Hello, Leo
*///:~

```

### HigherOrderFunctionDemo

```scala
package basic

object HigherOrderFunctionDemoI {

  def main(args: Array[String]): Unit = {
    val sayHelloFunc = (name: String) => println("Hello, " + name)
    def greeting(func: (String) => Unit, name: String) { func(name) }
    greeting(sayHelloFunc, "Leo")

    println(Array(1, 2, 3, 4, 5).map((num: Int) => num * num).mkString(","))

    def getGreetingFunc(msg: String) = (name: String) => println(msg + ", " + name)
    val greetingFunc = getGreetingFunc("hello")
    greetingFunc("leo")
  }

}

```

```
/* Output:
Hello, Leo
1,4,9,16,25
hello, leo
*///:~

```

```scala
package basic

object HigherOrderFunctionDemoII {

  def main(args: Array[String]): Unit = {
    def greeting(func: (String) => Unit, name: String) { func(name) }
    greeting((name: String) => println("Hello, " + name), "Leo")
    greeting((name) => println("Hello, " + name), "Leo")
    greeting(name => println("Hello, " + name), "Leo")

    def triple(func: (Int) => Int) = { func(3) }
    println(triple(3 * _))
  }

}

```

```
/* Output:
Hello, Leo
Hello, Leo
Hello, Leo
9
*///:~

```

```scala
package basic

object HigherOrderFunctionDemoIII {

  def main(args: Array[String]): Unit = {
    println(Array(1, 2, 3, 4, 5).map(2 * _).mkString(","))
    (1 to 9).map("*" * _).foreach(println _)
    println((1 to 20).filter(_ % 2 == 0).mkString(","))
    println((1 to 5).reduceLeft(_ * _))
    println(Array(3, 2, 5, 4, 10, 1).sortWith(_ < _).mkString(","))
  }

}

```

```
/* Output:
2,4,6,8,10
*
**
***
****
*****
******
*******
********
*********
2,4,6,8,10,12,14,16,18,20
120
1,2,3,4,5,10
*///:~

```

### ClosureDemo

```scala
package basic

object ClosureDemo {

  def main(args: Array[String]): Unit = {
    def getGreetingFunc(msg: String) = (name: String) => println(msg + ", " + name)
    val greetingFuncHello = getGreetingFunc("hello")
    val greetingFuncHi = getGreetingFunc("hi")
    greetingFuncHello("leo")
    greetingFuncHi("leo")
  }

}

```

```
/* Output:
hello, leo
hi, leo
*///:~

```

### SAMConvertDemo

```scala
package basic

import java.awt.event._
import javax.swing._

object SAMConvertDemo extends App {

//  val f = new JFrame("Hello World")
//  val b = new JButton("Click Me")
//  b.setBounds(100, 100, 65, 30)
//  b.addActionListener(new ActionListener {
//    override def actionPerformed(event: ActionEvent): Unit = {
//      println("Click Me")
//    }
//  })
//  f.add(b)
//  f.setSize(300, 300)
//  f.setVisible(true)

  val f = new JFrame("Hello World")
  val b = new JButton("Click Me")
  b.setBounds(100, 100, 65, 30)
  implicit def getActionListener(actionProcessFunc: (ActionEvent) => Unit) = new ActionListener {
    override def actionPerformed(event: ActionEvent): Unit = {
      actionProcessFunc(event)
    }
  }
  b.addActionListener((event: ActionEvent) => println("Click Me"))
  f.add(b)
  f.setSize(300, 300)
  f.setVisible(true)

}

```

```
/* Output:
Click Me
*///:~

```

### CurryingDemo

```scala
package basic

object CurryingDemo {

  def main(args: Array[String]): Unit = {
    def sum1(a: Int, b: Int) = a + b
    println(sum1(1, 1))
    def sum2(a: Int) = (b: Int) => a + b
    println(sum2(1)(1))
    def sum3(a: Int)(b: Int) = a + b
    println(sum3(1)(1))
  }

}

```

```
/* Output:
2
2
2
*///:~

```

### ReturnDemo

```scala
package basic

object ReturnDemo {

  def main(args: Array[String]): Unit = {
    def greeting(name: String) = {
      def sayHello(name: String): String = {
        return "Hello, " + name
      }
      sayHello(name)
    }
    println(greeting("Leo"))
  }

}

```

```
/* Output:
Hello, Leo
*///:~

```

## Container

### ListDemo

```scala
package basic

object ListDemo {

  def main(args: Array[String]): Unit = {
    def decorator(l: List[Int], prefix: String): Unit = {
      if (l != Nil) {
        print(prefix + l.head + " ")
        decorator(l.tail, prefix)
      }
    }
    val l = List(1, 2, 3, 4, 5)
    decorator(l, "#")
  }

}

```

```
/* Output:
#1 #2 #3 #4 #5 
*///:~

```

### LinkedListDemo

```scala
package basic

object LinkedListDemo {

  def main(args: Array[String]): Unit = {
//    val l = scala.collection.mutable.LinkedList(1, 2, 3, 4, 5)
//    var current = l
//    while (current != Nil) {
//      current.elem = current.elem * 2
//      current = current.next
//    }
//    println(l.mkString(","))

    val l = scala.collection.mutable.LinkedList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)
    var current = l
    var first = true
    while (current != Nil && current.next != Nil) {
      if (first) { current.elem = current.elem * 2; first = false }
      current = current.next.next
      if (current != Nil) current.elem = current.elem * 2
    }
    println(l.mkString(","))
  }

}

```

```
/* Output:
2,2,6,4,10,6,14,8,18,10
*///:~

```

### SetDemo

```scala
package basic

object SetDemo {

  def main(args: Array[String]): Unit = {
    val s1 = new scala.collection.mutable.HashSet[Int]()
    s1 += 1; s1 += 2; s1 += 5
    val s2 = new scala.collection.mutable.LinkedHashSet[Int]()
    s2 += 1; s2 += 2; s2 += 5
    val s3 = scala.collection.mutable.SortedSet[String]("banana", "apple", "candy")
    println(s1)
    println(s2)
    println(s3)
  }

}

```

```
/* Output:
Set(1, 5, 2)
Set(1, 2, 5)
TreeSet(apple, banana, candy)
*///:~

```

## Generic

### GenericDemo

```scala
package basic

object GenericDemo {

  def main(args: Array[String]): Unit = {
    class Student[T](val localId: T) {
      def getSchoolId(hukouId: T) = "S-" + hukouId + "-" + localId
    }
    val leo = new Student[Int](111)
    println(leo.getSchoolId(100))

    def getCard[T](content: T) = {
      if (content.isInstanceOf[Int]) "card: 001, " + content
      else if (content.isInstanceOf[String]) "card: this is your card, " + content
      else "card: " + content
    }
    println(getCard[String]("hello world"))
  }

}

```

```
/* Output:
S-100-111
card: this is your card, hello world
*///:~

```

### BoundsDemo

```scala
package basic

object BoundsDemoI {

  def main(args: Array[String]): Unit = {
    class Person(val name: String) {
      def sayHello = println("Hello, I'm " + name)
      def makeFriends(p: Person): Unit = {
        sayHello
        p.sayHello
      }
    }
    class Student(name: String) extends Person(name)
    class Party[T <: Person](p1: T, p2: T) {
      def play = p1.makeFriends(p2)
    }
    val s1 = new Student("leo")
    val s2 = new Student("jack")
    val p = new Party[Student](s1, s2)
    p.play
  }

}

```

```
/* Output:
Hello, I'm leo
Hello, I'm jack
*///:~

```

```scala
package basic

object BoundsDemoII {

  def main(args: Array[String]): Unit = {
    class Father(val name: String)
    class Child(name: String) extends Father(name)
    def getIDCard[R >: Child](person: R): Unit = {
      if (person.getClass == classOf[Child]) println("please tell us your parents' names.")
      else if (person.getClass == classOf[Father]) println("sign your name for your child's id card.")
      else println("sorry, you are not allowed to get id card.")
    }
    getIDCard[Child](new Child("leo"))
    getIDCard[Father](new Father("jack"))
  }

}

```

```
/* Output:
please tell us your parents' names.
sign your name for your child's id card.
*///:~

```

```scala
package basic

object BoundsDemoIII {

  def main(args: Array[String]): Unit = {
    class Person(val name: String) {
      def sayHello = println("Hello, I'm " + name)
      def makeFriends(p: Person): Unit = {
        sayHello
        p.sayHello
      }
    }
    class Student(name: String) extends Person(name)
    class Dog(val name: String) { def sayHello = println("Wang Wang, I'm " + name) }
    implicit def dog2person(dog: Object): Person = if (dog.isInstanceOf[Dog]) { val _dog = dog.asInstanceOf[Dog]; new Person(_dog.name ) } else null
    class Party[T <% Person](p1: T, p2: T) {
      def play = p1.makeFriends(p2)
    }
    val s = new Student("leo")
    val d = new Dog("jack")
    val p = new Party[Person](s, d)
    p.play
  }

}

```

```
/* Output:
Hello, I'm leo
Hello, I'm jack
*///:~

```

```scala
package basic

object BoundsDemoIV {

  def main(args: Array[String]): Unit = {
    class Calculator[T: Ordering](val number1: T, val number2: T) {
      def max(implicit order: Ordering[T]) = if (order.compare(number1, number2) > 0) number1 else number2
    }
    val c = new Calculator[Int](1, 2)
    println(c.max)
  }

}

```

```
/* Output:
2
*///:~

```

```scala
package basic

object BoundsDemoV {

  def main(args: Array[String]): Unit = {
    class Meat(val name: String)
    class Vegetable(val name: String)
    def packageFood[T: Manifest](food: T*) = {
      val foodPackage = new Array[T](food.length)
      for (i <- 0 until food.length) foodPackage(i) = food(i)
      foodPackage
    }
    val gongbaojiding = new Meat("gongbaojiding")
    val yuxiangrousi = new Meat("yuxiangrousi")
    val shousiyangpai = new Meat("shousiyangpai")
    val meatPackage = packageFood(gongbaojiding, yuxiangrousi, shousiyangpai)
    val qingcai = new Vegetable("qingcai")
    val baicai = new Vegetable("baicai")
    val huanggua = new Vegetable("huanggua")
    val vegetablePackage = packageFood(qingcai, baicai, huanggua)
    println(meatPackage)
    println(vegetablePackage)
  }

}

```

```
/* Output:
[Lbasic.BoundsDemoV$Meat$1;@e580929
[Lbasic.BoundsDemoV$Vegetable$1;@1cd072a9
*///:~

```

### ContravariantDemo

```scala
package basic

object ContravariantDemo {

  def main(args: Array[String]): Unit = {
    class Master
    class Professional extends Master
    class Card[-T](val name: String)
    def enterMeet(card: Card[Professional]): Unit = {
      println("welcom to have this meeting!")
    }
    enterMeet(new Card[Professional]("leo"))
    enterMeet(new Card[Master]("jack"))
  }

}

```

```
/* Output:
welcom to have this meeting!
welcom to have this meeting!
*///:~

```

```scala
package basic

object CovariantDemo {

  def main(args: Array[String]): Unit = {
    class Master
    class Professional extends Master
    class Card[+T](val name: String)
    def enterMeet(card: Card[Master]): Unit = {
      println("welcom to have this meeting!")
    }
    enterMeet(new Card[Master]("leo"))
    enterMeet(new Card[Professional]("jack"))
  }

}

```

```
/* Output:
welcom to have this meeting!
welcom to have this meeting!
*///:~

```

## Implicit Conversion

### ImplicitConversionDemo

```scala
package basic

object ImplicitConversionDemoI {

  def main(args: Array[String]): Unit = {
    class SpecialPerson(val name: String)
    class Student(val name: String)
    class Older(val name: String)
    class Teacher(val name: String)
    implicit def object2SpecialPerson(obj: Object): SpecialPerson = {
      if (obj.getClass == classOf[Student]) { val stu = obj.asInstanceOf[Student]; new SpecialPerson(stu.name) }
      else if (obj.getClass == classOf[Older]) { val older = obj.asInstanceOf[Older]; new SpecialPerson(older.name) }
      else null
    }
    var ticketNumber = 0
    def buySpecialTicket(p: SpecialPerson) = {
      ticketNumber += 1
      "T-" + ticketNumber
    }
    val s = new Student("leo")
    println(buySpecialTicket(s))
    val o = new Older("jack")
    println(buySpecialTicket(o))
    val t = new Teacher("tom")
    println(buySpecialTicket(t))
  }

}

```

```
/* Output:
T-1
T-2
T-3
*///:~

```

```scala
package basic

object ImplicitConversionDemoII {

  def main(args: Array[String]): Unit = {
    class Man(val name: String)
    class Superman(val name: String) {
      def emitLaser = println("emit a laster!")
    }
    implicit def man2supername(man: Man): Superman = new Superman(man.name)
    val leo = new Man("leo")
    leo.emitLaser
  }

}

```

```
/* Output:
emit a laster!
*///:~

```

```scala
package basic

object ImplicitConversionDemoIII {

  def main(args: Array[String]): Unit = {
    class SpecialPerson(val name: String)
    class Student(val name: String)
    class Older(val name: String)
    class Teacher(val name: String)
    implicit def object2SpecialPerson(obj: Object): SpecialPerson = {
      if (obj.getClass == classOf[Student]) { val stu = obj.asInstanceOf[Student]; new SpecialPerson(stu.name) }
      else if (obj.getClass == classOf[Older]) { val older = obj.asInstanceOf[Older]; new SpecialPerson(older.name) }
      else null
    }
    class TicketHouse {
      var ticketNumber = 0
      def buySpecialTicket(p: SpecialPerson) = {
        ticketNumber += 1
        "T-" + ticketNumber
      }
    }
    val th = new TicketHouse
    val s = new Student("leo")
    println(th.buySpecialTicket(s))
    val o = new Older("jack")
    println(th.buySpecialTicket(o))
    val t = new Teacher("tom")
    println(th.buySpecialTicket(t))
  }

}

```

```
/* Output:
T-1
T-2
T-3
*///:~

```

### ImplicitParameterDemo

```scala
package basic

object ImplicitParameterDemo {

  def main(args: Array[String]): Unit = {
    class SignPen {
      def write(content: String) = println(content)
    }
    implicit val signPen = new SignPen
    def signForExam(name: String)(implicit signPen: SignPen): Unit = {
      signPen.write(name + " come to exam in time.")
    }
    signForExam("leo")
  }

}

```

```
/* Output:
leo come to exam in time.
*///:~

```

## Actor

### ActorDemo

```scala
package basic

import scala.actors.Actor

object ActorDemoI {

  def main(args: Array[String]): Unit = {
    val helloActor = new HelloActor
    helloActor.start()
    helloActor ! "leo"
  }

  class HelloActor extends Actor {

    def act() {
      while (true) {
        receive {
          case name: String => println("Hello, " + name)
        }
      }
    }

  }

}

```

```
/* Output:
Hello, leo
*///:~

```

```scala
package basic

import scala.actors.Actor

object ActorDemoII {

  def main(args: Array[String]): Unit = {
    val userManageActor = new UserManageActor
    userManageActor.start()
    userManageActor ! Login("leo", "1234")
    userManageActor ! Register("leo", "1234")
  }

  case class Login(username: String, password: String)

  case class Register(username: String, password: String)

  class UserManageActor extends Actor {

    def act() {
      while (true) {
        receive {
          case Login(username, password) => println("login, username is " + username + ", password is " + password)
          case Register(username, password) => println("register, username is " + username + ", password is " + password)
        }
      }
    }

  }

}

```

```
/* Output:
login, username is leo, password is 1234
register, username is leo, password is 1234
*///:~

```

```scala
package basic

import scala.actors.Actor

object ActorDemoIII {

  def main(args: Array[String]): Unit = {
    val leoTelephoneActor = new LeoTelephoneActor
    leoTelephoneActor.start()
    val jackTelephoneActor = new JackTelephoneActor(leoTelephoneActor)
    jackTelephoneActor.start()
  }

  case class Message(content: String, sender: Actor)

  class LeoTelephoneActor extends Actor {

    def act() {
      while (true) {
        receive {
          case Message(content, sender) => { println("leo telephone: " + content); sender ! "I'm leo, please call me after 10 minutes." }
        }
      }
    }

  }

  class JackTelephoneActor(val leoTelephoneActor: Actor) extends Actor {

    def act() {
      leoTelephoneActor ! Message("Hello, Leo, I'm Jack", this)
      receive {
        case response: String => println("jack telephone: " + response)
      }
    }

  }

}

```

```
/* Output:
leo telephone: Hello, Leo, I'm Jack
jack telephone: I'm leo, please call me after 10 minutes.
*///:~

```