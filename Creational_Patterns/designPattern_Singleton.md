# SingleTon（单件模式）

## 1、意图

保证一个类仅有一个实例，并提供一个访问它的全局访问点。

## 2、适用性

1. 当类只能有一个实例而且客户可以从一个全局的访问点访问它；
2. 当这个类唯一实例应该是通过子类化可扩展的，并且客户无需更改代码就能使用一个扩展实例的时候（就是子类可以通过相同方式进行扩展，但需要进行注册或在实例化时ifelse进行选择）；

## 3、结构图

![singleton](/screenshot/singleton.PNG)

## 4、效果

1. 对唯一实例的访问进行控制，仅能通过访问点访问，保证唯一性；
2. 缩小了namespace，这是对全局变量的一种改进（全局变量很容易污染namespace）；
3. 允许对操作和表示进行精化（很容易进行扩展子类）；
4. 允许可变数据的实例（？）；
5. 比类操作更加灵活（类操作只能通过静态成员函数的方式，但该方式无法改变一个实例，也无法通过子类进行重定义）；

## 5、个人总结 

1. singleton基本的实现方式为：

   ```cpp
   singleton *singleton::_instance = 0;
   
   singleton* singleton::Instance() {
       if (_instance == 0) {
           _instance = new singleton;
       }
       return _instance;
   }
   ```

   客户可以通过Instance方法获取该实例；

   该方法获取实例指针时的**if判断条件，是为了避免多个实例，保证了唯一的实例**；

2. singleton允许有多个子类实例化：

   ```cpp
   MazeFactory* MazeFactory::Instance {
       if (_instance == 0） {
           const char *mazeStyle = MazeStyle("name");
           
           if (mazeStype == bomed_maze) {
               _instance = new BombedMazeFactory;
           } else if (mazeStyle == enchant_maze) {
               _instance = new EnchantMazeFactory;
           //...other subclasses
           } else {
               _instance = new MazeFactory;
           }
       }
       return _instance;
   }
   ```

   可以定义新的子类实例，并统一一个实例化接口；同时保证子类的实例化唯一。

   但每增加一个子类实例，Instance接口都必须对应修改；另一种对该问题的修改时通过注册表的方式，通过注册的方式将子类实例化挂载至当前接口。

   ```cpp
   class singleton {
       public:
           static void register(char *, singleton *);
           static singleton *Instance();
       protected:
           static singleton * Lookup(char *);
       private:
           static singleton *_instance;
   }
   
   singleton *singleton::_instance = 0;
   
   singleton* singleton::Instance() {
       if (_instance == 0) {
           const char *mazeStyle = MazeStyle("name");
           
           _instance = Lookup(mazeStyle);
       }
       return _instance;
   }
   ```

3. 很多模式都是使用singleton来实现的（abstract factory、builder、prototype）；
