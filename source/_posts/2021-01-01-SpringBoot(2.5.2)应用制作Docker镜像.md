---
title: SpringBoot(2.5.2)应用制作Docker镜像
date: 2021-01-01 08:29:53
categories:
- SpringBoot
- Docker
tags:
- SpringBoot
- Docker
---

#### 1.创建SpringBoot项目

![在这里插入图片描述](https://raw.githubusercontent.com/ChangNingbo/blog_img/main/img202208081315925.png)

 - 版本请选择2.5.2,不同版本对应的方式略有不同 
 - 选择Spring Web以及Spring Boot Actuator组件
![在这里插入图片描述](https://raw.githubusercontent.com/ChangNingbo/blog_img/main/img202208081315904.png)

最后点击Finish即可

#### 2. 创建接口

我们会在启动类下写一个Restful接口,以供我们后期测试

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@SpringBootApplication
@RestController
public class DockerDemoApplication {

    public static void main(String[] args) {
        SpringApplication.run(DockerDemoApplication.class, args);
    }

    @GetMapping("/demo")
    public String dockerDemo(){
        return "docker success";
    }

}
```

 #### 3. 修改pom.xml文件

```
 <build>
        <!--指定打包后的finalName-->
        <finalName>docker-demo</finalName>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <configuration>
                    <!--Spring Boot 2.3.0以后版本制作Docker镜像需添加此配置-->
                    <layers>
                        <enabled>true</enabled>
                    </layers>
                </configuration>
                <executions>
                    <execution>
                        <goals>
                            <goal>repackage</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>

        </plugins>
    </build>
```

#### 4.编写Dockerfile文件

```
FROM java:8

ENV SERVER_PORT 8999

## docker-demo.jar即pom.xml文件中配置的finalName

ADD  target/docker-demo.jar app.jar

ENTRYPOINT ["java", "-jar","/app.jar"]

```

#### 5. 上传镜像

```
docker build -t docker-demo .
```

#### 6. 重命名镜像

```bash
docker tag IMAGEID(镜像id) REPOSITORY:TAG（仓库：标签）
```

#### 7. 运行

```
docker run -itd --name docker-demo -p 8999:8999 docker-demo
```

#### 8. 浏览器访问
![在这里插入图片描述](https://raw.githubusercontent.com/ChangNingbo/blog_img/main/img202208081315904.png)