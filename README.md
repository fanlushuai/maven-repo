# maven-repo
maven 仓库

<repositories>

    <repository>
        <id>maven-repo</id>
        <url>https://raw.githubusercontent.com/fanlushuai/maven-repo/master/</url>
    </repository>

</repositories>


# 如何上传到github maven repo
- 1、settings.xml 文件中添加 

```

<server>
  <id>github</id>
  <password>{##github上添加的token，注意要加repo的权限，和用户名的权限！##}</password>
</server>
    
```    
- 2、pom.xml中添加

```
<build>
    <plugins>
        <!--1、生成到本地-->
        <plugin>
            <artifactId>maven-deploy-plugin</artifactId>
            <version>2.8.2</version>
            <configuration>
                <altDeploymentRepository>
                    repo.stage::default::file://${project.build.directory}/repo-stage
                </altDeploymentRepository>
            </configuration>
        </plugin>
        <!--2、上传到github-->
        <plugin>
            <groupId>com.github.github</groupId>
            <artifactId>site-maven-plugin</artifactId>
            <version>0.12</version>
            <configuration>
                <!--settings.xml文件里面配置，server-->
                <repositoryName>{maven-repo(github上作为maven仓库的repositoryName)}</repositoryName>
                <repositoryOwner>{github-username(github自己的名字)}</repositoryOwner>
                <server>github</server>
                <message>更新 ${project.groupId}-${project.artifactId}-${project.version}</message>
                <noJekyll>false</noJekyll>
                <!--指定分支，默认的是refs/heads/gh-pages -->
                <branch>refs/heads/master</branch>
                <includes>
                    <include>**/*</include>
                </includes>
                <!--上面的输出目录-->
                <outputDirectory>${project.build.directory}/repo-stage</outputDirectory>
                <force>false</force>
                <merge>true</merge>
            </configuration>
            <executions>
                <execution>
                    <goals>
                        <goal>site</goal>
                    </goals>
                    <!--注意绑定到deploy。site过程有异常-->
                    <phase>deploy</phase>
                </execution>
            </executions>
        </plugin>
    </plugins>
</build>


```
