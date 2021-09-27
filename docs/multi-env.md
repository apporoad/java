## 多环境切换
支持多环境配置不同的properties文件，实现开发环境方便切换

## steps
1. pom.xml配置
```xml
    <build>
        <resources>
            <resource>
                <directory>src/main/resources</directory>
                <includes>
                    <include>**/*.properties</include>
                    <include>**/*.yml</include>
                    <include>**/*.xml</include>
                </includes>
                <filtering>true</filtering>
            </resource>
        </resources>
    </build>
    <profiles>
        <!--开发环境-->
        <profile>
            <id>dev</id>
            <properties>
                <env>dev</env>
            </properties>
        </profile>
        <!--测试环境-->
        <profile>
            <id>fat</id>
            <properties>
                <env>fat</env>
            </properties>
        </profile>
        <!--生产环境-->
        <profile>
            <id>prod</id>
            <properties>
                <env>prod</env>
            </properties>
        </profile>
    </profiles>
```

2. 添加src/main/resources多环境文件
application-dev.properties
application-fat.properties
application-uat.properties
application-prod.properties

3. 配置application.properties
```xml
spring.profiles.active=@env@
```

4. 在 maven的profiles中选择你的环境


## 生产运行指定环境
```bash
java -jar -Dspring.profiles.active=test demo.jar
# 或者
java -jar --spring.profiles.active=test demo.jar
```