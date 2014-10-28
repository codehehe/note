超快学scala
====================
> 《快学scala》学习笔记 : 每敲一个键都是珍贵的



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

排序：<code>Array(1,3,2,4).sorted(\_ < \_)		//Array(1,2,3,4)</code>

mkString : <code>Array(1,2,3).mkString(" and ")	//1 and 2 and 3
                 Array(1,2,3).mkString("<",",",">")	  //<1,2,3></code>

其余方法请参考：[scala doc array](http://www.scala-lang.org/api/2.10.4/#scala.Array)




第四章 映射和元组
-----------------------
#####1.构造映射
<code>val scores = Map("Alice" -> 10, "Bob" -> 3, "Cindy" ->8)    //不可变的映射</code>
如果想要创建一个可变的映射，需要引入scala.collection.mutable.Map
如果要从一个空的映射开始 <code>val map = new HashMap[String, Int]     //注意这里的HashMap也需要从mutable引入</code>
> scala中，映射是对偶的集合。
也可以用<code>val socres = Map(("Alice", 10) ,("Bob", 3), ("Cindy", 8))</code>这种对偶集合的方式创建映射


#####2.从映射中取值
<code>val bobsScore = scores("Bob")</code>
如果映射中不包含"Bob"这个键，则会抛出异常java.util.NoSuchElementException
检查某个键是否存在:<code>val bobsScore = if(scores.contains("Bob")) scores("Bob") else 0</code>
更简单的方法:<code>val bobsScore = scores.contains("Bob", 0)</code>


#####3.更新
<code>scores("Bob") = 5   //更新</code>
<code>scores("Fred") = 9   //添加</code>
<code>scores += ("Bob" -> 5, "Fred" -> 9)  //更简单的操作</code>
<code>scores -= ("Alice")   //-=当然也是支持的</code>


#####4.迭代映射
<code>for((k,v) <- map)   print("key : " + k + ", value : " + v)</code>
<code>map.keySet    //获得所有键</code>
<code>for(v <- map.values)    //遍历所有值</code>


#####5.已排序的映射
映射底层有两种实现，*平衡树*和*哈希表*.默认情况下，scala使用哈希表，要得到一个不可变的平衡树形映射，可以:
<code>val scores = scala.collections.immutable.SortedMap("Alice" -> 10, "Bob" -> 3, "Cindy" ->8)</code>

scala没有提供可变的平衡树映射.

> 平衡树映射和哈希表映射的区别是什么？各适用于什么场景？


#####6.元组
映射是对偶的集合，对偶是元组的最简单形态，元组是不同类型的值得聚集。
<code>(1, 3.14, "Fred")</code>
类型为Tuple3[Int, Double, java.lang.String]
访问元组：<code>val second = t._2</code>或者<code>t _2</code>
> 元组通常用于函数不只返回一个值的情况


#####7.拉链操作
<pre><code>val symbols = Array("<", "-", ">")
val counts = Array(2, 10, 2)
val pairs = symbols.zip(counts)   //Array(("<", 2), ("-", 10), (">", 2))
for((k,v) <- pairs) print(k * v)  //<<---------->></code></pre>

> 练习，编写一段程序，从文件中读取单词。用一个可变映射来清点每个单词出现的频率。


第五章 类
-----------------------

#####1.简单类和无参方法
类不需要声明为public,写public会报编译错误，默认具有公有可见性
例子：
<pre><code>class Counter{
  private var value = 0
  def increment() { value += 1 }
  def current() = value
}</code></pre>
使用：
<code>val counter = new Counter     //or new Counter()</code>
调用无参方法，可以不用写()
<code>counter.current</code>
建议：对于改值器（例如上面的increment方法），要使用(),对于取值器，去掉()，可以通过声明不带()的改值器方法，来强制这种风格。


#####2.属性
java中为get/set方法实现.

<pre><code>class Person{
  var age = 0
}</code></pre>
这里定义了一个Person类，和一个公有字段，scala面向JVM生成了一个私有的age字段和相应的公有get/set方法，分别是age和age_=。
如果将age声明为private，则生成的get/set也是私有的。
要想看到以上效果，可以执行<code>scalac Person.class</code>,<code>javap -private Person</code>查看字节码

<pre><code>public class Person {
  private int age;
  public int age();
  public void age_$eq(int);     //=被翻译成了$eq
  public Person();
}</code></pre>

可以重新定义get/set方法：
<pre><code>class Person{
  private var privateAge = 0    //改名，并变成私有
  def age = privateAge
  def age_= (newValue : Int){
    privateAge = newValue
  }
}
</code></pre>

<pre><code>val fred = new Person
fred.age = 20
print(fred.age)</code></pre>


**重要提示**
1.如果字段是私有的，则get/set方法也是私有的
2.如果字段是val，scala则只生成get方法(代表只读),和一个私有的final字段
3.如果不需要任何get/set方法，将字段声明为private[this]

新的需求，有一个这样的属性：客户端不能随意改值，但可以通过某种途径，可以修改这个值，也能取得这个值（例如之前的Counter类）。

分析：1.用私有的字段（no，因没有公有的get方法）
     2.设置属性为val（no，因没有公有的set方法）
     3.需要提供一个私有字段，和一个属性的get方法，像这样：
    <pre><code>class Counter{
      private var value = 0
      def increment() { value += 1 }
      def current = value
    }</code></pre>
因为private属性没有提供公有的set方法，但是提供了increment方法，间接的修改了var value的值，同时又定义了一个current属性，来获取value的值


#####3.对象私有字段
<pre><code>class Counter{
  private var value = 0
  def increment() { value += 1 }
  def current = value
  def isLess(other : Counter) = value \< other.value    //可以访问到另外一个对象的私有字段
}</code></pre>

scala提供更加严格的访问限制，通过private[this]修饰来实现，<code>private[this] var value = 0</code>
这样<code>other.value</code>就无法访问到了。对于这样的对象私有字段，scala根本不会声明get/set方法.


#####4.Bean属性
使用@BeanProperty标记，生成java bean所欲期的getXXX和setXXX方法

<pre><code>class Person{
  @BeanProperty var name : String = _
}</code></pre>

生成了四个方法：
1.name:String
2.name_=(newValue : String) : Unit
3.getName() : String
4.setName(newValue : String) : Unit

或者：class Person(@BeanProperty var name : String)


#####5.辅助构造器
1.名称为this
2.调用辅助构造器之前必须以一个先前已经定义过的辅助构造器或者主构造器的调用开始
<pre><code>class Person{
  private var name = ""
  private var age = 0

  def this(name : String){
    this()  //调用主构造器
    this.name = name
  }

  def this(name : String, age : Int){
    this(name)  //调用之前的构造器
    this.age = age
  }
}</code></pre>


#####6.主构造器
放在类名之后<code>class Person(val name: String, val age: Int){...}</code>，主构造器的参数被编译为参数

在JVM中字节码如下：
<pre><code>public class Person {
  private final java.lang.String name;
  private final int age;
  public java.lang.String name();
  public int age();
  public Person(java.lang.String, int);
}
</code></pre>

<pre><code>class Person{
  print("constructor invoke...")  //构造函数的一部分
}</code></pre>

通过主构造器中使用默认的参数来避免过多的使用辅助构造器：
<code>class Person(val name: String = "", val age: Int = 0){...}</code>

如果不带val或var的参数被至少一个方法使用，则该参数被升级为字段：
<pre><code>class Person(name: String, age: Int){
  def desc = "name is " + name + " age : " + age
}</code></pre>

JVM中的字节码：
<pre><code>public class Person {
  private final java.lang.String name;
  private final int age;
  public java.lang.String desc();
  public Person(java.lang.String, int);
}</code></pre>


总结(主构造器参数和生成的字段和方法):
name : String : 对象私有字段。如果没有方法使用name，则没有name字段 
private val/var name : String : 私有字段。私有的get/set方法
val/var name : String : 私有字段。公有的get/set方法
@BeanProperty val/var name : String : 私有字段。公有的scala版和javaBean版get/set方法 


构造函数私有化：<code>class Person private(val id : Int)</code>




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

> 我对输入一千个数的ArrayBuffer, 测试两种不同的截断方法，时间相差无几，有时候第一种方法还快 - -!


3.编写一段程序，从文件中读取单词。用一个可变映射来清点每个单词出现的频率。
<pre><code>import scala.io.Source  
import scala.collection.mutable.HashMap  
  
val source = Source.fromFile("myfile.txt").mkString  
  
val tokens = source.split("\\s+")  
  
val map = new HashMap[String,Int]  
  
for(key <- tokens){  
    map(key) = map.getOrElse(key,0) + 1  
}  
  
println(map.mkString(","))</code></pre>







