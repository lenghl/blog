# <center>@Getter and @Setter</center> #
# <center>Never write public int getFoo() {return foo;} again.</center> #
## <center>Overview</center> ##
</br>&nbsp; &nbsp; &nbsp; 你可以使用`@Getter`和(或)`@Setter`注释任何成员变量，去使`lombok`自动生成缺省的`getter/setter`。缺省的getter简单地返回所属的成员变量，方法名字会是`getFoo`如果所属的成员变量名字为`foo`(或者方法名字会是`isFoo`如果所属的成员变量名字为`foo`且类型是`boolean`)。缺省的`setter`方法名字会是`setFoo`如果所属的成员变量名字为`foo`，返回`void`，并有一个和所属成员变量类型相同的入参。它简单地设置所属成员变量的值。
</br>&nbsp; &nbsp; &nbsp; 生成的`getter/setter`方法缺省为`public`，这个可以通过设置`AccessLevel`来改变，像下面例子所展示的那样。合法的等级有`PUBLIC`， `PROTECTED`，`PACKAGE`和 `PRIVATE`。
</br>&nbsp; &nbsp; &nbsp; 你也可以使用`@Getter`和(或)`@Setter`注释一个类，这种情况相当于你使用此注解注释此类中的所有非静态成员变量。
</br>&nbsp; &nbsp; &nbsp; 你总可以手动的中止任何一个成员变量`getter/setter`的生成通过使用指定的`AccessLevel.NONE`等级。这回允许你覆盖修饰类的`@Getter`，`@Setter` 或 `@Data`注解的行为。
</br>&nbsp; &nbsp; &nbsp; 你可以使用`onMethod=@__({@AnnotationsHere})`来放置注解于被生成的方法上；你可以使用`onParam=@__({@AnnotationsHere})`来放置注解于被生成`setter`方法唯一的参数上。不过要小心！这是一个试验性质的`featur`。更多相关信息请看文档[onX](https://projectlombok.org/features/experimental/onX)
</br>&nbsp; &nbsp; &nbsp; `lombok v1.12.0`中的新特征：成员变量上的javadoc会被复制到被生成的`getter/setter`上。通常情况下，所有的文档都会被复制，`@return`被移动到`getter`上，同时`@param`被移动到`setter`。Moved means: Deleted from the field's javadoc. 可以为每一个`getter/setter`定制唯一的内容。你需要创建一个被命名为`GETTER`和(或)`SETTER`的`section`。一个`section`就是`javadoc`中的一行，而且包含2个或多个破折号，`GETTER`或`SETTER`只被2个或多个破折号跟随。如果你使用`sections`，`@return`和`@param` stripping for that section is no longer done (move the @return or @param line into the section).
## <center>With Lombok</center> ##
<pre><code>
import lombok.AccessLevel;
import lombok.Getter;
import lombok.Setter;

public class GetterSetterExample {
  /**
   * Age of the person. Water is wet.
   * 
   * @param age New value for this person's age. Sky is blue.
   * @return The current value of this person's age. Circles are round.
   */
  @Getter @Setter private int age = 10;
  
  /**
   * Name of the person.
   * -- SETTER --
   * Changes the name of this person.
   * 
   * @param name The new value.
   */
  @Setter(AccessLevel.PROTECTED) private String name;
  
  @Override public String toString() {
    return String.format("%s (age: %d)", name, age);
  }
}
</pre></code>
## <center>Vanilla Java</center> ##
<pre><code>
public class GetterSetterExample {
  /**
   * Age of the person. Water is wet.
   */
  private int age = 10;

  /**
   * Name of the person.
   */
  private String name;
  
  @Override public String toString() {
    return String.format("%s (age: %d)", name, age);
  }
  
  /**
   * Age of the person. Water is wet.
   *
   * @return The current value of this person's age. Circles are round.
   */
  public int getAge() {
    return age;
  }
  
  /**
   * Age of the person. Water is wet.
   *
   * @param age New value for this person's age. Sky is blue.
   */
  public void setAge(int age) {
    this.age = age;
  }
  
  /**
   * Changes the name of this person.
   *
   * @param name The new value.
   */
  protected void setName(String name) {
    this.name = name;
  }
}
</pre></code>
## <center>Supported configuration keys:</center> ##
`lombok.accessors.chain` = [`true` | `false`] (default: false)</br>
如果设置行为`true`，生成的`setters`将会返回`this`(而不是`void`)。`@Accessors`注释的显式配置链参数优先于此设置。</br>
`lombok.accessors.fluent` = [`true` | `false`] (default: false)</br>
如果设置为`true`，生成的`getters`和`setters`将不会被加前缀(`get`，`is`，`set`)；做为替代，方法的名字将会和字段相同，`@Accessors`注释的显式配置链参数优先于此设置。</br>
`lombok.accessors.prefix` += a field prefix (default: empty list)</br>
这是一个列表属性；例表中的条目可以通过`+=`来添加。从父配置文件继承而来的条目可以通过`-=`来删除。`lombok`将会去除字段名字中匹配到的前缀，来确定生成的`getter/setter`方法的名字。例如：如果`m`是这个列表配置中的一员，一个字段的名字为`mFoobar`，那么`getter`的名字将会是`getFoobar`，而不是`getMFoobar`。`@Accessors`注释的显式配置链参数优先于此设置。</br>
`lombok.getter.noIsPrefix` = [`true` | `false`] (default: false)</br>
如果设置成为`true`，为`boolean`字段生成的`getters`将会使用`get`前缀而不是缺省的`is`前缀，而且任何生成的且调用了`getter`的代码，比如`@ToString`，将会使用`get`代替`is`。</br>
`lombok.setter.flagUsage` = [`warning` | `error`] (default: not set)</br>
`Lombok` will flag any usage of `@Setter` as a `warning` or `error` if configured.</br>
`lombok.getter.flagUsage` = [`warning` | `error`] (default: not set)</br>
`Lombok` will flag any usage of `@Getter` as a `warning` or `error` if configured.</br>


## <center>Small print</center> ##
对于生成方法的名字，如果字段的首字母是小写，则会变为大写，否则不变，然后`get/set/is`被加为前缀。</br>
如果存在相同名字且相同参数个数的方法，则不会有方法生成。例如：`getFoo()`不会被生成如果`getFoo(String... x)`存在即使技术上可以生成这个方法。
这个告警存在去阻止混乱。如果某个方法的生成跳过这个规则，一个报警将会抛出。可变参数的个数为0到N。你可以使用`@lombok.experimental.Tolerate`标注任何方法，来使得它无法被`lombok`察觉。</br>
对于`boolean`的，且以后面跟随大写字母的`is`开头的字段，其`getter`方法不会加任何前缀。</br>
Any variation on boolean will not result in using the is prefix instead of the get prefix; for example, returning java.lang.Boolean results in a get prefix, not an is prefix.</br>
任何修饰字段且名字为`@NonNull`的注解都会被理解为：这个字段不会为`null`。因此，这些注解会导致`setter`中存在空指针检查。而且，这些注释会被复制到`setter`参数上，`getter`方法上。</br>
你可以注释一个类使用`@Getter`或`@Setter`注解。这等同于直接注释这个类中的所有非静态字段。字段上面的`@Getter/@Setter`注解的优先级高于类上面的。</br>
使用`AccessLevel.NONE`，什么都不会生成。它仅仅应该和`@Data`或修饰类的`@Getter/@Setter`一起使用。
`@Getter`可以被用于枚举。`@Setter`不可以，不是技术原因，只是这样做不好。
## 参考资料
https://projectlombok.org/features/GetterSetter