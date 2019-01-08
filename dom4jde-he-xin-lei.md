如果想要了解Dom4j的大致原理，我想，搞清楚Dom4j的一些列接口应该是第一步。Dom4j是面向对象实现的，它同时也大量使用了接口的技术，这给Dom4j带来了非常大的灵活性。

#### 用接口描述对象

Dom4j既然是采用面向对象的编程哲学来解决XML的解析问题的，那么Dom4j的首要工作就是建立起XML文档树的抽象对象模型。这样说的理由我前面已经说过，XML文件的内容会形成一个XML文档树结构，而每个元素、属性、文本、注释等都成为相应的节点，对XML文档的DOM解析操作就是对这棵抽象文档树上各个节点的作用。Dom4j为了建立XML文档树对象，首先它定义了一系列节点接口，用这些接口来约束规范每个节点对象所应该有的属性和方法。

#### Node接口（Node.java）

Node接口是一个节点接口原型，XML文档树上所有的节点接口都继承自Node接口，如下图所示：

![](/assets/nodetree1.png)

Node接口的部分实现代码如下：

```java
public interface Node extends Cloneable {
    // W3C DOM complient node type codes

    /** Matches Element nodes */
    short ANY_NODE = 0;

    /** Matches Element nodes */
    short ELEMENT_NODE = 1;

    /** Matches elements nodes */
    short ATTRIBUTE_NODE = 2;

    /** Matches elements nodes */
    short TEXT_NODE = 3;
    ...
     * @return true if this node supports the parent relationship or false it is
     *         not supported
     */
    boolean supportsParent();

    Element getParent();

    void setParent(Element parent);

    Document getDocument();

    void setDocument(Document document);

    boolean isReadOnly();

    boolean hasContent();

    String getName();
    ...
```

可以看到Node接口继承了Java.lang包中的Cloneable接口，使得所有节点支持继承自Object类的克隆方法clone\(\)，因为显然文档树中的节点都是可复制的。紧接着，在Node接口内部定义了一些所有节点的共性行为。

* **节点类型常量挂载**

在Dom4j中节点类型由short型常量表示，例如ANY\_NODE代表任意节点，ELEMENT\_NODE代表元素节点，ATTRIBUTE\_NODE代表属性节点，TEXT\_NODE代表文本节点。在Dom4j中一共定义了14中节点，当然其中部分节点还没有被实现，但预留了位置。同时，使用UNKNOWN\_NODE代表未知节点，它不属于XML文档树中的节点类型。

Dom4j中实现的节点类型有：

`ANY_NODE`：代表任意节点

`ELEMENT_NODE`：代表元素节点

`ATTRIBUTE_NODE`：代表属性节点

`TEXT_NODE`：代表文本节点

`CDATA_SECTION_NODE`：代表CDATA域节点，CDATA是一种特殊节点，它里面的内容不会被解析

`ENTITY_REFERNCE_NODE`：代表一个实体引用节点

`PROCESSING_INSTRUCTION_NODE`：代表一个处理指令节点

`COMMENT_NODE`：代表一个注释节点，例如&lt;--我是注释--!&gt;

`DOCUMENT_TYPE_NODE`：代表一个Document type型节点，即文档类型声明节点

`NAMESPACE_NODE`：名字空间节点

* **节点共性行为定义**

`supportsParent()`方法：调用该方法可以知道当前节点是否支持向父级节点索引访问；

`getParent()`方法：获取节点的父节点，如果该节点是root节点或不支持父级节点索引，就返回null；

`setParent()`方法：指定当前节点的父类节点

`getDocument()`方法：获取当前节点所在的文档节点

`isReadOnly()`方法：判断当前节点是否是只读、不可更改的

`hasContent()`方法：判断当前节点是否是Branch型节点，即它是否有子节点

`getName()`方法：返回当前节点的名字

...

#### CharacterData接口（Character.java）

CharacterData接口继承自Node接口，它代表XML文档树中所有的叶子节点，即不存在子节点的节点类型：

![](/assets/cdtree.png)

可以看到，CDATA节点、Comment节点、Text节点的接口都继承自Charcater接口。因为CDATA节点域内部的内容是不允许被解析的，自然它也就不可能含有子节点了；Comment是注释节点，形如

`<--我是注释节点--!>`，它里面也是不可能有子节点的；Text节点是文本节点，如

`<title>I'm Text node except title</title>`，它也是不可能含有子节点的。

CharacterData接口定义如下：

```java
public interface CharacterData extends Node {
void appendText(String text);
}
```

可以看到，CharacterData接口为所有自接口声明了`appendtext()`方法，即向自己添加文本内容。

#### Attribute接口（Attribute.java）

Attribute接口直接继承于Node接口，代表属性节点，也是一种叶子节点类型，即不可含有子节点。Attribute是依附于元素节点的，因为我们只能说某个标签元素具有某个属性，在W3C标准里，属性也被单独称为属性节点。

#### Branch接口（Branch.java）

Branch接口继承自Node接口，它描述了XML文档树上的枝干节点，即可有子节点的那些节点：

![](/assets/bhtree.png)

Branch接口定义的部分代码如下：

```java
public interface Branch extends Node {
    Node node(int index) throws IndexOutOfBoundsException;

    int indexOf(Node node);

    int nodeCount();

    Element elementByID(String elementID);
```

可以看到，在Branch接口里定义了许多具有枝干节点特色的行为方法：

`node(int index)`方法：通过给定索引index返回当前Branch节点的子节点列表中的一个节点；

`indexOf(Node node）`方法：如果给定参数中的节点是当前Branch节点的子节点，就返回该节点在子节点列表中的索引；

`nodeCount()`方法：返回当前Branch节点的子节点个数

...

