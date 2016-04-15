title: maven不同环境资源文件打包
tags:
  - maven
  - java
categories:
  - 后端开发
date: 2016-03-09 15:39:00
---

<img src="/asserts/images/logo/maven.png" class="img-logo img-center" />


## 一、资源结构
maven工程资源结构如下：
``` bash
yerba-buena:jvwa yeshaoting$ tree src/main/resources/
src/main/resources/
├── app-db.properties
├── app.properties
├── dev
│   ├── app-db.properties
│   └── app.properties
├── file
│   ├── image.jpg
│   └── stage6.jsp
├── log4j.properties
├── model
│   └── UserMapper.xml
├── prod
│   ├── app-db.properties
│   └── app.properties
├── spring-beans.xml
├── spring-db.xml
└── spring-mvc.xml

4 directories, 13 files
```


## 二、需求
服务部署时，希望针对于不同的环境，使用不同的配置文件。
针对于当前工程而言，变化的配置文件为app.properties和app-db.properties。

本地环境使用：src/main/resources/ 目录下的这二个文件。
测试环境使用：src/main/resources/dev 目录下的这二个文件。
生产环境使用：src/main/resources/prod 目录下的这二个文件。


<!-- more -->


## 三、解决方案
借助maven-resources-plugin插件，在pom.xml指定配置。
打包时，通过 mvn clean package -Pdev 来执行对应的环境资源拷贝工作。

具体配置如下：

### 1. 基本配置
``` xml
<profiles>
    <profile>
      <id>dev</id>
      <build>
        <plugins>
          <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-resources-plugin</artifactId>
            <version>2.6</version>
            <executions>
              <execution>
                <id>copy-resources</id>
                <!-- 在default生命周期的 validate阶段就执行resources插件的copy-resources目标 -->
                <phase>validate</phase>
                <goals>
                  <goal>copy-resources</goal>
                </goals>
                <configuration>
                  <!-- 指定resources插件处理资源文件到哪个目录下 -->
                  <outputDirectory>${project.build.outputDirectory}</outputDirectory>
                  <!-- 也可以用下面这样的方式（指定相对url的方式指定outputDirectory） <outputDirectory>target/classes</outputDirectory> -->
                  <!-- 待处理的资源定义 -->
                  <resources>
                    <resource>
                      <!-- 指定resources插件处理哪个目录下的资源文件 -->
                      <directory>src/main/resources/dev</directory>
                      <!-- 指定不需要处理的资源 <excludes> <exclude>WEB-INF/*.*</exclude> </excludes> -->
                      <!-- 是否对待处理的资源开启过滤模式 (resources插件的copy-resources目标也有资源过滤的功能，这里配置的 这个功能的效果跟<build><resources><resource>下配置的资源过滤是一样的，只不过可能执行的阶段不一样，这里执行的阶段是插件指定的validate阶段，<build><resources><resource>下的配置将是在resources插件的resources目标执行时起作用（在process-resources阶段）) -->
                      <filtering>false</filtering>
                    </resource>
                  </resources>
                </configuration>
                <inherited></inherited>
              </execution>
            </executions>
          </plugin>
        </plugins>
      </build>
    </profile>
</profiles>
```

### 2. 改进配置
对于上面的配置，如果有prod, dev, local三类环境的话，则需要在对应的profile节点下添加三次build的配置。我们可以观察到针对不同的环境只是directory节点内容不同而已，可以使用一个变更代替就好了。

我们在profile节点下添加properties属性，使用`deploy.env`来代替不同的环境目录名。于是改进后的配置如下：
``` xml
<profiles>
    <profile>
      <id>dev</id>
      <properties>
        <deploy.env>dev</deploy.env>
      </properties>
    </profile>
    <profile>
      <id>prod</id>
      <properties>
        <deploy.env>prod</deploy.env>
      </properties>
    </profile>
  </profiles>

  <build>
    <plugins>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-resources-plugin</artifactId>
        <version>2.6</version>
        <executions>
          <execution>
            <id>copy-resources</id>
            <!-- 在default生命周期的 validate阶段就执行resources插件的copy-resources目标 -->
            <phase>validate</phase>
            <goals>
              <goal>copy-resources</goal>
            </goals>
            <configuration>
              <!-- 指定resources插件处理资源文件到哪个目录下 -->
              <outputDirectory>${project.build.outputDirectory}</outputDirectory>
              <!-- 也可以用下面这样的方式（指定相对url的方式指定outputDirectory） <outputDirectory>target/classes</outputDirectory> -->
              <!-- 待处理的资源定义 -->
              <resources>
                <resource>
                  <!-- 指定resources插件处理哪个目录下的资源文件 -->
                  <directory>src/main/resources/${deploy.env}</directory>
                  <!-- 指定不需要处理的资源 <excludes> <exclude>WEB-INF/*.*</exclude> </excludes> -->
                  <!-- 是否对待处理的资源开启过滤模式 (resources插件的copy-resources目标也有资源过滤的功能，这里配置的 这个功能的效果跟<build><resources><resource>下配置的资源过滤是一样的，只不过可能执行的阶段不一样， 
                    这里执行的阶段是插件指定的validate阶段，<build><resources><resource>下的配置将是在resources插件的resources目标执行时起作用（在process-resources阶段）) -->
                  <filtering>false</filtering>
                </resource>
              </resources>
            </configuration>
            <inherited></inherited>
          </execution>
        </executions>
      </plugin>
    </plugins>
  </build>
```

**注**：对于资源拷贝问题，网上也用不少通过 maven-antrun-plugin 插件来完成资源拷贝工作，也可以参考。


## 四、参考文档
[利用maven中resources插件的copy-resources目标进行资源copy和过滤](http://www.tuicool.com/articles/JfaA7r)
[Copy Resources](http://maven.apache.org/plugins/maven-resources-plugin/examples/copy-resources.html)
[使用maven复制配置文件](http://www.blogjava.net/iduido/archive/2013/03/24/396913.html)
[Maven 如何为不同的环境打包 —— 开发、测试和产品环境](https://www.zybuluo.com/haokuixi/note/25985)
[maven copy file文件到指定目录](http://www.tuicool.com/articles/bEbaIz)
