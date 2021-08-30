JAR文件

Java归档 Java Archive 是一种软件包文件格式

通常用于聚合大量的java类文件、相关的元数据和资源（文本、图片等）到一个文件，以便开发java平台应用软件或库

以zip格式构建，.jar为扩展名，文件名是Unicode文本

与zip不同的是不仅用于压缩和发布，还用于部署和封装库、组件、插件等，并可被编译器、JVM等工具直接使用。内部包含特殊的文件，如mainfests和部署描述符等文件来指定工具如何处理特定的JAR

特点：

1.安全性 可以加上数字化签名，这样能够识别签名的工具就能有选择地授予软件安全特权，还可以检测代码是否被篡改过

2.减少下载时间 在一个HTTP事务中下载而不是每个文件都打开一个连接

3.压缩 允许压缩文件以提高存储效率

4.传输平台扩展 Java扩展框架 Java Extensions Framework提供了向Java核心平台添加功能的方法，这些方法是由JAR文件打包的，例如Java 3D和JavaMail就是Sun开发的例子

5.包密封 密封一个包意味着包中所有的类都必须在同一个JAR文件中找到，以增强版本一致性和安全性

6.可移植性 处理JAR文件的机制是Java平台核心API的标准部分

创建

假设主类是com.myCompany.myapp.Sample

先创建一个名为manifest的文件，在里面加一行Main-Class: com.myCompany.myapp.Sample//结尾回车

然后执行 jar cmf manifest ExecutableJar.jar application-dir

注意，一个可执行的JAR必须通过manifest文件的头引用它所需要的其他从属JAR

运行

java -jar ExecutableJar.jar执行

META-INF目录

该目录用于存储包和扩展的配置数据，如安全性和版本信息，如

MANIFEST.MF 定义与扩展和包相关的数据

INDEX.LIST 由jar工具的-i选项生成，包含包的位置信息，是JarIndex实现的一部分，并由类装载器用于加速类装载的过程

xxx.SF 签名文件，占位符xxx标识了签名者

xxx.DSA 与签名文件相关的签名程序块文件，存储了用于签名JAR文件的公共签名



![image-20210809104149651](C:\Users\rickyrqzhao\AppData\Roaming\Typora\typora-user-images\image-20210809104149651.png)