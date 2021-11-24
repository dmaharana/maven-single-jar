# maven-single-jar
maven single jar with dependencies comparision

https://github.com/jinahya/executable-jar-with-maven-example


```
mvn archetype:generate -DgroupId=com.mkyong.core.utils -DartifactId=dateUtils
 -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
```

https://stackoverflow.com/questions/9689793/cant-execute-jar-file-no-main-manifest-attribute

1. With Spring boot jar size is 120K
   
2. With Shade plugin size is 2.6K
   
3. With Assembly plugin size is 2.5K
   
4. With Maven jar plugin size is 2.4K
   

Changes to POM file
1. Spring boot
`
+       <parent>
+               <groupId>org.springframework.boot</groupId>
+               <artifactId>spring-boot-starter-parent</artifactId>
+               <version>2.3.4.RELEASE</version>
+               <relativePath /> <!-- lookup parent from repository -->
+       </parent>

+       <build>
+               <plugins>
+                       <plugin>
+                               <groupId>org.springframework.boot</groupId>
+                               <artifactId>spring-boot-maven-plugin</artifactId>
+                       </plugin>
+               </plugins>
+       </build>
`
2. Shade plugin
`
+       <build>
+               <plugins>
+                       <plugin>
+                               <groupId>org.apache.maven.plugins</groupId>
+                               <artifactId>maven-shade-plugin</artifactId>
+                               <version>2.0</version>
+                               <executions>
+                                       <execution>
+                                               <phase>package</phase>
+                                               <goals>
+                                                       <goal>shade</goal>
+                                               </goals>
+                                               <configuration>
+                                                       <transformers>
+                                                               <transformer
+                                                                       implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
+                                                                       <mainClass>in.home.JenkinsStatus</mainClass>
+                                                               </transformer>
+                                                       </transformers>
+                                               </configuration>
+                                       </execution>
+                               </executions>
+                       </plugin>
+               </plugins>
+       </build>
`
3. Assembly plugin
`
+       <build>
+               <plugins>
+                       <plugin>
+                               <artifactId>maven-assembly-plugin</artifactId>
+                               <executions>
+                                       <execution>
+                                               <phase>package</phase>
+                                               <goals>
+                                                       <goal>single</goal>
+                                               </goals>
+                                       </execution>
+                               </executions>
+                               <configuration>
+                                       <archive>
+                                               <manifest>
+                                                       <addClasspath>true</addClasspath>
+                                                       <mainClass>in.home.JenkinsStatus</mainClass>
+                                               </manifest>
+                                       </archive>
+                                       <descriptorRefs>
+                                               <descriptorRef>jar-with-dependencies</descriptorRef>
+                                       </descriptorRefs>
+                               </configuration>
+                       </plugin>
+               </plugins>
+       </build>
`
4. Maven jar plugin
`
+       <build>
+               <plugins>
+                       <plugin>
+                               <!-- Build an executable JAR -->
+                               <groupId>org.apache.maven.plugins</groupId>
+                               <artifactId>maven-jar-plugin</artifactId>
+                               <version>3.1.0</version>
+                               <configuration>
+                                       <archive>
+                                               <manifest>
+                                                       <addClasspath>true</addClasspath>
+                                                       <classpathPrefix>lib/</classpathPrefix>
+                                                       <mainClass>in.home.JenkinsStatus</mainClass>
+                                               </manifest>
+                                       </archive>
+                               </configuration>
+                       </plugin>
+               </plugins>
+       </build>
`
