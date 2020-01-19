# Factory Method（工厂模式）

## 1、意图

定义一个用于创建对象的接口，让子类决定实例化哪一个类， 也叫virtualConstructor

## 2、适用性

1. 当一个类不知道它所必须创建的对象的类的时候
2. 当一个类希望又它的子类来指定它所创建的类的时候
3. 当类将创建对象的职责委托给多个帮助子类中的某一个，并且你希望将哪一个帮助子类是代理者这一信息局部化的时候

## 3、结构图

![builder](/master/screenshot/factorymethod.PNG)

## 4、效果

1. 工厂不再与任何特定应用有关的类绑定，代码仅会处理product接口
2. 缺点在于：为了创建一个特定的product对象，就需要创建一个对应的子类
3. 为子类可以提供挂钩(类似于挂钩)，就是在类的内部进行创建，而不是直接创建
4. 连接平行类层次：例如图形交互操作时，旋转、拉长等操作产生不同的行为

## 5、个人总结 

1. abstract factory经常使用factory method进行实现。

2. factory method通常在Template method中进行调用。

   对于abstract factory与factory method的很难区分，网上也有很多讲解，此处标记一个比较好的答案(https://stackoverflow.com/questions/5739611/what-are-the-differences-between-abstract-factory-and-factory-design-patterns)：

   The main difference between a "factory method" and an "abstract factory" is that the factory method is a single method, and an abstract factory is an object。

   对于factory method其实仅仅是提供了一个方法去创建产品，而abstract factory是创建一个对象。代码例子：

    here is a factory method in use:

   ```java
   class A {
       public void doSomething() {
           Foo f = makeFoo();
           f.whatever();   
       }
   
       protected Foo makeFoo() {
           return new RegularFoo();
       }
   }
   
   class B extends A {
       protected Foo makeFoo() {
           //subclass is overriding the factory method 
           //to return something different
           return new SpecialFoo();
       }
   }
   ```

   And here is an abstract factory in use:

   ```java
   class A {
       private Factory factory;
   
       public A(Factory factory) {
           this.factory = factory;
       }
   
       public void doSomething() {
           //The concrete class of "f" depends on the concrete class
           //of the factory passed into the constructor. If you provide a
           //different factory, you get a different Foo object.
           Foo f = factory.makeFoo();
           f.whatever();
       }
   }
   
   interface Factory {
       Foo makeFoo();
       Bar makeBar();
       Aycufcn makeAmbiguousYetCommonlyUsedFakeClassName();
   }
   
   //need to make concrete factories that implement the "Factory" interface here
   ```

注意看其中的注释， 第一个例子中factory method，其实仅仅通过子类的来重新创建产品的实现；而对于第二个代码abstract factory是整个object不同，具体的完全取决于当前类的实例化。

