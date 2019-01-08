#### 享元模式简介

享元模式（Flyweight Pattern）是Java中一种非常实用的设计模式，它主要用于减少创建对象的数量，以减少内存占用和提高性能。

**意图：**

运用共享技术有效地支持大量细粒度的对象。

**动机：**

用于对数量庞大的实例对象进行建模，通过共享的flyweight对象来减少创建的对象的数量，以减少内存占用，提高程序运行性能。

**参与者：**

Flyweight：描述一个外部接口，通过这个接口flyweight可以接受并作用于外部状态（或称为flyweight所处的场景）。

ConcreteFlyweight：实现Flyweight接口，并且存储flyweight的内部状态，ConcreteFlyweight必须是共享的。

UnsharedConcreteFlyweight：不被共享的Flyweight子类，其通常是ConcreteFlyweight独享的父节点。

FlyweightFactory：创建并管理flyweight对象，确保合理地共享flyweight。当用户请求一个flyweight时，FlyweightFactory对象提供一个已创建的实例或者再创建一个（如果不存在的话）。

Client：维持

