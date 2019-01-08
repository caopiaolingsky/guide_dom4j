#### 单例模式简介

单例模式（Singleton Pattern）是Java中非常简单的一种设计模式，同工厂模式一样，单例模式也属于创建型模式，它提供了一种创建对象的最佳方式。这种模式涉及到一个单一的类，该类负责创建自己的对象，同时确保只有单个对象被创建。这个类提供了一种访问其唯一的对象的方式，可以直接访问，不需要实例化该类的对象。

**意图：**

保证一个类仅有一个实例，并提供一个访问它的全局访问点。

**动机：**

对于一些类来说，只有一个实例是非常重要的，例如一个打印系统可以有多台打印机，但只能有一个文件管理系统和窗口管理器；又例如一个国家只能有一个总统；Dom4j中只能有一个DocumentFactory对象（为什么会这样，我会在下一节Flyweight模式的探讨中说明这个问题）。

**适用性：**

* 当类只能有一个实例而且客户可以从一个众所周知的访问点访问它时。
* 当这个唯一实例应该是通过子类化可拓展的，并且客户无需更改代码就能使用一个拓展的实例时。

**参与者：**

Singleton：单例类，里面含有Instance这个类操作，负责创建它自己的唯一实例，并允许客户访问它的唯一实例。

#### 单例模式在Dom4j中的巧妙实现

Dom4j中只能有一个DocumentFactory对象（为什么会这样，我会在下一节Flyweight模式的探讨中说明这个问题）。所以Dom4j中对DocumentFactory这个对象进行了单例实现，其实现代码如下：

```java
public class DocumentFactory implements Serializable {
    private static SingletonStrategy<DocumentFactory> singleton = null;

    protected transient QNameCache cache;

    /** Default namespace prefix → URI mappings for XPath expressions to use */
    private Map<String, String> xpathNamespaceURIs;

    private static SingletonStrategy<DocumentFactory> createSingleton() {
        SingletonStrategy<DocumentFactory> result;

        String documentFactoryClassName;
        try {
            documentFactoryClassName = System.getProperty("org.dom4j.factory",
                    "org.dom4j.DocumentFactory");
        } catch (Exception e) {
            documentFactoryClassName = "org.dom4j.DocumentFactory";
        }

        try {
            String singletonClass = System.getProperty(
                    "org.dom4j.DocumentFactory.singleton.strategy",
                    "org.dom4j.util.SimpleSingleton");
            Class<SingletonStrategy> clazz = (Class<SingletonStrategy>) Class.forName(singletonClass);
            result = clazz.newInstance();
        } catch (Exception e) {
            result = new SimpleSingleton<DocumentFactory>();
        }

        result.setSingletonClassName(documentFactoryClassName);

        return result;
    }

    public DocumentFactory() {
        init();
    }
    public static synchronized DocumentFactory getInstance() {
        if (singleton == null) {
            singleton = createSingleton();
        }
        return singleton.instance();
    }

    ...
```

我们可以看到它的构造方法DocumentFactory方法定义成了public型，这样就破坏了单例类只能通过Instance操作实例化的要求。但其实不然，遍历Dom4j的源代码可以发现，没有地方直接使用了（除了后面立马说的）DocumentFactory这个类。其他User类都是通过一个DocumentHelper类来实现对DocumentFactory类的使用的。而DocumentHelper类的构造方法就是私有的，它只能通过调用类方法getDocumentFactory\(\)来实现对DocumentFactory类的实例化，实例化后的对象将存储在DocumentFactory类的一个static域里：

```java
public final class DocumentHelper {
    private DocumentHelper() {
    }

    private static DocumentFactory getDocumentFactory() {
        return DocumentFactory.getInstance();
    }
    ...
```

为了方便对Dom4j中单例模式实现的阐述，将以上两部分代码等效改写如下\(只保留关键代码）：

```java
public class DocumentFactory implements Serializable {
    private static SingletonStrategy<DocumentFactory> singleton = null;
    private DocumentFactory() {
        init();
    }
    public static synchronized DocumentFactory getInstance() {
        if (singleton == null) {
            singleton = createSingleton();
        }
        return singleton.instance();
    }


    private static SingletonStrategy<DocumentFactory> createSingleton() {
    SingletonStrategy<DocumentFactory> result;

    String documentFactoryClassName;
    try {
    documentFactoryClassName = System.getProperty("org.dom4j.factory",
    "org.dom4j.DocumentFactory");
    } catch (Exception e) {
    documentFactoryClassName = "org.dom4j.DocumentFactory";
    }

    try {
    String singletonClass = System.getProperty(
    "org.dom4j.DocumentFactory.singleton.strategy",
    "org.dom4j.util.SimpleSingleton");
    Class<SingletonStrategy> clazz = (Class<SingletonStrategy>) Class.forName(singletonClass);
    result = clazz.newInstance();
    } catch (Exception e) {
    result = new SimpleSingleton<DocumentFactory>();
    }

    result.setSingletonClassName(documentFactoryClassName);

    return result;
    }
    ...
```

* 分析该代码容易发现，单例类DocumentFactory定义了一个私有的static属性singleton用于存放持有自己的唯一实例。它的构造方法`DocumentFactory()`也被定义成了private的，这就使得该类不能被new出一个实例，而只能通过调用类的方法来产生实例。这里是通过类方法`getInstance()`来获得类的唯一实例，注意必须是类方法即被static修饰的，因为不能通过实例方法来创建实例，毕竟这里只有一个实例。而且由于static域的singleton属性是共享的就保证了DocumentFactory类的实例的唯一性。`grtInstance()`方法被多次调用，也不会跟新该实例，除非reset，因为`getInstance()`只有在`singleton=null`的条件满足时才创建实例，而这只在创建第一个实例或是reset后才满足条件。



