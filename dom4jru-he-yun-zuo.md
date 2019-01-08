#### 核心类概览

关于XML文档对象描述的核心类都在dom4j\tree下面。通过使用上一节所讲的各个接口进行规范和约束，可以很方便地构造出各个节点的类。下面先给出XML文档树节点类的继承关系图：

![](/assets/cclasstree.png)从这个类的关系图中可以大致看到，Dom4j首先是定义了各个节点的抽象类，这些抽象类都实现了相应节点的接口。再由一个DefaultNodeName的具体类继承相应节点的抽象类来实现对具体节点的定义。

#### Node抽象类（AbstractNode.java）

同Node接口是所有节点接口的原型一样，Node抽象类AbstractNode也是所有抽象节点的基类。其部分代码如下：

```java
public abstract class AbstractNode implements Node, Cloneable, Serializable {
    protected static final String[] NODE_TYPE_NAMES = {"Node", "Element",
            "Attribute", "Text", "CDATA", "Entity", "Entity",
            "ProcessingInstruction", "Comment", "Document", "DocumentType",
            "DocumentFragment", "Notation", "Namespace", "Unknown" };

    /** The <code>DocumentFactory</code> instance used by default */
    private static final DocumentFactory DOCUMENT_FACTORY = DocumentFactory
            .getInstance();

    public AbstractNode() {
    }

    public short getNodeType() {
        return UNKNOWN_NODE;
    }

    public String getNodeTypeName() {
        int type = getNodeType();

        if ((type < 0) || (type >= NODE_TYPE_NAMES.length)) {
            return "Unknown";
        }

        return NODE_TYPE_NAMES[type];
    }

    public Document getDocument() {
        Element element = getParent();

        return (element != null) ? element.getDocument() : null;
    }

    public void setDocument(Document document) {
    }

    public Element getParent() {
        return null;
    }

    public void setParent(Element parent) {
    }

    public boolean supportsParent() {
        return false;
    }

    public boolean isReadOnly() {
        return true;
    }
```

在Node抽象类里，对节点的共同行为进行了一些实现。代码比较简单，这里就不解释了。

#### 其他抽象类

对应的还有AbstractCharacterData、AbstractComment、AbstractBranch、AbstractElenmen类，它们都是对相应节点的接口进一步进行实现，以便逐渐描述出对应节点的类。它们之间的关系同对应接口的关系一样，这里就不再赘述了。请看面的接口关系分析部分。通过这些类，Dom4j基本完成了对XML文档树所有节点对象的建模，其他也就简单了。

