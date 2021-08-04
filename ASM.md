# ASM

1. Core API

   内存消耗小，编程复杂。基于ClassVisitor接口，接口的每个方法对应class文件每一项，有三个实现类：

   - ClassReader：实现类，解析类class字节码

   - ClassAdapter：实现类，改造功能

   - ClassWriter：实现类，输出变化后的字节码

   ASMMifier工具生成ASM结构进行对比。

2. Tree API

   内存消耗大，编程简单。