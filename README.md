# PublishToJcenter
发布到jcenter路过各种坑，尝试了各个大神的文章一直跑步起来，这里综合一下

##主要是针对新版本的Brantray的lib发布到Jcenter的Gradle脚本及属性文件.

`bintray.gradle`: 用于发布到JCenter的脚本。

`build.gradle`: project下的脚本

`gradle.properties`: 在`bintray.gradle`对应的属性，新版本增加了组织的概念

`lib/build.gradle`: 针对需要发布的model，其中切记 <h5>apply from: '../bintray.gradle'一定要写在最后面<\h5>

###1. 注册保存bintray

bintray的地址：https://bintray.com/ ，注册时候qq邮箱和163邮箱注册不了，微软的live邮箱和新浪邮箱可以注册。

网上大多数文章都说进入后会有一个API Key，但是我在edit profile一直没找到，直达后来创建了maven之后才发现。

记住账号名以及API Key是bintray上传必须的。 

目前我是放在了项目的gradle.properties下，需要的时候就填写了发布，各位如果有更好的办法可以提供下
```
BINTRAY_USER=bintray account name
BINTRAY_KEY=bintray API Key
```

###2. project目录下的`build.gradle`文件，对应<a href="https://github.com/CarGuo/PublishToJcenter/blob/master/build.gradle">build.gradle></a>
主要添加这个依赖
```
buildscript {
    repositories {
        jcenter()
    }
    dependencies {
        classpath 'com.github.dcendents:android-maven-gradle-plugin:1.5'
        classpath 'com.jfrog.bintray.gradle:gradle-bintray-plugin:1.6'
    }
}
```
###3. 在lib的这个`build.gradle`的**底部**添加以下代码：
    （一定要在底部，對應<a href="https://github.com/CarGuo/PublishToJcenter/blob/master/lib/build.gradle">build.gradle></a>
主要添加这个依赖）
    apply from: './bintray.gradle'

###34.根目录下的`gradle.properties`

接下来对内容进行配置，下面是一个例子：
```
# <dependency>
#   <groupId>com.shuyu</groupId>
#   <artifactId>bbb</artifactId>
#   <version>1.0.0</version>
#   <type>pom</type>
# </dependency>

BINTRAY_USER= 你在bintray上的账号名
BINTRAY_KEY=  你在bintray上的API KEY
PROJ_USER_ORG=你在bintray上的组织名字
PROJ_USER_MAVEN=你在bintray上的repo名字
PROJ_NAME=你在bintray上的repo名字下的包名

PROJ_GROUP= 这是上的groupId，自己配置
PROJ_VERSION=这是上面的version，自己配置
PROJ_ARTIFACTID=上面的artifactId

PROJ_WEBSITEURL=github上的url就好了，可以不填
PROJ_ISSUETRACKERURL=可以不填
PROJ_VCSURL=github上的ssh就好了，可以不填
PROJ_DESCRIPTION=描述，可以不填


DEVELOPER_ID=发布人id，自己填
DEVELOPER_NAME=发布人名字，自己填
DEVELOPER_EMAIL=发布人邮箱，自己填
```
上面的例子最终在Android Studio中的引用形式为：
```
dependencies {
    compile 'com.shuyu:bbb:1.0.0'
}
```
它的格式是 `PROJ_GROUP:PROJ_ARTIFACTID:PROJ_VERSION`组成。

###4. 执行发布命令
执行 `gradlew bintrayUpload` 将库发布到 bintray.com.
```
gradlew bintrayUpload
```
###5. 将库加入Jcenter
最后一步，需要登录bintray.com，将我们刚刚发布的库申请加入到jcenter，这样别人才能直接引用到。

##参考自

* http://www.jianshu.com/p/e2cc4f66b1e7
* https://github.com/msdx/gradle-publish
