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



