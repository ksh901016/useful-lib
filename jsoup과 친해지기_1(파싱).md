### jsoup 라이브러리와 친해지기

---

jsoup은 자바로 만들어진 HTML 파서(Parser) 이다.

자바 언어로 HTML을 쉽고 강력하게 다룰 수 있는 기능을 제공한다.

`https://jsoup.org/`에 오픈소스로 제공되어 있음

```xml
// pom.xml에 추가하여 사용
<dependency>
    <groupId>org.jsoup</groupId> 
    <artifactId>jsoup</artifactId> 
    <version>1.10.3</version> 
</dependency>
```



#### HTML로 파싱하기

##### 1. Parse a document form a String(문자열로부터 파싱하기)

---

jsoup의 parse를 이용하면 String 문자열을 HTML Document로 파싱을 해준다.

````java
String html = "<html><head><title>First Page</title></head>"
				+"<body><p>Parsed HTML into a doc.</p></body></html>";
		
Document doc = Jsoup.parse(html);
````

String 문자열을 well-formed하게 파싱해준다.

ex) `<p> Lorem <p>Ipsum  ===> <p>Lorem</p> <p>Ipsum</p>`



##### The object model of a document.

> 1. Document 객체는 Elements와 TextNode로 이루어져 있다.
>
> 2. 상속관계는 아래와 같다.
>    * `Document` extends `Element` extends `Node`
>    * `TextNode` extends `Node`
> 3. Element는 여러개의 children Nodes 로 구성되어 있고, 하나의 parent Element를 갖는다.



##### 2. Parsing a body fragment(문서의 일부분을 가진 문자열로부터 파싱하기)

---

```java
String html = "<div><p>Lorem ipsum.</p>";
Document doc = Jsoup.parseBodyFragment(html);
Element body = doc.body();
System.out.println(doc);
```

parseBodyFragment는 빈 document를 생성하여, body 엘리먼트에 해당 내용을 삽입한다. 위에서 사용한 parse 메소드를 사용해도 똑같은 결과를 얻을 수 있지만, 명시적으로 사용하면 bozo HTML의 제공을 보장한다고함(해석을 잘 못하겟음)

Document.body() 메소드는 document의 body 엘리먼트를 갖고오는 것이며 doc.getElementByTag("body"); 와 같다.



##### 3. Load a Document from a URL

---

웹사이트에서 HTML document를 fetch, parse할 때 connect 메소드가 사용된다.

```java
Document doc = jsoup.connect("http://example.com/").get();
String title = doc.title();
```

GET 방식을 사용하여 HTML 문서를 파싱해올 수 있다.

물론 POST방식도 가능

```java
Document doc = Jsoup.connect("http://example.com")
    .data("query", "Java")
    .userAgent("Mozilla")
    .cookie("auth", "token")
    .timeout(3000)
    .post();
```



