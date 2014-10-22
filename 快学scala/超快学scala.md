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








练习答案
--------------------------
1.编写一个过程countdown(n:Int)，打印从n到0的数字
<pre><code>def countdown(n:Int){  
    0 to n reverse foreach print  
}</code></pre>









