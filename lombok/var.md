# <center>var</center> #
# <center>Mutably! Hassle-free local variables</center> #
`var`是在lombok 2.0.0 版本中被提升引入主包中的；`JEP 286`建立了期望，`lombok`使用`var`追踪`JEP 286`，我们决定推出`var`，即使它遗留了争议。</br>
`var`在`lombok`1.16.12中被介绍作为试验性质的feature。
## <center>Overview</center> ##
</br>&nbsp; &nbsp; &nbsp; `var`起的作用很像`val`，除了局部变量不会被标记为`final`。
</br>&nbsp; &nbsp; &nbsp; `local variable`类型仍然完全地取决于严格意义上的初始化表达式，和随后的语句，而且这合法(因为局部变量不再是`final`)，不能通过表面的肉眼查看来确定合适的类型。
</br>&nbsp; &nbsp; &nbsp; 举例，`var x = "Hello"; x = Color.RED;`这条语句不会工作；`x`的类型会被推断为`java.lang.String`，而且因此`x = Color.RED`语句会失败。
如果`x`的类型被推断为`java.lang.Object`这段代码就会被编译，但是这样`var`就没有作用了。
## <center>Supported configuration keys:</center> ##
`lombok.var.flagUsage` = [`warning` | `error`] (default: not set)</br>
`lombok`将会把对`var`的使用标记为警告或者错误如果配置。
## 参考资料
https://projectlombok.org/features/var