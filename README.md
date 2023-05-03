Download Link: https://assignmentchef.com/product/solved-cs2030s-lab-4-immutable-list
<br>
<h2>Topic Coverage</h2>

<ul>

 <li>Static methods</li>

 <li>Interface</li>

 <li>Generics</li>

 <li>Wildcards</li>

 <li>Variable-Length Arguments</li>

</ul>

<h2>Problem Description</h2>

In this lab, we are going to extend our generic immutable <tt>Box</tt> to take the form of a list. Although Java provides a <tt>List</tt> interface as a <tt>Collection</tt>, the <code>List</code> implementations from the Java Collection Framework are still mutable.

In this lab, you will build you own immutable list class (<code>ImmutableList</code>) that implements various methods to manipulate this list We do this by wrapping an immutable API over a mutable <code>List</code> object — known as the <em>immutable delegation class</em> class, which obviously comes with a performance overhead. Note that a much more efficient implementation of an immutable list is possible (using a recursive data structure), but let’s leave that to a future lab on infinite lists.

<h2>Task</h2>

By wrapping around an internal mutable <code>List</code>, implement the methods <code>add</code>, <code>remove</code>, <code>replace</code>, <code>limit</code>, <code>equals</code>, <code>sorted</code>, and <code>toArray</code>.




<table border="1" cellpadding="10">

 <tbody>

  <tr>

   <td><h2>Level 1</h2><h3>Creating a Generic Immutable List</h3>Create an <i>immutable</i> class <code>ImmutableList</code> that operates as a list. The list should be <em>ordered</em>, i.e., the position of the items in the list matters. Your immutable list should not implement the <code>List</code> interface but should contain a <code>List</code> instead.<pre>import java.util.List;class ImmutableList&lt;T&gt; {    private final List&lt;T&gt; list;}</pre>Your class should have two constructors, both accept items to populate your new immutable list. The first one takes in a generic <code>List</code> containing the items as an argument; the second takes in a sequence of items as <em>variable-length arguments</em> (or varargs). Variable-length arguments allow you to pass an unspecified number of arguments to a method or constructor. You can <a href="https://docs.oracle.com/javase/tutorial/java/javaOO/arguments.html">read up about varargs</a> if you are unfamiliar with this Java feature.Using varargs with generic type parameters could be unsafe. Varargs is a syntactic sugar for an array, which is covariant in Java and potentially unsafe as you have seen in class (recall assigning a <tt>Shape</tt> array variable to reference a <tt>Circle</tt> array, and then storing a <tt>Rectangle</tt> as one of the array elements).To tell the compiler that you know what you are doing is type-safe, when you declare a method that takes in varargs with a generic type parameter, add an annotation <code>@SafeVarargs</code> before the method.Your class should also have an appropriate <code>toString</code> method which prints out the contents of the List.


    <table border="1" cellpadding="10">

     <tbody>

      <tr>

       <td><pre><b>jshell&gt; /open ImmutableList.java</b><b>jshell&gt; new ImmutableList&lt;Integer&gt;(1,2,3)</b>$.. ==&gt; [1, 2, 3]<b>jshell&gt; new ImmutableList&lt;Integer&gt;(Arrays.asList(1,2,3))</b>$.. ==&gt; [1, 2, 3]<b>jshell&gt; new ImmutableList&lt;Integer&gt;(new ArrayList&lt;Integer&gt;(Arrays.asList(1,2,3)))</b>$.. ==&gt; [1, 2, 3]<b>jshell&gt; new ImmutableList&lt;Integer&gt;()</b>$.. ==&gt; []<b>jshell&gt; /exit</b></pre></td>

      </tr>

     </tbody>

    </table>Check the format correctness of the output by running the following on the command line:<pre>$ javac -Xlint:rawtypes *.java$ jshell -q &lt; test1.jsh</pre>The <code>-Xlint:rawtypes</code> flag would warn you if you forget to specify generics and use raw types instead.Check your styling by issuing the following<pre>$ checkstyle *.java</pre></td>

  </tr>

  <tr>

   <td><h2>Level 2</h2><h3>Manipulating the List</h3>Include the method <code>add</code> (append an item to the list), <code>replace</code> (replace all occurences of an item with another), and <code>remove</code> (remove the first occurence of an item) methods. Remember your list has to be immutable. <pre><b>jshell&gt; /open ImmutableList.java</b><b>jshell&gt; List&lt;Integer&gt; mList = new ArrayList&lt;Integer&gt;(Arrays.asList(1,2,3))</b><b>jshell&gt; ImmutableList&lt;Integer&gt; imList = new ImmutableList&lt;Integer&gt;(mList)</b><b>jshell&gt; imList.remove(3)</b>$.. ==&gt; [1, 2]<b>jshell&gt; imList</b>imList ==&gt; [1, 2, 3]<b>jshell&gt; imList.remove(3).add(2)</b>$.. ==&gt; [1, 2, 2]<b>jshell&gt; imList</b>imList ==&gt; [1, 2, 3]<b>jshell&gt; imList.remove(6)</b>$.. ==&gt; [1, 2, 3]<b>jshell&gt; imList.add(1).replace(1,3)</b>$.. ==&gt; [3, 2, 3, 3]<b>jshell&gt; imList.add(1).replace(1,1)</b>$.. ==&gt; [1, 2, 3, 1]<b>jshell&gt; imList.replace(6,3)</b>$.. ==&gt; [1, 2, 3]<b>jshell&gt; mList.set(0,10)</b>$.. ==&gt; 1<b>jshell&gt; mList</b>mList ==&gt; [10, 2, 3]<b>jshell&gt; imList</b>imList ==&gt; [1, 2, 3]<b>jshell&gt; Integer[] array = {1, 2, 3}</b><b>jshell&gt; ImmutableList&lt;Integer&gt; imList = new ImmutableList&lt;Integer&gt;(array)</b><b>jshell&gt; array[0] = 10</b>$.. ==&gt; 10<b>jshell&gt; imList</b>imList ==&gt; [1, 2, 3]<b>jshell&gt; new ImmutableList&lt;Integer&gt;(List.of(4,5,6)).add(7)</b>$.. ==&gt; [4, 5, 6, 7]<b>jshell&gt; ImmutableList&lt;String&gt; stringList = new ImmutableList&lt;String&gt;(Arrays.asList("One","Two","Three"))</b><b>jshell&gt; stringList.add("Four");</b>$.. ==&gt; [One, Two, Three, Four]<b>jshell&gt; stringList</b>stringList ==&gt; [One, Two, Three]<b>jshell&gt; /exit</b></pre>Check the format correctness of the output by running the following on the command line:<pre>$ javac -Xlint:rawtypes *.java$ jshell -q &lt; test2.jsh</pre>Check your styling by issuing the following<pre>$ checkstyle *.java</pre></td>

  </tr>

  <tr>

   <td><h2>Level 3</h2><h3>Limiting the List</h3>Let’s create a new method <code>limit</code> in the <code>ImmutableList</code> class. The <code>limit</code> method takes in a <code>long</code> value and then returns an <code>ImmutableList</code> truncated to the length specified by that <code>long</code> value.Consider the case where you pass in a negative number. In this case, let’s throw an <code>IllegalArgumentException</code> with the exception message “<code>limit size &lt; 0</code>“.It is important to note that, the test cases below <em>catch unchecked exceptions for the purpose of testing only</em>. It is an unusual coding practice to catch unchecked exceptions, as unchecked exceptions are usually caused by bugs or improper use of APIs, and the application usually cannot recover from such exceptions.


    <table border="1" cellpadding="10">

     <tbody>

      <tr>

       <td><pre><b>jshell&gt; /open ImmutableList.java</b><b>jshell&gt; new ImmutableList&lt;Integer&gt;(1,2,3).limit(1)</b>$.. ==&gt; [1]<b>jshell&gt; new ImmutableList&lt;Integer&gt;(1,2,3).limit(10)</b>$.. ==&gt; [1, 2, 3]<b>jshell&gt; ImmutableList&lt;Integer&gt; list = new ImmutableList&lt;Integer&gt;(1,2,3)</b><b>jshell&gt; list.limit(0)</b>$.. ==&gt; []<b>jshell&gt; list</b>list ==&gt; [1, 2, 3]<b>jshell&gt; list = list.limit(0)</b><b>jshell&gt; try {</b><b>   ...&gt; new ImmutableList&lt;Integer&gt;(1,2,3).limit(-1);</b><b>   ...&gt; } catch (IllegalArgumentException e) {</b><b>   ...&gt; System.out.println(e);</b><b>   ...&gt; }</b>java.lang.IllegalArgumentException: limit size &lt; 0<b>jshell&gt; /exit</b></pre></td>

      </tr>

     </tbody>

    </table>Check the format correctness of the output by running the following on the command line:<pre>$ javac -Xlint:rawtypes *.java$ jshell -q &lt; test3.jsh</pre>Check your styling by issuing the following<pre>$ checkstyle *.java</pre></td>

  </tr>

  <tr>

   <td><h2>Level 4</h2><h3>List Equality and Sorting the List</h3>Write an overridden method <tt>equals</tt> that takes in another <tt>ImmutableList</tt> and checks for equality. Two immutable lists are the same if they contain the same elements (as determined by each element’s <tt>equals</tt> method).We would also like to sort the list using an arbitrary <code>Comparator</code>. Let’s create a <code>sorted</code> method that takes in a <code>Comparator</code> and returns the <code>ImmutableList</code> as sorted by this <code>Comparator</code>. What if a <code>null</code> value is passed as a <code>Comparator</code>? To preempt such <i>irresponsible</i> actions, let’s throw a <code>NullPointerException</code> with the exception message “<code>Comparator is null</code>“.


    <table border="1" cellpadding="10">

     <tbody>

      <tr>

       <td><pre><b>jshell&gt; /open ImmutableList.java</b><b>jshell&gt; </b><b>jshell&gt; ImmutableList&lt;Integer&gt; list = new ImmutableList&lt;Integer&gt;(1,2,3)</b><b>jshell&gt; list.equals(list)</b>$.. ==&gt; true<b>jshell&gt; list.equals(Arrays.asList(1,2,3))</b>$.. ==&gt; false<b>jshell&gt; list.equals(new ImmutableList&lt;Integer&gt;(1,2,3))</b>$.. ==&gt; true<b>jshell&gt; list.equals(new ImmutableList&lt;Integer&gt;(1,2))</b>$.. ==&gt; false<b>jshell&gt; list.equals(new ImmutableList&lt;Integer&gt;(1,2,3,3))</b>$.. ==&gt; false<b>jshell&gt; list.equals(new ImmutableList&lt;Integer&gt;(3,2,1))</b>$.. ==&gt; false<b>jshell&gt; list.sorted(new Comparator&lt;Integer&gt;() {</b><b>   ...&gt;     public int compare(Integer i1, Integer i2) {</b><b>   ...&gt;         return i2 - i1;</b><b>   ...&gt;     }})</b>$.. ==&gt; [3, 2, 1]<b>jshell&gt; list</b>list ==&gt; [1, 2, 3]<b>jshell&gt; try {</b><b>   ...&gt;   new ImmutableList&lt;Integer&gt;(1,2,3).sorted(null);</b><b>   ...&gt; } catch (NullPointerException e) {</b><b>   ...&gt;   System.out.println(e);</b><b>   ...&gt; }</b>java.lang.NullPointerException: Comparator is null<b>jshell&gt; /exit</b></pre></td>

      </tr>

     </tbody>

    </table>Check the format correctness of the output by running the following on the command line:<pre>$ javac -Xlint:rawtypes *.java$ jshell -q &lt; test4.jsh</pre>Check your styling by issuing the following<pre>$ checkstyle *.java</pre></td>

  </tr>

  <tr>

   <td><h2>Level 5</h2><h3>Implement a toArray Method</h3>Create overloaded methods <code>toArray</code> that takes in either no argument, or an array as an argument. The <code>toArray</code> method without argument will return the items in the list as an array of type <code>Object[]</code>. The <code>toArray</code> method with an array as an argument is a <strong><em>generic method</em></strong> that will return the items in the list in an array <em>of the same type</em> as the argument. The behavior of <code>toArray</code> of your list is similar to that of <code>toArray</code> of <code>List</code>, with the only difference being the error message associated with the exception thrown.When an array of the wrong type is being passed into the ImmutableList, an <code>ArrayStoreException</code> exception is thrown with the exception message “<code>Cannot add element to array as it is the wrong type</code>“.As for passing a <code>null</code> argument? Once again, we throw a <code>NullPointerException</code> with the exception message “<code>Input array cannot be null</code>“.


    <table border="1" cellpadding="10">

     <tbody>

      <tr>

       <td><pre><b>jshell&gt; /open ImmutableList.java</b><b>jshell&gt; new ImmutableList&lt;Integer&gt;(1,2,3).toArray()</b>$.. ==&gt; Object[3] { 1, 2, 3 }<b>jshell&gt; new ImmutableList&lt;Integer&gt;().toArray()</b>$.. ==&gt; Object[0] {  }<b>jshell&gt; Integer[] integers = new ImmutableList&lt;Integer&gt;(1,2,3).toArray(new Integer[0])</b><b>jshell&gt; integers</b>integers ==&gt; Integer[3] { 1, 2, 3 }<b>jshell&gt; try {</b><b>   ...&gt;   new ImmutableList&lt;Integer&gt;(1,2,3).toArray(new String[0]);</b><b>   ...&gt; } catch (ArrayStoreException e) {</b><b>   ...&gt;   System.out.println(e);</b><b>   ...&gt; }</b>java.lang.ArrayStoreException: Cannot add element to array as it is the wrong type<b>jshell&gt; try {</b><b>   ...&gt;   new ImmutableList&lt;Integer&gt;(1,2,3).toArray(null);</b><b>   ...&gt; } catch (NullPointerException e) {</b><b>   ...&gt;   System.out.println(e);</b><b>   ...&gt; }</b>java.lang.NullPointerException: Input array cannot be null<b>jshell&gt; /exit</b></pre></td>

      </tr>

     </tbody>

    </table>Check the format correctness of the output by running the following on the command line:<pre>$ javac -Xlint:rawtypes *.java$ jshell -q &lt; test5.jsh</pre>Check your styling by issuing the following<pre>$ checkstyle *.java</pre></td>

  </tr>

  <tr>

   <td><h2>Bonus Level</h2><h3>Implement Map and Filter Methods</h3>Now, let’s create two new methods in the ImmutableList class.First, the <code>filter</code> method. The <code>filter</code> method will return an <code>ImmutableList</code> with elements based on a <code>Predicate</code> you pass to it. Remember that <code>Predicate</code> is a generic functional interface, and therefore it has one method to implement: <code>test</code>.Next, implement the <code>map</code> method. The <code>map</code> method will return an <code>ImmutableList</code> in which all elements are transformed based on a <code>Function</code> you pass to it. Again, <code>Function</code> is a generic functional interface, and therefore it has one method to implement: <code>apply</code>. Remember that the input type may not be the same as the output type: consider a mapping from <code>String</code> to <code>Integer</code> using the <code>length()</code> method.Try passing your own <code>Predicate</code>s and <code>Function</code>s to these methods! We use lambda expressions in the following test cases below. You are encouraged to <a href="https://docs.oracle.com/javase/tutorial/java/javaOO/lambdaexpressions.html" target="_blank" rel="noopener">read more about lambda expressions</a> as you will be using them extensively for the rest of the semester.


    <table border="1" cellpadding="10">

     <tbody>

      <tr>

       <td><pre><strong>jshell&gt; /open ImmutableList.java</strong><strong>jshell&gt; new ImmutableList&lt;Integer&gt;(1,2,3).filter(x -&gt; x % 2 == 0)</strong>$.. ==&gt; [2]<strong>jshell&gt; new ImmutableList&lt;String&gt;("one", "two", "three").filter(x -&gt; x.hashCode()%10 &gt; 5)</strong>$.. ==&gt; [two, three]<strong>jshell&gt; Predicate&lt;Object&gt; p = x -&gt; x.hashCode()%10 == 0</strong><strong>jshell&gt; new ImmutableList&lt;String&gt;("one", "two", "three").filter(p)</strong>$.. ==&gt; []<strong>jshell&gt; ImmutableList&lt;Integer&gt; list = new ImmutableList&lt;String&gt;("one", "two", "three").map(x -&gt; x.length())</strong><strong>jshell&gt; /var list</strong>|    ImmutableList&lt;Integer&gt; list = [3, 3, 5]<strong>jshell&gt; Function&lt;Object,Integer&gt; f = x -&gt; x.hashCode()</strong><strong>jshell&gt; ImmutableList&lt;Number&gt; list = new ImmutableList&lt;String&gt;("one", "two", "three").map(f)</strong><strong>jshell&gt; /var list</strong>|    ImmutableList&lt;Number&gt; list = [110182, 115276, 110339486]<strong>jshell&gt; new ImmutableList&lt;Integer&gt;(1,2,3).filter(x -&gt; x &gt; 3).map(x -&gt; x + 1)</strong>$.. ==&gt; []<strong>jshell&gt; new ImmutableList&lt;Integer&gt;(1,2,3).filter(x -&gt; x &gt; 2).map(x -&gt; x + 1)</strong>$.. ==&gt; [4]<strong>jshell&gt; new ImmutableList&lt;Integer&gt;(1,2,3).map(x -&gt; x + 1).filter(x -&gt; x &gt; 2)</strong>$.. ==&gt; [3, 4]<strong>jshell&gt; new ImmutableList&lt;String&gt;().filter(s -&gt; s.endsWith("s"))</strong>$.. ==&gt; []<strong>jshell&gt; new ImmutableList&lt;String&gt;().map(s -&gt; s.endsWith("s"))</strong>$.. ==&gt; []<strong>jshell&gt; /exit</strong></pre></td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>