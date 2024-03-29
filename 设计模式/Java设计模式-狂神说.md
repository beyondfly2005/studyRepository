### 什么是设计模式

- 设计模式（Design Pattern）是前人对代码开发经验的总结，是解决特定问题的一系列套路。它不俗是语法规定，而是一种套用来提高代码可复用性、可维护性、可读性、稳健性以及安全性的解决方案。
- 1995年，GoF（Gang of Four 四人组/四人帮）合作出版了《设计模式：可复用面向对象软件的基础》一书，工收录了23种设计模式，从此树立了软件设计模式领域的里程碑，人称[Gof设计模式]。这23种设计模式也简称GoF23。

### 学习设计模式的意义

- 设计模式的本质是面向对象设计原则的实际运用，是对类的封装性、继承性、多态性以及类和类的关联关系和组合关系的充分理解。

- 正确的使用设计模式具有以下优点：
  - 可以提高程序员的而思维能力、编程能力和设计能力
  - 使程序设计更加标准化、代码编制更加工程化，使软件开发效率大大提高，从而缩短软件的开发周期。
  - 使设计的代码可重用性高、刻度性强、可靠性高、灵活性好、可维护性强。

### 设计模式的基本要素

- 模式名称

  通常用一两个简单的词来描述，可以根据模式的问题、特点、解决方案、功能或者效果来命名；可以有效的帮助我们记忆设计的作用，也方便我们讨论自己的设计

- 问题

  描述了设计模式应用的环境，解释了设计问题和问题存在的前因后果，以及满足一系列的条件

- 解决方案

  上述问题的解决方案，其内容给出了设计的各个组成部分，它们之间的关系、职责划分和协作方式。

- 效果

  描述了模式应用的效果及使用模式应权衡的问题；设计模式的优缺点。



### Gof23 

- 一种思维 一种态度 一种进步



### 设计模式分类

- 创建性模式

  - 单例模式、工程模式、抽象工厂模式、建造者模式、原型模式、

- 结构型模式

  - 适配器模式、桥接模式、装饰模式、组合模式、外观模式、享元模式、代理模式

- 行为型模式

  模板方法模式、命令模式、迭代器模式、观察者模式、中介模式、备忘录模式、解释器模式、

  状态模式、策略模式、职责链模式、访问者模式

  

### 面向对象(OOP)七大原则

- 开闭原则：对扩展开发、对修改关闭
- 里氏替换原则：计策必须确保超类所又有的性质在子类中仍然成立
- 依赖倒置原则：要面向接口编程、不要面向实现编程
- 单一职责原则：控制类的粒度大小、讲对象解耦、提高其内聚性
- 接口隔离原则：要为哥哥类建立他们专用的接口
- 迪米特法则：支队医德直接朋友交谈，不跟“陌生人”说话
- 合成复用原则：尽量使用组合或者聚合等关联关系来实现，其次才考虑使用使用集成关系来实现。



### 单例模式

- 核心作用
  - 保证一个雷只有一个实例，并且提供一个访问该实例额全局访问点
- 常见场景
  - Window的只有管理器
  - Windows的回收站
  - 项目中、读取配置文件的雷，一般也只有一个对象，没必要每次去new对象读取
  - 网站的计数器也一般会采用单利模式，可以保证同步
  - 数据库连接池的设计一般也是单例模式
  - 在Servlet编程中，每个Servlet也是单例的
  - 在Spring中，每个Bean默认就是单例的
  - ... ... ...

 ### 工厂模式 

- 作用

  - 实现了创建者和调用者的分离
  - 详细分类
    - 简单工厂模式
    - 工厂方法模式
    - 抽象工厂模式

- OOP七大原则

  - 开闭原则
  - 依赖倒转原则
  - 迪米特法则：只与捏直接的朋友通信，

- 核心本质：

  - 实例化对象不使用new ，用工厂方法代替
  - 将选择实现类，创建对象同意管理和控制，从而将调用者和他们的实现类解耦。

- 三种模式：

  - 简单工厂模式

    - 用了生产同意登记结构中的任意产品（对于增加新的产品，需要球盖已有代码）

  - 工厂方法模式

    - 用了生产同一等级结构中的固定产品（支持增加任意产品）

  - 抽象工厂模式

    - 围绕一个超级工厂创建其他工厂，该超级工厂又称为其他工厂的工厂

      

      

  

  

  
