#####1.字符串String对象是不可变的，每一个看起来会修改String的方法，实际上都是创建了一个新的String对象

#####2.重载+和StringBuilder：
   用重载+，实际上编译器自动引入了StringBuilder，并为每一次重载+做了一次append()，最后调用toString生成结果
   区别：虽然底层都是StringBuilder，但使用重载+时，每+一次，就会生成一个新的StringBuilder，而用StringBuilder的append()方法时，只会生成一个StringBuilder，所以在简单的字符串操作时，可以信赖编译器，否则就用StringBuilder

#####3.StringBuilder和StringBuffer
   StringBuffer是线程安全的，开销会稍微大一点

#####4.无意识的递归：

<pre><code>public class Test {
     @Override
     public String toString() {
          return "Test [getClass()=" + getClass() + ", hashCode()=" + hashCode()
                    + ", toString()=" + this.toString() + "]";
     }

     public static void main(String[] args) {
          Test t = new Test();
          System.out.println(t);
     }

}
</code></pre>
toString方法中的this.toString(),前后有个重载+操作，编译器会将Test转换成字符串，但巧的是，toString方法还是这个toString，以此递归下去，因此java.lang.StackOverflowError;
改成super.toString(),Object.toString()才是负责此任务的方法






