有了前面的准备之后，我们可以开始编写程序简单使用Dom4j了。

* 首先在test\_dom4j\testfiles路径下创建XML文件books.xml，其内容如下:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<books>
   <book id="001">
      <title>深入解析Dom4j</title>
      <author>赵飞</author>
   </book>
   <book id="002">
      <title>设计模式</title>
      <author>GOF</author>
   </book>
   <book id="003">
      <title>Java编程思想</title>
      <author>Bruce Eckel</author>
   </book>
</books> 
```

* 在前面所建立的test\_dom4j项目中新建类TestDom4j：

```java
package test_dom4j;

import java.io.File;
import java.util.List;
 
import org.dom4j.Document;
import org.dom4j.Element;
import org.dom4j.io.SAXReader;
 
public class TestDom4j {
	public static void main(String[] args) throws Exception 
	{
		SAXReader reader = new SAXReader();	// SAXReader类来自Dom4j的实现
		File file = new File("testfile/books.xml");	// 使用books.xml文件创建文件输入流
		Document document = reader.read(file);	// 建立获取文档对象模型
		Element root = document.getRootElement();	// 获取文档的根节点
		List<Element> childElements = root.elements();	// 根节点的所有子节点形成一个列表childElements
		for (Element child : childElements) {	// 对根节点的子节点进行遍历
			//已知属性名情况下
			System.out.println("id: " + child.attributeValue("id"));
			System.out.println("title" + child.elementText("title"));
			System.out.println("author" + child.elementText("author"));
			//这行是为了格式化美观而存在
			System.out.println();
		}
	} 
}
```

* 运行该程序即可在控制台得到以下输出结果：

![](/assets/testDom4j1.png)

是不是很神奇呢？通过短短的几行代码（这个程序中大部分是结果输出print）就实现了对books.xml文件的解析，并且将文档内容以我们想要的格式再一次给打印出来了，这就是Dom4j的强大之处。那Dom4j是怎么做到这一点的呢？我将通过下面两个章节来阐明这个问题。

