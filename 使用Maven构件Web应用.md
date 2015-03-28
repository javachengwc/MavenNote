###使用Maven构件Web应用
####Web项目的目录结构
一个典型的WAR文件会有如下结构：
<pre>
-war/
 +/META-INF/
 +WEB-INF/
 | + classes/
 | | + ServletA.class
 | | + config.properties
 | | + ...
 | | 
 | + lib/
 | | + dom4j-1.4.1.jar
 | | + mail-1.4.1.jar
 | | + ...
 | |
 | +web.xml
 +img/
 |
 +css/
 |
 +js/
 |
 +index.xml
 +sample.jsp
 </pre>
 一个WAR包下至少包含两个子目录：MEAT-INF和WEB-INF。前者包含了一些打包元数据信息，我们一般不去关心；后者是WAR包的核心，WEB-INF下必须包含一个Web资源的描述文件web.xml，它的子目录classes包含所有该Web项目的类，而另一个子目录lib则包含所有该Web项目的依赖JAR包，classes和lib目录都会在运行时加入到Classpath中。除了MEAT-INF和WEB-INF外，一般的WAR还会包含很多Went资源。
 同任何其他Maven项目一样，Maven对Web项目的布局结构也有一个通用的约定。不过首先要记住的是，用户必须为Web项目显式指定打包方式是war。代码如下：
 
 

      <project>
		 ...
		 <groupId>com.juvenxu.mvnbook</groupId>
		 <artifactId>sample-war</artifactId</artifactId>
		 <packaging>war</packaging>
		 <version>1.0-SNAPSHOT</version>
		 ...
	  </project>

>如果不显式指定apckaging，Maven会使用默认的jar打包方式，从而导致无法正确打包Web项目。

Web项目的类及资源文件同一般JAR项目一样，默认位置都是<code>src/main/java</code>和<code>src/main/resources</code>，测试类及测试资源文件的默认位置是<code>src/test/java</code>和<code>src/test/reources</code>。Web项目比较特殊的地方在于：它还是一个Web资源目录，其默认位置是<code>src/main/webapp</code>。一个典型的Web项目的Maven目录结构如下：
<pre>
+project
 | 
 +pom.xml
 |
 +src/
  +main/
  | + java/
  | | + ServletA.java
  | | + . . .
  | |
  |+ resources/
  | | + config.properties
  | | + . . .
  | |
  | +webapp/
  |  +WEB_INF/
  |  | + web.xml
  |  |
  |  + img/
  |  |
  |  + css/
  |  |
  |  + js/
  |  |
  |  + index.html
  | +  sample.jsp
  +test/
   +java/
   +resources/
   </pre>
   在<code>src/main/webapp/</code>目录下，必须包含一个WEB-INF，该子目录还必须要包含web.xml文件。<code>src/main/webapp</code>目录下其他文件和目录包括html、jsp、css、js等，它们与WAR包中的Web资源一致。