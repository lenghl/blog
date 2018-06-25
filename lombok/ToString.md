# <center>@ToString</center> #
# <center>不需要启动一个调试器来查看你的成员变量:仅需让`lombok`生成一个`toString`给你!</center> #
## <center>Overview</center> ##
任何类定义都可以被`@toString`注释, 来让`lombok`生成一个`toString`方法. 缺省状态下, 这个方法 会打印类的名字, 和按序排列, 被逗号分隔的每一个成员变量.<br/>
通过设置参数`includeFieldNames`为`true`来增加`toString()`输出的内容及格式的清晰度(当然, 这个参数比较长).<br/>
缺省状态下, 所有成员变量都会被打印, 如果你想略过某些字段, 你可以提名它们在`exclude`参数中; 每一个被提名的参数都不会在被打印.<br/>
相对应的, 你可以精确的指定出你希望哪些字段被使用通过提名它们在`of`参数中.<br/>
通过设置`callSuper`为`true`, 可以把父类的`toString()`方法的输出添加到由`lombok`生成的`toString()`的输出中. 需要注意的是根类`java.lang.Object`中<br/>
的`toString()`方法的输出几乎没有意义, 所以你最好不要这样作除非你继承了其他的类.

## <center>With Lombok</center> ##
<pre><code>
import lombok.ToString;

@ToString(exclude="id")
public class ToStringExample {
  private static final int STATIC_VAR = 10;
  private String name;
  private Shape shape = new Square(5, 10);
  private String[] tags;
  private int id;
  
  public String getName() {
    return this.name;
  }
  
  @ToString(callSuper=true, includeFieldNames=true)
  public static class Square extends Shape {
    private final int width, height;
    
    public Square(int width, int height) {
      this.width = width;
      this.height = height;
    }
  }
}
</pre></code>
## <center>Vanilla Java</center> ##
<pre><code>
import java.util.Arrays;

public class ToStringExample {
  private static final int STATIC_VAR = 10;
  private String name;
  private Shape shape = new Square(5, 10);
  private String[] tags;
  private int id;
  
  public String getName() {
    return this.getName();
  }
  
  public static class Square extends Shape {
    private final int width, height;
    
    public Square(int width, int height) {
      this.width = width;
      this.height = height;
    }
    
    @Override public String toString() {
      return "Square(super=" + super.toString() + ", width=" + this.width + ", height=" + this.height + ")";
    }
  }
  
  @Override public String toString() {
    return "ToStringExample(" + this.getName() + ", " + this.shape + ", " + Arrays.deepToString(this.tags) + ")";
  }
}
</pre></code>
## <center>Supported configuration keys:</center> ##
`lombok.toString.includeFieldNames` = [`true` | `false`] (default: true)
通常情况下, `lombok`会在`toString()`的响应中为每一个字段按照`fieldName = fieldValue`这种格式生成一个片段. 如果这个属性被设置`false`, `lombok`将会<br/>
忽略字段的名字, 只是简单呈现出一个被逗号分隔的字段值列表. `toString`注解的属性`includeFieldNames`, 如果被明确的指定, 将会比这个设置有更高的优先级.
`lombok.toString.doNotUseGetters` = [`true` | `false`] (default: false)
如果设置为`true`, `lombok`将会直接使用字段而不是`getters`方法(如果可以获得)在生成`toString()`方法时. `toString`注解的属性`doNotUseGetters`, 如果被明确的指定, 将会比这个设置有更高的优先级.
`lombok.toString.flagUsage` = [`warning` | `error`] (default: not set)
如果配置这个属性, `Lombok`将会把`@toString`的使用标记为warning或者error.
## <center>Small print</center> ##
如果存在名称为'toString', 无参数的方法, 无论返回值类型为何, 'lombok'将不会生成'toString'方法. 作为替代一条报警会被发出, 来说明'@ToString'注解什么都没做.你可以使用'@lombok.experimental.Tolerate'来标记任何方法来对'lombok'隐藏它.<br/>
数组(Arrays)被打印经由'Arrays.deepToString', 这意味着包含自身的数组会导致'StackOverflowError'.而且这一点对于'ArrayList'是一样的.<br/>
如果一个方法被标记为inclusion, 而且它的名字和某一成员变量相同, 它会替换这个变量在'toString'方法的输出中.<br/>
先前的'lombok 1.16.22', 'inclusion/exclusion'能够被完成利用'@ToString'的'of'和'exclude'.这种老式的'inclusion'机制仍然被支持但在未来会被弃用.<br/>
使用'@ToString.Exclude'和'@ToString.Include'同时作用于某个成员会生成一条告警; 这个成员会被弃用.<br/>
我们不保证在两个不同版本的'lombok'会生成相同的'toString'方法. 你的代码不要直接暴力解析'toString'的输出.<br/>
缺省状态下, 以'$'开始的变量会被自动的排除掉. 你只能通过使用'@ToString.Include'注解来包含他们.<br/>
如果某个字段存在getter, getter会被直接调用来替代直接使用字段引用. 这个行为会被抑制:'@ToString(doNotUseGetters = true)'<br/>
'@ToString' can also be used on an enum definition.

## 参考资料
https://projectlombok.org/features/ToString