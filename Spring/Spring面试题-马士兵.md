#### > 课程地址

> https://www.bilibili.com/video/BV1Av411L7Rb/?spm_id_from=333.788.videocard.2

答题技巧：

​	总：当前问题回答的哪些具体的电

​	分：以1,2,3,4,5的方式分细节描述相关的知识点，不清楚的点 忽略掉

突出一些一些技术名称（核心概念、接口、类、关键方法）

#### 1、谈谈你的SpringIOC的理解，原理与实现？

​	控制反转

​	容器 - Spring容器，存储对象，使用map结构来存储，Spring中一般存在三级缓存，SingletonObject完成属性值的注入整个Bean的声明周期，从创建到使用到销毁的过程全部都是由容器来管理

​	Bean的声明周期：

​	DI 依赖注入，将对应的属性值注入到具体的对象中，

​				@Autowired

​				populateBen 完成属性值的注入



#### 7、Spring中用到的设计模式

​	单例模式：Spring的bean默认都是单例模式

​	原型模式：指定作用于为protptype

​	工厂模式：BeanFactory

​	模板方法模式：postProcessBeanFactory  onRefresh  initPropertyValue

​	策略模式：XmlBeanFinitionReader PropertiesBeanDefinitionReader

​	观察者模式：listener event  multicast

​	适配器模式：Adapter：

 

#### 8、Redis是单线程还是多线程

​	无论什么版本，工作线程都是一个

​	高版本6.x出现了IO多线程

​	延伸：[学IO课]  面向IO模型编程的时候，有内核的事，从内核把数据搬运到程序里这是第一步，然后搬运回来的数据做得计算是第二步，netty



#### 9、Redis存在线程安全吗？为什么





#### 10、Redis是如何回收的

​	如何删除过期的Key（缓存是如何回收的）

​		1、缓存在轮询，分段分批的删除那些过期的key

   	2、请求的时候判断时  已经过期了

缓存是如何淘汰的

