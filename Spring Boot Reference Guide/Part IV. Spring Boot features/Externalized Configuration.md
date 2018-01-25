
Spring Boot uses a very particular PropertySourceorder that is designed to allow sensible overriding
of values. Properties are considered in the following order:

1. Devtools global settings propertieson your home directory (~/.spring-boot-devtools.propertieswhen devtools is active).
2. @TestPropertySourceannotations on your tests.
3. @SpringBootTest#propertiesannotation attribute on your tests.
4. Command line arguments.
5. Properties from SPRING_APPLICATION_JSON(inline JSON embedded in an environment variable
or system property)
6. ServletConfiginit parameters.
7. ServletContextinit parameters.
8. JNDI attributes from java:comp/env.
9. Java System properties (System.getProperties()).
10. OS environment variables.
11. A RandomValuePropertySourcethat only has properties in random.*.
12. Profile-specific application propertiesoutside of your packaged jar (application-{profile}.propertiesand YAML variants)
13. Profile-specific application propertiespackaged inside your jar (application-{profile}.propertiesand YAML variants)
14. Application properties outside of your packaged jar (application.propertiesand YAML
variants).
15. Application properties packaged inside your jar (application.propertiesand YAML variants).
16. @PropertySourceannotations on your @Configurationclasses.
17. Default properties (specified using SpringApplication.setDefaultProperties).
To provide a concrete example, suppose you develop a @Componentthat uses a nameproperty:

```java
importorg.springframework.stereotype.*
importorg.springframework.beans.factory.annotation.*

@Component
public classMyBean {
    
    @Value("${name}")
    privateString name;
    // ...
}

```

----


SpringApplicationwill load properties from  application.propertiesfiles in the following
locations and add them to the Spring Environment:
1. A /configsubdirectory of the current directory.
2. The current directory
3. A classpath /configpackage
4. The classpath root
The list is ordered by precedence (properties defined in locations higher in the list override those defined
in lower locations).

----

Config locations are searched in reverse order. By default, the configured locations are
classpath:/,classpath:/config/,file:./,file:./config/. The resulting search order is:
1. file:./config/
2. file:./
3. classpath:/config/
4. classpath:/

----

When custom config locations are configured, they are used in addition to the default locations. Custom
locations are searched before the default locations. For example, if custom locations  classpath:/
custom-config/,file:./custom-config/are configured, the search order becomes:
1. file:./custom-config/
2. classpath:custom-config/
3. file:./config/
4. file:./
5. classpath:/config/
6. classpath:/

---- 


