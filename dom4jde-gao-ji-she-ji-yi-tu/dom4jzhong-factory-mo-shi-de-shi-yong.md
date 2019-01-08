#### 工厂模式简介

工厂模式（Factory Pattern）是 Java 中最常用的设计模式之一。这种类型的设计模式属于创建型模式，它提供了一种创建对象的最佳方式。通俗的说，工厂模式就是创建了一个作用类似于工厂的类（creator\)，这个工厂类提供生产产品的方法，调用工厂方法的人可以通过产生指定具体生产什么样的产品，生产产品的工作就交由相应的帮助子类（ConcreteCreator）来负责。

**意图：**

定义一个创建对象的接口，让其子类自己决定实例化哪一个工厂类，Factory Method方法使一个类的实例化延迟到子类进行。

**动机：**

框架使用抽象类定义和维护对象之间的关系。这些对象的创建通常也由框架负责。

**适用性：**

* 当一个类不知道它所必须创建的对象的类的时候
* 当一个类希望由它的子类来指定它所创建的对象的时候
* 当类将创建对象的职责委托给多个帮助子类中的某一个，并且希望将”哪一个帮助子类是代理者“这一信息局部化的时候。

**参与者：**

* Product：定义工厂方法所创建的对象的接口
* ConcreteProduct：工厂需要创建的具体实例，它实现了Product接口
* Creator：工厂类
* ConcreteCreator：创建具体实例的帮助子类

#### 工厂模式在Dom4j中的一处实现

Dom4j中有多个工厂类，其中一个是DocumentFactory（DocumentFactory.java）类，其部分代码如下：

```java
public class DocumentFactory implements Serializable {
   ...
   
    // Factory methods
    public Document createDocument() {
        DefaultDocument answer = new DefaultDocument();
        answer.setDocumentFactory(this);

        return answer;
    }

    public Document createDocument(String encoding) {
        Document answer = createDocument();

        answer.setXMLEncoding(encoding);
        return answer;
    }

    public Document createDocument(Element rootElement) {
        Document answer = createDocument();
        answer.setRootElement(rootElement);

        return answer;
    }

    public DocumentType createDocType(String name, String publicId,
            String systemId) {
        return new DefaultDocumentType(name, publicId, systemId);
    }

    public Element createElement(QName qname) {
        return new DefaultElement(qname);
    }

    public Element createElement(String name) {
        return createElement(createQName(name));
    }

    public Attribute createAttribute(Element owner, QName qname, String value) {
        return new DefaultAttribute(qname, value);
    }
    ...
```



