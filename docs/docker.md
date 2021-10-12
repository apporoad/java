### 环境装备
1. adoptopenjdk11
2. mvn

### add pom
```pom
<profiles>
        <profile>
            <id>docker</id>
            <build>
                <plugins>
                    <plugin>
                        <groupId>io.fabric8</groupId>
                        <artifactId>docker-maven-plugin</artifactId>
                        <configuration>
<!--                            <dockerHost>tcp://ip:2347</dockerHost>-->
                            <authConfig>
                                <username>registry 用户</username>
                                <password>registry 密码</password>
                            </authConfig>
                            <images>
                                <!-- 单个镜像配置 -->
                                <image>
                                    <!--镜像名(含版本号)-->
                                    <name>${project.name}:${project.version}</name>
                                    <!--registry地址,用于推送,拉取镜像-->
                                    <registry>ip:8888</registry>
                                    <!--镜像build相关配置-->
                                    <build>
                                        <!--使用dockerFile文件-->
                                        <dockerFile>${project.basedir}/Dockerfile</dockerFile>
                                    </build>
                                </image>
                            </images>
                        </configuration>
                    </plugin>
                </plugins>
            </build>
        </profile>
    </profiles>
```

### 创建dockerfile
```bash
cd ${project.basedir}
vim Dockerfile
```

```Dockerfile
# 基础镜像
FROM  xxxxxx:8889/adoptopenjdk:11.0.11_9-jre-openj9-0.26.0
# 作者信息
MAINTAINER test

ENV LISTEN_PORT=8888

# 创建工作目录
RUN mkdir -p /deploy/app
# 切换工作目录
WORKDIR /deploy/app
# 容器开放端口
EXPOSE ${LISTEN_PORT}
# 添加jar包
ADD ./target/*.jar ./demo.jar
# 构建 : docker build  -t test:2  .
# 容器启动执行命令  docker run -it --rm --env LISTEN_PORT=8089  -p 8803:8089 test:2 /bin/bash
# 访问  ip:8803/hi
ENTRYPOINT ["java", "-jar", "-Dserver.port=${LISTEN_PORT}", "demo.jar" ]

```


### cmds

```bash
#run 
mvn clean package docker:stop docker:remove docker:build docker:run  -Pdocker

# push
mvn clean package docker:remove  docker:build  docker:publish -Pdocker
```