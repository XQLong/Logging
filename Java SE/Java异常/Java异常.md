Java把所有的非正常情况分为了两种：异常（Exception）和错误（Error），它们都继承了Throwable类。

Error错误，一般是指与虚拟机相关的问题，如系统崩溃、虚拟机错误、动态链接失败等，这种错误无法恢复或不可能捕获，将导致应用程序中断。通常应用程序无法处理这些错误，因此应用程序不应该试图使用catch块来捕获Error对象。

Java将异常分为两种，Checked异常和Runtime异常，Java认为Checked异常都是可以在编译阶段被处理的异常，所以它强制程序处理所有的Checked异常，而Runtime异常则无需处理。


![title](https://raw.githubusercontent.com/XQLong/Image-Hosting/master/gitnote/2019/08/28/1566997803792-1566997803801.png)