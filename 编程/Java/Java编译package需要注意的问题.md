编译package时，

1、必须在源文件声明属于的包

2、必须有public类。

如果只声明属于的包，没有public类，命令行编译后没有反应，

如只有public类，没有声明属于的包，命令行编译后，所有源文件都只会在bin目录，不会有子目录

