#### Dom4j中的访问者模式（Visitor Pattern）

在Dom4j中，可能随时需要对XML文档树进行一种与之前不同的遍历，例如一种遍历会将所有注释节点给删除，而另一种遍历会给每个元素节点添加某个属性。一个很自然的传统的想法就是把这些方法行为给封装到相应的节点对象里，倘若这样组织程序，那么当对节点的操作需求越来越多时，这些分散在各个对象里的方法就会让系统难以理解、难以维护和修改。这时我们可以采用Visitor模式，它是一种对象行为模式，可以让我们在不改变各个元素类的前提下定义作用于这些元素的新操作：

![](/assets/visitor1.png)

在Dom4j中定义了Visitor接口，以便支持Visitor模式：

```java
// Visitor.java
public interface Visitor {

    void visit(Document document);

    void visit(DocumentType documentType);

    void visit(Element node);

    ...
```

然后在每个元素类内部为对象定义了accept方法，例如在Element的抽象类中有如下方法：

```java
// AbstractElement.java
public void accept(Visitor visitor) {
    visitor.visit(this);

    // visit attributes
    for (int i = 0, size = attributeCount(); i < size; i++) {
        Attribute attribute = attribute(i);

        visitor.visit(attribute);
    }

    // visit content
    for (int i = 0, size = nodeCount(); i < size; i++) {
        Node node = node(i);

        node.accept(visitor);
    }
}
```

Element的`accept()`方法首先将自身传给访问者visitor的`visit()`方法，然后依次遍历将自己的属性节点、子节点传递给visitor的`visit()`方法，以便让访问者操作。

#### Dom4j中的适配器模式（Adapter Pattern）

适配器模式就像其名字所描述的那样，起适配器的作用。适配器在中间作为中介，让原来不兼容的两个东西变得兼容匹配。Dom4j的核心代码只具备对XML文档的DOM操作功能，但我们前面已经提到过Dom4j同时支持SAX（simple API XML for Java），支持jaxb（Java architecture XML binding），支持swing tree binding，支持JavaBean，这都得益于Dom4j采用了适配器模式，写了一层适配器代码（如jaxb，swing文件夹下的代码），对DOM操作进行了进一步封装。这里不再赘述了，先写到这里了。

