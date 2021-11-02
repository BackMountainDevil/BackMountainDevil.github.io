# Eclipse 设置国内镜像加快下载速度 Plugin Maven Mirror
- date: 2021-07-12T12:05:03+08:00
- lastmod: 2021-07-12

由于一些原因，默认镜像下载速度感人，颇有愚公移山的精神，但是时间就是生命！下面是更换国内镜像走上高速的办法。

# Plugin
Eclipse -> Window -> Preferences -> Install/Update -> Available software Sites -> Add...  

Name: utsc  
Location: http://mirrors.ustc.edu.cn/eclipse  
然后 Add -> Apply and Close

# Maven
首先建立一个文件 `setting.xml`，具体路径看你心情，官方建议是 /home/kearney/.m2/setting.xml （kearney是我的用户名）。

<details>
<summary>点击查看 setting.xml 文件内容 </summary>

里面设置了一些国内大厂的 Maven 镜像，复制粘贴保存即可
```xml
<settings xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 http://maven.apache.org/xsd/settings-1.0.0.xsd">
	<mirrors>
		<mirror>
		    <id>alimaven</id>
		    <name>aliyun maven</name>
		    <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
		    <mirrorOf>central</mirrorOf>
		</mirror>

		<mirror>
		    <id>huaweicloud</id>
		    <name>huaweicloud maven</name>
		    <mirrorOf>*</mirrorOf>
		    <url>https://mirrors.huaweicloud.com/repository/maven/</url>
		</mirror>

		<mirror>
		    <id>nexus-163</id>
		    <mirrorOf>*</mirrorOf>
		    <name>Nexus 163</name>
		    <url>http://mirrors.163.com/maven/repository/maven-public/</url>
		</mirror>


		<mirror>
		    <id>nexus-tencentyun</id>
		    <mirrorOf>*</mirrorOf>
		    <name>Nexus tencentyun</name>
		    <url>http://mirrors.cloud.tencent.com/nexus/repository/maven-public/</url>
		</mirror> 

		<mirror>
	        <id>aliyun-public</id>
	        <mirrorOf>*</mirrorOf>
	        <name>aliyun public</name>
	        <url>https://maven.aliyun.com/repository/public</url>
	    </mirror>

	    <mirror>
	        <id>aliyun-central</id>
	        <mirrorOf>*</mirrorOf>
	        <name>aliyun central</name>
	        <url>https://maven.aliyun.com/repository/central</url>
	    </mirror>
	
	    <mirror>
	        <id>aliyun-spring</id>
	        <mirrorOf>*</mirrorOf>
	        <name>aliyun spring</name>
	        <url>https://maven.aliyun.com/repository/spring</url>
	    </mirror>

	    <mirror>
	        <id>aliyun-spring-plugin</id>
	        <mirrorOf>*</mirrorOf>
	        <name>aliyun spring-plugin</name>
	        <url>https://maven.aliyun.com/repository/spring-plugin</url>
	    </mirror>
	
	    <mirror>
	        <id>aliyun-apache-snapshots</id>
	        <mirrorOf>*</mirrorOf>
	        <name>aliyun apache-snapshots</name>
	        <url>https://maven.aliyun.com/repository/apache-snapshots</url>
	    </mirror>
	
	    <mirror>
	        <id>aliyun-google</id>
	        <mirrorOf>*</mirrorOf>
	        <name>aliyun google</name>
	        <url>https://maven.aliyun.com/repository/google</url>
	    </mirror>
	
	    <mirror>
	        <id>aliyun-gradle-plugin</id>
	        <mirrorOf>*</mirrorOf>
	        <name>aliyun gradle-plugin</name>
	        <url>https://maven.aliyun.com/repository/gradle-plugin</url>
	    </mirror>
	
	    <mirror>
	        <id>aliyun-jcenter</id>
	        <mirrorOf>*</mirrorOf>
	        <name>aliyun jcenter</name>
	        <url>https://maven.aliyun.com/repository/jcenter</url>
	    </mirror>
	
	    <mirror>
	        <id>aliyun-releases</id>
	        <mirrorOf>*</mirrorOf>
	        <name>aliyun releases</name>
	        <url>https://maven.aliyun.com/repository/releases</url>
	    </mirror>
	
	    <mirror>
	        <id>aliyun-snapshots</id>
	        <mirrorOf>*</mirrorOf>
	        <name>aliyun snapshots</name>
	        <url>https://maven.aliyun.com/repository/snapshots</url>
	    </mirror>

	    <mirror>
	        <id>aliyun-grails-core</id>
	        <mirrorOf>*</mirrorOf>
	        <name>aliyun grails-core</name>
	        <url>https://maven.aliyun.com/repository/grails-core</url>
	    </mirror>
	
	    <mirror>
	        <id>aliyun-mapr-public</id>
	        <mirrorOf>*</mirrorOf>
	        <name>aliyun mapr-public</name>
	        <url>https://maven.aliyun.com/repository/mapr-public</url>
	    </mirror>
	
		<mirror>
			<id>nexus-osc</id>
			<mirrorOf>central</mirrorOf>
			<name>Nexus osc</name>
			<url>http://maven.oschina.net/content/groups/public/</url>
		</mirror>
		
		<mirror>
			<id>nexus-osc-thirdparty</id>
			<mirrorOf>thirdparty</mirrorOf>
			<name>Nexus osc thirdparty</name>
			<url>http://maven.oschina.net/content/repositories/thirdparty/</url>
		</mirror>
	</mirrors>

	<profiles>
		<profile>
			<id>default</id>
			<repositories>
				<repository>
					<id>alimaven</id>
					<name>aliyun maven</name>
					<url>http://maven.aliyun.com/nexus/content/groups/public/</url>
					<releases>
						<enabled>true</enabled>
					</releases>
					<snapshots>
						<enabled>false</enabled>
					</snapshots>
				</repository>
			</repositories>
			<pluginRepositories>
				<pluginRepository>
					<id>alimaven</id>
					<name>aliyun maven</name>
					<url>http://maven.aliyun.com/nexus/content/groups/public//</url>
					<releases>
						<enabled>true</enabled>
					</releases>
					<snapshots>
						<enabled>false</enabled>
					</snapshots>
				</pluginRepository>
			</pluginRepositories>
		</profile>
	</profiles>
</settings>
```
</details>

Eclipse -> Window -> Preferences -> Maven  。然后把配置文件的路径（/home/kearney/.m2/setting.xml）粘贴到 User Setting 中，然后 Update Settings。原始镜像更新让人怀疑人生。

里面的镜像站地址也可用于使用到 Maven 的配置中，如 DBeaver。单独把 Eclipse 列出来是因为它的设置要自己建文件，格式还不好整。。。好的设置不应该是傻瓜式填个 ID 和 URL 就行了吗。

# Refer
- [eclipse设置maven加载国内镜像. 2016-08-29. 编程之熊](https://www.cnblogs.com/arvtie/p/5817793.html)