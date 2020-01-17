Builder（生成器）

1、意图

将一个复杂对象的构建与它的表示分离，使得同样的构建过程可以创建不同的表示

2、适用性

1. 当创建复杂对象的算法应该独立于该对象的组成部分以及他们的装配方式时
2. 当构造过程必须允许被构造的对象有不同表示时（**就是构造过程随对象的不同而变化**）

3、结构图

![builder]https://github.com/Mingming-Git/Design-Pattern-learning-note/blob/master/screenshot/builder.PNG

![1579231893366](C:\Users\limingming\AppData\Roaming\Typora\typora-user-images\1579231893366.png)

4、效果

1. 使你可以改变一个产品的内部表示（可以隐藏该产品的内部表示和结构，可见构造接口）
2. 将构造代码与表示代码分开（就是将创建过程的代码与该产品的内部代码分离）
3. 使你可以对构造过程进行更精细的控制（builder模式是一步一步构造产品，可以控制每一步的行为，与一下子构建的模式不同）

5、个人总结

​        builder模式与abstract factory很类似，其功能都是创建对象；但是不同之处在于：

1. builder着重于一步一步进行构建，而abstract factory着重于多个产品系列（如不同操作系统下的构建）。

2. builder是在所有步骤完成后返回产品，而abstract factory模式时立即生成。

   使用哪个模式进行产品创建，可以通过上述两点进行判断，两者的着重点不同。