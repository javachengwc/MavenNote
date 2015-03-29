####使用Jetty-maven-plugin进行测试
>能够使用单元测试覆盖的代码就不应该依赖于Web页面测试。

传统的Web测试方法要求我们编译、测试、打包及部署，这往往会消耗数10秒至数分钟的时间，jetty-maven-plugin能够帮助我们节省时间，它能够周期性地检查项目内容，发现变更后自动更新到内置的Jetty Web容器中。换句话说，它帮我们省去了打包和部署的步骤。jetty-maven-plugin默认就很好地支持了Maven的项目目录结构。在通常情况下，我们需要直接在IDE中修改源码，IDE能够执行自动编译，jetty-maven-plugin发现编译后的文件变化后，自动将其更新到Jetty容器，这时就可以直接测试Web页面了。
使用jetty-maven-plugin十分简单。指定该插件的坐标，并且稍加配置即可。

    <plugin>
	    <groupdId>org.mortbay.jetty</groupId>
	    <artifactId>jetty-maven-plugin</artifactId>
	    <version>7.1.6.v2010715</version>
	    <configuration>
			<scanIntervalSeconds>10</scanIntervalSeconds>
			<webAppConfig>
				<contextPath>/test</contextPath>
			</webAppConfig>
		</configuration>
	</plugin>

然后，配置<code>settings.xml</code>文件

    <settings>
	    ...
	    <pluginGroups>
	    <!-- pluginGroup
	     | Specifies a further group identifier to use for plugin lookup.
	    <pluginGroup>com.your.plugins</pluginGroup>
	    -->
			<pluginGroup>org.mortbay.jetty</pluginGroup>
		</pluginGroups>
		...
	</settings>
现在可以运行如下命令启动jetty-maven-plugin：
<code>mvn jetty:run</code>
jetty-mavne-plugin会启动Jetty，并且默认监听本地的8080端口，并且将当前项目部署到容器中，同时它还会根据用户配置扫描代码改动。
如果希望使用其他端口，可以添加jetty.port参数，例如：
<code>mvn jetty:run -Djetty.port=8081</code>
现在就可以打开浏览器通过地址<code>http://localhost:8081/test/</code>测试应用了。要停止Jetty，只需要在命令行输入<code>Ctrl + C </code>即可。