# <center>@Cleanup</center> #
# <center>Automatic resource management: Call your close() methods safely with no hassle.</center> #
`@NonNull`是在lombok v0.11.10 版本中被引入的。</br>
## <center>Overview</center> ##
</br>&nbsp; &nbsp; &nbsp; 你可以使用`@Cleanup`注解去确保一件事：在代码执行指针走出当前作用域之前资源实例被自动清理掉。为达到上面所述，需要使用`@Cleanup`去注释局部变量的声明，例如：`@Cleanup InputStream in = new FileInputStream("some/file");`
</br>&nbsp; &nbsp; &nbsp; 上述操作的影响结果，在你当前作用域的结尾，`in.close()`被调用。这个调用通过一个try/finally代码块来保证运行。看下面的样例来了解如何工作。
</br>&nbsp; &nbsp; &nbsp; 如果你将要清理的实例所属类型没有`close()`方法。但是有其他无参数方法，你可以指定这个方法的名字像下面这样：`@Cleanup("dispose") org.eclipse.swt.widgets.CoolBar bar = new CoolBar(parent, 0);`
</br>&nbsp; &nbsp; &nbsp; 缺省条件下，清理方法被假定为`close()`。含有入参的清理方法是不能被`@Cleanup`调用的。
## <center>With Lombok</center> ##
<pre><code>
import lombok.Cleanup;
import java.io.*;

public class CleanupExample {
  public static void main(String[] args) throws IOException {
    @Cleanup InputStream in = new FileInputStream(args[0]);
    @Cleanup OutputStream out = new FileOutputStream(args[1]);
    byte[] b = new byte[10000];
    while (true) {
      int r = in.read(b);
      if (r == -1) break;
      out.write(b, 0, r);
    }
  }
}
</pre></code>
## <center>Vanilla Java</center> ##
<pre><code>
import java.io.*;

public class CleanupExample {
  public static void main(String[] args) throws IOException {
    InputStream in = new FileInputStream(args[0]);
    try {
      OutputStream out = new FileOutputStream(args[1]);
      try {
        byte[] b = new byte[10000];
        while (true) {
          int r = in.read(b);
          if (r == -1) break;
          out.write(b, 0, r);
        }
      } finally {
        if (out != null) {
          out.close();
        }
      }
    } finally {
      if (in != null) {
        in.close();
      }
    }
  }
}
</pre></code>
## <center>Supported configuration keys:</center> ##
`lombok.cleanup.flagUsage` = [`warning` | `error`] (default: not set)</br>
`lombok`将会把对`@Cleanup`的使用标记为警告或者错误如果配置。
## <center>Small print</center> ##
在finally代码块里，给定的资源实例不为空时，清理方法才会被调用。然而，如果你使用`delombok`在代码上，a call to `lombok.Lombok.preventNullAnalysis(Object o)` is inserted to prevent warnings if static code analysis could determine that a null-check would not be needed. Compilation with `lombok.jar` on the classpath removes that method call, so there is no runtime dependency.</br>
如果你的代码抛出一个异常，清理方法被触发同时它也抛出一个异常，这时前面的异常会被清理方法的异常隐藏掉。对于上述特点，你不应该依赖它。相对于前者更好的的方案：如果主代码抛出异常，任何清理方法抛出的异常都应该被隐藏(如果主代码结束，清理方法的异常不被隐藏)。`lombok`的实现者们当前没有好的方法来实现这个设计，如果`java`本身的更新允许或者找到好的方法，实现者们定会更新。</br>
清理方法抛出的异常你仍然需要处理。
## 参考资料
https://projectlombok.org/features/NonNull