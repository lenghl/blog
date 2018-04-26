# <center>@NonNull</center> #
# <center>or: How I learned to stop worrying and love the NullPointerException.</center> #
`@NonNull`是在lombok v0.11.10 版本中被引入的。</br>
## <center>Overview</center> ##
</br>&nbsp; &nbsp; &nbsp; 你可以使用`@NonNull`修饰某个方法或者构造函数来让`lombok`为你生成一个空指针检测代码块。
</br>&nbsp; &nbsp; &nbsp; `lombok`总会将任何一个名字为`@NonNull`且修饰字段的注解作为一个信号，就是在完全由`lombok`构造的方法或构造函数里生成一个空指针检测代码块，例如`@Data`。现在，需特别注意，使用`lombok`自己的`@lombok.NonNull`修饰某个入参仅会导致你自己的方法或者构造函数里面生成一个空指针检测代码块。
</br>&nbsp; &nbsp; &nbsp; 空指针检测代码块看起来像`if (param == null) throw new NullPointerException("param")`；而且会被插入方法的最顶端。对于构造函数，空指针检测代码块将会被插入到显示调用`this()`或者`super()`调用后面。
</br>&nbsp; &nbsp; &nbsp; 如果一个空指针检测代码块已经在方法顶端，则不会有额外的空指针检测代码块生成。
## <center>With Lombok</center> ##
<pre><code>
import lombok.NonNull;

public class NonNullExample extends Something {
  private String name;
  
  public NonNullExample(@NonNull Person person) {
    super("Hello");
    this.name = person.getName();
  }
}
</pre></code>
## <center>Vanilla Java</center> ##
<pre><code>
import lombok.NonNull;

public class NonNullExample extends Something {
  private String name;
  
  public NonNullExample(@NonNull Person person) {
    super("Hello");
    if (person == null) {
      throw new NullPointerException("person");
    }
    this.name = person.getName();
  }
}
</pre></code>
## <center>Supported configuration keys:</center> ##
`lombok.nonNull.exceptionType` = [`NullPointerException` | `IllegalArgumentException`] (default: `NullPointerException`)</br>
当`lombok`生成一个`if`空指针检测代码块时，缺省情况下，一个使用字段名字作为异常消息的`java.lang.NullPointerException`将会被抛出。当然，你可以使用`IllegalArgumentException `在配置里面来使得`lombok`抛出使用`fieldName is null`作为异常消息的这类`IllegalArgumentException `异常，
`lombok.nonNull.flagUsage` = [`warning` | `error`] (default: not set)</br>
`lombok`将会把对`@NonNull`的使用标记为警告或者错误如果配置。
## <center>Small print</center> ##
针对已经存在的空指针检测代码块，`lombok`的检测方案就是扫描`if`代码块，判别的条件就是与`lombok`自己的相似。相似的具体含义：任何`throws`语句要所属`if`语句的`then`部分，无论是否在括号里面，无论有几个。`if`语句的条件表达式的模式必须和`PARAMNAME == null`一致。方法中的第一个语句如果不是空指针检测代码块，则对于这个方法的检测将停止。</br>
当`@Data`或者`lombok`其他的方法生成注解触发名字为`@NonNull`的注解时，无论封装情况或者包名如何，这只会触发`lombok`自己的`@NonNull`。</br>
`@NonNull`不能修饰基本数据类型，否则会导致`warning`而且不会生成空指针检测代码块。</br>
`@NonNull`修饰抽象方法的入数以前会导致`warning`；从`1.16.8`版本开始，就不会时这种情况了，因为`@NonNull`被赋予了一个记录的角色。也正因如此，你能够使用`@NonNull`注释任何一个方法，这不会生成`warning`，也不会生成代码。

## 参考资料
https://projectlombok.org/features/NonNull