# ProtoType（原型模式）

## 1、意图

用原型实例指定创建对象的种类，并且通过拷贝这些原型创建新的对象。

## 2、适用性

1. 当要实例化的类是在运行时刻指定的时候；
2. 为了避免创建一个与产品类层次平行的工厂类层次（如factory method每次都需要对一个产品类创建一个creator）；
3. 当一个类的实例只能有几个不同状态组合中的一种时；

## 3、结构图

![builder](/screenshot/prototype.PNG)

## 4、效果

1. 对客户隐藏了具体的产品类（与abstract factory、builder类似）
2. 客户无需改变即可使用与特定应用相关的类（与abstract factory 、builder类似）
3. 运行时刻可以动态增加和删除产品：只需要向原型管理器注册即可，一个新的具体产品类就会并入系统；
4. 可以改变值以指定新的对象：可以通过为一个对象变量指定值，就可以定义新的行为，同时不需要定义新的类；
5. 可以改变结构以指定新对象：可以将一个子部件进行深拷贝来作为一个原型，该原型就可以生成其他对象；
6. 可以减少子类的构造：protoType模式是克隆一个原型，而不需要请求一个工厂方法来产生一个对象（工厂方法就是一个新的子类）；
7. 可以用类动态配置应用：允许在运行时刻将类装载到应用中；
8. 缺点在于：每一个prototype的子类都必须实现clone操作（若内部有不支持拷贝或循环引用的对象时，克隆会很困难）。

## 5、特别注意

**prototype模式需要每一个类都实现clone操作**，但是此处的clone操作需要区分浅拷贝和深拷贝：

​        从下面这幅图可以很容易的区别出浅拷贝和深拷贝，当在进行浅拷贝时，指针还是指向原来Value地址中的值，当进行深拷贝时，系统会把原来Value的地址即所包含的值都进行复制。

![clone](/screenshot/prototype_clone.PNG)

![img](https://images.cnblogs.com/cnblogs_com/feipeng/Pic014.jpg)

​                                                               （此图来自网上）

**浅克隆**：创建一个新对象，新对象的属性和原来对象完全相同，对于非基本类型属性，仍指向原有属性所指向的对象的内存地址。
**深克隆**：创建一个新对象，属性中引用的其他对象也会被克隆，不再指向原有对象地址。

## 6、个人总结 

1. abstract factory与prototype在某些方面是互相竞争的，但是也可以一起使用（如abstract factory可以存储一个被克隆的原型的集合，并返回产品对象）。

2. 原型模式与abstract factory的主要区别在于：

   1. abstract factory创建新的产品时都需要创建一个新类（creator或子类），然后用new的方法实现； 而prototype则不需要创建新的类，仅通过复制原有的类，并改变其内部参数即可生成新的产品。使用clone的好处在于clone操作的效率要比new高（省去构造等），同时可以减少类的数目。

   2. abstract factory主要强调的是**一个系列的变化**，而prototype强调的是**对象的变化**：

      abstract factory如下：

      ```c++
      class AbstractFactory
      {
       public:
          virtual CarFactory* CreateCarFactory();
          virtual TankFactory* CreateTankFactory();
          virtual PlaneFactory* CreatePlaneFactory();
      }
      ```

      该模式主要是强调一系列产品，如有创建car 、tank、plane的一系列工厂。它主要应对的是新系列的需求变动，而难以应对新对象。**在新增一个系列产品时，采用abstract factory会更好**。

      prototype如下：

      ```cpp
      // "Prototype"
      abstract class Prototype
      {
        // Fields
        private string id;
        // Constructors
        public Prototype( string id )
        {
          this.id = id;
        }
        public string Id
        {
          get{ return id; }
        }
        // Methods
        abstract public Prototype Clone();
      }
      
      // "ConcretePrototype1"
      class ConcretePrototype1 : Prototype
      {
        // Constructors
        public ConcretePrototype1( string id ) : base ( id ) {}
      
        // Methods
        override public Prototype Clone() {
          // Shallow copy
          return (Prototype)this.MemberwiseClone();
        }
      }
      
      // "ConcretePrototype2"
      class ConcretePrototype2 : Prototype
      {
        // Constructors
        public ConcretePrototype2( string id ) : base ( id ) {}
      
        // Methods
        override public Prototype Clone() {
          // Shallow copy
          return (Prototype)this.MemberwiseClone();
        }
      }
      
      /// <summary>
      /// Client test
      /// </summary>
      class Client
      {
        public static void Main( string[] args )
        {
          // Create two instances and clone each
          ConcretePrototype1 p1 = new ConcretePrototype1( "I" );
          ConcretePrototype1 c1 = (ConcretePrototype1)p1.Clone();
          Console.WriteLine( "Cloned: {0}", c1.Id );
      
          ConcretePrototype2 p2 = new ConcretePrototype2( "II" );
          ConcretePrototype2 c2 = (ConcretePrototype2)p2.Clone();
          Console.WriteLine( "Cloned: {0}", c2.Id );
        }
      }
      ```

      prototype主要是强调大量重复对象生成的情况，如上面代码中的原型，**在新增一个原型（而不是一个系列产品时）时，该方法有较好的隔离性与新增性**。

3. prototype虽然可以通过拷贝原型来生成新的产品，但是其clone操作要特别注意是深拷贝还是浅拷贝，例如：

   拷贝用户清单时应该用浅拷贝，而不需要拷贝对应的用户信息；这样在改变一个用户的信息时，同样另一个清单也会自动改变。

   而对于复制一个新的楼盘时，就需要深拷贝，因为需要全新的房间信息以及数据，两者相互独立。

   总体来说，采用深拷贝还是浅拷贝，取决于是否需要维护拷贝前后的数据一致性，两者若完全独立则应该深拷贝。
