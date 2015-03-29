####使用Cargo实现自动化部署
[Cargo](http://cargo.codehaus.org/Home)是一组帮助用户操作Web容器的工具，它能够帮助用户实现自动化部署，而且它几乎支持所有的Web容器，如：Tomcat、JBoss、Jetty和Glassfish等。
![Cargo架构](http://docs.codehaus.org/download/attachments/58083/access-layers.jpg?api=v2)
![Cargo架构](http://docs.codehaus.org/download/attachments/58083/architecture.jpg?api=v2)
#####部署至本地Web容器
Cargo支持两种本地部署的方式，分别为standalone模式和existing模式。在standalone模式中，Cargo会从Web容器的安装目录复制一份配置到用户指定的目录，然后在此基础上部署应用，，每次重新构建的时候，这个目录会被清空，所有配置被重新生成。
而在existing模式中，用户需要指定现有的Web容器配置目录，然后Cargo会直接使用这些配置并将应用部署到其对应的位置。
>使用standalone模式部署应用至本地Web容器

    <plugin>
	    <groupId>org.codehaus.cargo</groupId>
	    <artifactId>cargo-maven2-plugin</artifactId>
	    <version>1.0</version>
	    <configuration>
		    <container>
			    <containerId>tomcat6x</containerId>
			    <home>D:\apache-tomcat</home>
			</container>
			<configuration>
				<type>standalone</type>
				<home>${project.build.directory}/tomcat6</home>
			</configuration>
		</configuration>
	</plugin>
现在，要让Cargo启动Tomcat并部署应用，只需要运行：
<code>mvn cargo:start</code>

>使用existing模式部署应用至本地Web容器

    <plugin>
	    <groupId>org.codehaus.cargo</groupId>
	    <artifactId>cargo-maven2-plugin</artifactId>
	    <version>1.0</version>
	    <configuration>
		    <container>
			    <containerId>tomcat6x</containerId>
			    <home>D:\apache-tomcat-7.0.41</home>
			</container>
			<configuration>
				<type>existing</type>
				<home>D:\apache-tomcat-7.0.41</home>
			</configuration>
		</configuration>
	</plugin>

#####部署至远程Web容器
除了让Cargo直接管理本地Web容器然后部署应用之外，也可以让Cargo部署应用至远程的正在运行的Web容器中。

    <plugin>
	    <groupId>org.codehaus.cargo</groupId>
	    <artifactId>cargo-maven2-plugin</artifactId>
	    <version>1.0</version>
	    <configuration>
		    <container>
			    <containerId>tomcat6x</containerId>
			    <type>remote</type>
			</container>
			<configuration>
				<type>runtime</type>
				<properties>
					<cargo.remote.username>admin</cargo.remote.username>
					<cargo.remote.password>admin123</cargo.remote.password>
					<cargo.tomcat.manager.url>http://localhost:8080/manager</cargo.tomcat.manager.url>
				</properties>
			</configuration>
		</configuration>
	</plugin>
有了上述配置以后，就可以让Cargo部署应用了。运行命令如下：
<code>mvn cargo:redeploy</code>
如果容器中已经部署了当前应用，Cargo会先将其卸载，让后在重新部署。
