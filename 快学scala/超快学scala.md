超快学scala
====================
> 《快学scala》学习笔记



第一章 基础
--------------------

#####1.变量声明
val 常量，无法改变值（鼓励使用）
var 值可变
指定类型声明：<code>var greeting : String  = “hello” </code> 
语句结束后不需要以';'结束，除非是一行代码中有多条语句


#####2.数据类型
七种数据类型：
Byte Char Short Int Long Float Double和一个Boolean类型
与java不同：这些类型都是类，不刻意区分基本类型还是引用类型


#####3.操作符
操作符其实是方法,
例如:<code>a + b </code>
实际为 a.+(b)
这里的+是方法名，scala中可以使用任意符号当做方法名


#####4.不支持++操作
<code> counter ++ </code>  *注意：不支持*
<code> counter += 1 </code> *正确*
> 因为Int类是不可变的，这样一个方法不能改变某个整数类型的值,scala的设计者们认为不值得为少按一个键而增加一个特例

#####5.ScalaDoc
[ScalaDoc](http://www.scala-lang.org/api/2.10.4/#package)


第二章 控制结构和函数
-----------------------

#####1.条件表达式
可以有返回值的if/else： <code> val result = if(condition) 1 else 2 </code>


#####2.块表达式和赋值
{}块包含一系列表达式，结果也是表达式，块中最后一个表达式的值就是块的值
<code>val distance = {val dx=x-x0; val dy=y-y0; sqrt(dx * dx + dy* dy)}</code>
适用场景：需要一系列操作的val初始化


#####3.Unit类型
等同于java中得void，还可以写做(),表示一种 “无值” 的值
比喻：相当于钱包里装了一张写着没有钱的纸.
赋值语句的值是Unit,例如 <code> y = 1 </code> 的返回值是Unit


#####4.循环
<code>for(i <- 1 to n)</code>
scala中没有break和continue，通常使用以下方法：
1. 使用Boolean控制
2. 使用嵌套函数，从函数中return
3. 使用Breaks对象中的break方法

__双重循环:__

<code>for(i <- 1 to 3; j <- 1 to 3) print((10 * i + j) + “”)	//11 12 13 21 22 23 31 32 33</code>

__守卫：__
<code>for(i<-1 to 3; j<-1 to 3; i != j) print((10 * i + j) + “”) //12 13 22 23 32 33</code>

__组合使用：__
<code>for(i<-1 to 3; from = 4-i;j<- from to 3;) print((10 * i + j) + “”)	//13 22 23 31 32 33</code>

__yield:__
循环体以yield开始，则该循环可以构造出一个集合
<code>for(i<-1 to 10) yield i % 3    //Vector(1,2,0,1,2,0,1,2,0,1)</code>


#####5.函数
函数体有多行代码，放进代码块里，最后一个表达式即为返回值
例子：<code>def abs(x : Double) = if (x >= 0) x else -x </code>

__递归函数必须有返回值__

__有默认参数的函数__:
<code>def dec(s : String, front : String = “[“, after : String = “]”) : String = front + s + after</code>

__任意个数参数的函数__:
<pre><code>def sum(args : Int*) = {
      var result = 0
      for(arg <- args) result += arg
      result
}</code></pre>

__过程__:
是一种函数的特殊表示法，函数体包含在花括号内但没有前面的=号，返回类型为Unit
<pre><code>def show(s: String) {
	print(s)
}</code></pre>

> 练习，编写一个过程countdown(n:Int)，打印从n到0的数字（答案在最后）



第三章 数组
-----------------------
#####1.定长数组
<code>val nums = new Array\[Int\](10)	//元素初始化都是0</code>
<code>val s = Array("Hello","World")	//元素类型推断出来，是String，已经提供初始值，就不用关键字new</code>
JVM中Scala的Array使用java数组方式实现的，字符串数组相当于java.lang.String[]，Int,Double或者其他与java中基本类型对应的数组都是基本类型数组,例如Array(1,2,3,4)相当于int[].



#####2.变长数组
相当于java中的ArrayList,Scala中为ArrayBuffer.
<pre><code>val b = ArrayBuffer\[int\]()
b += 1 	//在尾端加一个元素
b += (2,3,4,5)	//在尾端加多个元素
b.trimEnd(5)		//移除最后五个元素</code></pre>

> 在数组缓冲的尾端添加或移除元素是一种高效的操作

<pre><code>b.insert(2, 5)		//在下标2之前插入一个元素
b.insert(2, 1, 2, 3)	//在下标2之前插入多个元素
b.remove(2)			//移除下标2这个元素（第三个）
b.remove(2, 3)		//从下标2开始，移除多个元素，第二个参数是移除多少个</code></pre>

当我们要构建一个数组，但是又不知道他得长度时，应当先定义一个<code>ArrayBuffer</code>,然后调用<code>.toArray</code>方法，例如：<code>b.toArray</code>

反过来也可以将一个数组调用<code>.toBuffer</code>方法，将一个数组转成数组缓冲



#####3.遍历
<code>for(i <- 0 until b.length)	print(i + " : " b(i))</code>

两个元素一跳:<code>0 until (b.length, 2)</code>

从尾端遍历：<code>(0 until b.length).reverse</code>

不需要下标的遍历： <code>for(elem <- b) print("elem : " + elem)</code>



#####4.数组转换
<pre><code>val a = Array(1,2,3,4,5)
val result = for(elem <- a if elem % 2 == 0) yield elem * 2
//Array(4,8)</code></pre>
如果从数组出发，yield操作之后将得到一个数组，如果是数组缓冲，yield之后将得到数组缓冲

函数式编程风格：<code>a.filter(\_ % 2 ==0).map(\_ * 2)</code>

> 思考：给定一个整数的数组缓冲，移除第一个负数之外的所有负数.传统做法：遇到第一个负数时，置一个标记，然后移除剩下的所有负数，代码如下：
<pre><code>var first = true
var a = ArrayBuffer(1,2,-1,3,-2,4)
var n = a.length
var i = 0
while(i < n){
  if(a(i) >= 0) i+=1
  else{
    if(first) {first = false; i += 1}
    else {a.remove(i); n -= 1} 
  }
}</code></pre>

> 之前曾经提到过，在数组缓冲的尾端添加或移除元素是一种高效的操作，一个更好的办法实现该操作。（代码见练习2）

#####5.其他常用方法
求和：<code>Array(0,1,2).sum		//元素类型必须为数值类型</code>
最大值: <code>Array("12", "b").max	//ascii相加较大的,return "12"</code>
排序：<code>Array(1,3,2,4).sorted(_ < _)		//Array(1,2,3,4)</code>
mkString : <code>Array(1,2,3).mkString(" and ")	//1 and 2 and 3</code>
		   <code>Array(1,2,3).mkString("<",",",">")	//<1,2,3></code>
其余方法请参考：[scala doc array](http://www.scala-lang.org/api/2.10.4/#scala.Array)









练习答案
--------------------------
1.编写一个过程countdown(n:Int)，打印从n到0的数字
<pre><code>def countdown(n:Int){  
	0 to n reverse foreach print  
}</code></pre>

2.优化后的方法：先将需要删除的元素下标记录下来，然后将元素移动到数组缓冲的结尾，最后截断，代码如下：
<pre><code>var first = true
val indexes = for(i <- 0 until a.length if first || a(i) > 0) yield {
   if(a(i) < 0 ) first = false ; i
}
for(j <- 0 until indexes.length) a(j) = a(indexes(j))
a.trimEnd(a.length - indexes.length)</code></pre>
> 输入一千个数的ArrayBuffer, 测试两种不同的截断方法，时间相差无几，有时候第一种方法还快 - -!








