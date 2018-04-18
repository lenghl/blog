# <center>val</center> #
# <center>Finally! Hassle-free final local variables.</center> #
`val`是在lombok 0.10 版本中被引入的。
## <center>Overview</center> ##
&nbsp; &nbsp; &nbsp; 你可以使用`val`作为一个`局部变量`声明时的类型，而不用明确地写出这个`局部变量`的具体类型。当你这样作时，这个`局部变量`的实际类型将会根据`初始化表达式`推断出来。`局部变量`也会被设置为`final`类型。这个feature仅仅作用于`局部变量`和`foreach循环`，而不会作用于`字段`。`初始化表达式`是必须要有的。
</br>&nbsp; &nbsp; &nbsp; `val`实际上是sorts的一个`type`，而且作为一个真实的class存在于`lombok`package中。在使用时你必须要引入`val`(或者使用`lombok.val`作为类型)。在局部变量声明时`val`的使用会触发两个过程：添加`final`字段和使用`初始化表达式`的类型覆盖`val`。
`WARNING: This feature does not currently work in NetBeans.`
## <center>With Lombok</center> ##
<pre><code>
import java.util.ArrayList;
import java.util.HashMap;
import lombok.val;

public class ValExample {
  public String example() {
    val example = new ArrayList<String>();
    example.add("Hello, World!");
    val foo = example.get(0);
    return foo.toLowerCase();
  }
  
public void example2() {
    val map = new HashMap<Integer, String>();
    map.put(0, "zero");
    map.put(5, "five");
    for (val entry : map.entrySet()) {
      System.out.printf("%d: %s\n", entry.getKey(), entry.getValue());
    }
  }
}
</pre></code>
## <center>Vanilla Java</center> ##
<pre><code>
import java.util.ArrayList;
import java.util.HashMap;
import lombok.val;

public class ValExample {
  public String example() {
    val example = new ArrayList<String>();
    example.add("Hello, World!");
    val foo = example.get(0);
    return foo.toLowerCase();
  }
  
public void example2() {
    val map = new HashMap<Integer, String>();
    map.put(0, "zero");
    map.put(5, "five");
    for (val entry : map.entrySet()) {
      System.out.printf("%d: %s\n", entry.getKey(), entry.getValue());
    }
  }
}
</pre></code>
## <center>Supported configuration keys:</center> ##
`lombok.val.flagUsage` = [`warning` | `error`] (default: not set)</br>
`lombok`将会把对`val`的使用标记为警告或者错误如果配置。
## <center>Small print</center> ##
&nbsp; &nbsp; &nbsp; 对于`初始化表达式`有多个可选类型时，最大的共有父类会被选择，而不会时接口。比如，`bool ? new HashSet() : new ArrayList()`是一个有多个可选类型的表达式:结果类型是`AbstractCollection`或者`Serializable`。被推断的类型会是`AbstractCollection`，因为它是类，而`Serializable`是接口。</br>
&nbsp; &nbsp; &nbsp; 在更复杂的情况里，比如`初始化表达式`的值是`null`，`java.lang.Object`会被选择。
## 参考资料
https://projectlombok.org/features/val