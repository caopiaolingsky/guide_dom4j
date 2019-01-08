#### 享元模式简介

享元模式（Flyweight Pattern）是Java中一种非常实用的设计模式，它主要用于减少创建对象的数量，以减少内存占用和提高性能。

**意图：**

运用共享技术有效地支持大量细粒度的对象。

**动机：**

用于对数量庞大的实例对象进行建模，通过共享的flyweight对象来减少创建的对象的数量，以减少内存占用，提高程序运行性能。

**参与者：**

* Flyweight：描述一个外部接口，通过这个接口flyweight可以接受并作用于外部状态（或称为flyweight所处的场景）。

* ConcreteFlyweight：实现Flyweight接口，并且存储flyweight的内部状态，ConcreteFlyweight必须是共享的。

* UnsharedConcreteFlyweight：不被共享的Flyweight子类，其通常是ConcreteFlyweight独享的父节点。

* FlyweightFactory：创建并管理flyweight对象，确保合理地共享flyweight。当用户请求一个flyweight时，FlyweightFactory对象提供一个已创建的实例或者再创建一个（如果不存在的话）。

* Client：维持一个对flyweight的引用，计算或存储一个（多个）flyweight的外部状态。

#### Dom4j中享元模式的使用

Dom4j运行得非常快，其XML解析效率也非常高，是XML解析包中的佼佼者，这很大程度上得益于Dom4j使用了Flyweight模式。Dom4j使用享元模式是非常明智的。

在解析XML文档树时，由于是将整个文档树当作一个对象，每个节点都成为一个对象，如果在实际使用时将每个节点都单独实例化为一个对象，那么在XML文档很长时，就会出现危机，对内存的消耗巨大，最终使程序的性能严重较低。但根据实际情况来看，一个XML文档中，有很多的重复（严格地说是相似）节点，如Attribute属性节点、Text文本节点。由于这类节点经常重复地出现在不同的环境中，我们就可以使用共享对象（flyweight）来表示这些节点。共享对象中存有所有对象中的共性属性，它不用知道它所处的外部环境（场景）是怎样的。当要使用这个共享对象时，再由外部方法，一般是由client提供来计算当前flyiweight对象所处的环境并将之传给flyweight对象自身。

Dom4j中以下几类节点都实现了flyweight对象：

![](/assets/flyweights.png)

根据前面所说的Flyweight模式的要素，结合Attribute类节点，下面具体说明Attribute节点是如何实现flyweight对象的。

Flyweight类的实现非常简单，里面只包含属性节点可有的共享属性qname，value：

```java
public class FlyweightAttribute extends AbstractAttribute {
    /** The <code>QName</code> for this element */
    private QName qname;

    /** The value of the <code>Attribute</code> */
    protected String value;

    public FlyweightAttribute(QName qname) {
        this.qname = qname;
    }
...
```

为什说在创建Attribute属性节点时没有真的创建Attribute实例对象，而是使用的flyweight对象呢。因为在DocumentFactory的Attribute构造类里是这样调用的：

```java
    public Attribute createAttribute(Element owner, QName qname, String value) {
        return new DefaultAttribute(qname, value);
    }
```

可以看到虽然调用者仍认为创建一个属性节点时需要知道这个属性依附于那哪个element节点，但创建方法却忽略了这个参数（也就是使用者不用关心内部是否使用了flyweight对象，体现了封装隔离特性。

