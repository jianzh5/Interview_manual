### 1. Maven有哪些优点和缺点

**优点如下：**
 1.简化了项目依赖管理：
 2.易于上手，对于新手可能一个"mvn clean package"命令就可能满足他的工作
 3.便于与持续集成工具（jenkins）整合
 4.便于项目升级，无论是项目本身升级还是项目使用的依赖升级。
 5.有助于多模块项目的开发，一个模块开发好后，发布到仓库，依赖该模块时可以直接从仓库更新，而不用自己去编译。
 6.maven有很多插件，便于功能扩展，比如生产站点，自动发布版本等

**缺点如下：**
 1.maven是一个庞大的构建系统，学习难度大
 2.maven采用约定优于配置的策略（convention over configuration），虽然上手容易，但是一旦出了问题，难于调试。
 3.当依赖很多时，m2eclipse 老是搞得Eclipse很卡。
 4.中国的网络环境差，很多repository无法访问，比如google code， jboss 仓库无法访问等。





### 2. Maven坐标是什么

一般maven使用[groupID,artifactId,version，packaging]来表示一个项目的某个版本，有时还会使用classifier来表示项目的附属构建，常见的附属构建有javadoc和sources包。





### 3. Maven常见的依赖范围有哪些?

1. compile:编译依赖，默认的依赖方式，在编译（编译项目和编译测试用例），运行测试用例，运行（项目实际运行）三个阶段都有效，典型地有spring-core等jar。
2. test:测试依赖，只在编译测试用例和运行测试用例有效，典型地有JUnit。
3. provided:对于编译和测试有效，不会打包进发布包中，典型的例子为servlet-api,一般的web工程运行时都使用容器的servlet-api。
4. runtime:只在运行测试用例和实际运行时有效，典型地是jdbc驱动jar包。
5. system: 不从maven仓库获取该jar,而是通过systemPath指定该jar的路径。
6. import: 用于一个dependencyManagement对另一个dependencyManagement的继承。





### 4. Maven的生命周期

 maven有三套生命周期，分别为：

1. clean 周期：主要用于清理上一次构建产生的文件，可以理解为删除target目录

2. 默认周期，
   主要阶段包含:
   * process-resources 默认处理src/test/resources/下的文件，将其输出到测试的classpath目录中,
   * compile 编译src/main/java下的java文件，产生对应的class,
   * process-test-resources 默认处理src/test/resources/下的文件，将其输出到测试的classpath目录中,
   * test-compile 编译src/test/java下的java文件，产生对应的class,
   * test 运行测试用例,
   * package 打包构件，即生成对应的jar, war等,
   * install将构件部署到本地仓库,
   * deploy 部署构件到远程仓库
3. site周期
   主要阶段包含：
   * site 产生项目的站点文档
   * site-deploy 将项目的站点文档部署到服务器





### 5. 我们经常使用“Mvn Clean Package”命令进行项目打包，请问该命令执行了哪些动作来完成该任务？

 在这个命令中我们调用了maven的clean周期的clean阶段绑定的插件任务，以及default周期的package阶段绑定的插件任务
 默认执行的任务有（maven的术语叫goal, 也有人翻译成目标，我这里用任务啦）

```yaml
maven-clean-plugin:clean->
maven-resources-plugin:resources->
maven-compile-plugin:compile->
mavne-resources-plugin:testResources->
maven-compile-plugin:testCompile->
maven-jar-plugin:jar
```





### 6. 依赖的解析机制

* 解析发布版本：如果本地有，直接使用本地的，没有就向远程仓库请求。
* 解析快照版本：合并本地和远程仓库的元数据文件-groupId/artifactId/version/maven-metadata.xml，这个文件存的版本都是带时间戳的，将最新的一个改名为不带时间戳的格式供本次编译使用。
* 解析版本为LATEST,RELEASE，过于复杂，且解析的结果不稳定， 不推荐在项目中使用，感兴趣的同学自己去研究，简而言之就是合并`groupId/artifactId/maven-metadata.xml`找到对应的最新版本和包含快照的最新版本。





###  7. 插件的解析机制

 当我们输入"mvn dependency:tree"这样的指令，解析的步骤为：

1. 解析groupID:maven使用默认的groupID:"org.apache.maven.plugins"或者"org.codehaus.mojo"
2. 解析artifactId(maven的官方叫做插件前缀解析策略)
3. 合并该groupId在所有仓库中的元数据库文件（maven-metadata-repository.xml），比如maven官方插件的元数据文件所在的目录为orgapachemavenplugins，该文件下有如下的条目

```xml
<plugin>
 <name>MavenDependencyPlugin</name>
 <prefix>dependency</prefix>
 <artifactId>maven-dependency-plugin</artifactId>
 </plugin>
```

通过比较这样的条目，我们就将该命令的artifactId解析为maven-dependency-plugin

4. 解析version：如果你在项目的pom中声明了该插件的版本，那么直接使用该版本的插件，否则合并所有仓库中groupId/artifactId/maven-metadata-repository.xml，找到最新的发布版本。

对于非官方的插件，有如下两个方法可以选择：

1. 使用groupId:artifactId:version:goal 来运行
2. 在Settings.xml中添加pluginGroup项，这样maven不能在官方的插件库中解析到某个插件，那么就可以去你配置的group下查找啦。





### 8. 多模块如何聚合

 配置一个打包类型为pom的聚合模块，然后在该pom中使用<module>元素声明要聚合的模块





### 9. 对于一个多模块项目，如果管理项目依赖的版本

通过在父模块中声明dependencyManagement和pluginManagement， 然后让子模块通过<parent>元素指定父模块，这样子模块在定义依赖是就可以只定义groupId和artifactId，自动使用父模块的version,这样统一整个项目的依赖的版本。





### 10. 一个项目的依赖来源于不同的组织，可能这些依赖还会依赖别的Jar包，如何保证这些传递依赖不会引起版本冲突。

 使用<dependency>的<exclusion>元素将会引起冲突的元素排除。





### 11. 常见的 Maven私服的仓库类型。

 （宿主仓库）hosted repository, 

（代理仓库）proxy repository, 

（仓库组）group repository





### 12. 如何查询一个插件有哪些目标（Goal）

 mvn help:describe -Dplugin=groupId:artifactId





### 13. Maven仓库是什么

 Maven仓库是基于简单文件系统存储的，集中化管理Java API资源（构件）的一个服务。仓库中的任何一个构件都有其唯一的坐标，根据这个坐标可以定义其在仓库中的唯一存储路径。得益于 Maven 的坐标机制，任何 Maven项目使用任何一个构件的方式都是完全相同的，Maven 可以在某个位置统一存储所有的 Maven 项目共享的构件，这个统一的位置就是仓库，项目构建完毕后生成的构件也可以安装或者部署到仓库中，供其它项目使用。
 对于Maven来说，仓库分为两类：本地仓库和远程仓库。





### 14. Maven的工程类型有哪些？

 1）POM工程
 POM工程是逻辑工程。用在父级工程或聚合工程中。用来做jar包的版本控制。
 2）JAR工程
 将会打包成jar用作jar包使用。即常见的本地工程 - Java Project。
 3）WAR工程
 将会打包成war，发布在服务器上的工程。如网站或服务。即常见的网络工程 - Dynamic Web Project。war工程默认没有WEB-INF目录及web.xml配置文件，IDE通常会显示工程错误，提供完整工程结构可以解决。





### 15. Maven常用命令有哪些？

 1）install
 本地安装， 包含编译，打包，安装到本地仓库
 编译 - javac
 打包 - jar， 将java代码打包为jar文件
 安装到本地仓库 - 将打包的jar文件，保存到本地仓库目录中。
 2）clean
 清除已编译信息。
 删除工程中的target目录。
 3）compile
 只编译。javac命令
 4）deploy
 部署。常见于结合私服使用的命令。
 相当于是install+上传jar到私服。
 包含编译，打包，安装到本地仓库，上传到私服仓库。
 5）package
 打包。包含编译，打包两个功能。