# Springdoc + Native Sample Repository

Single purpose repository used to demonstrate a bug somewhere in the project, that appears to be based around Springdoc.
The presence of any springdoc-openapi library (including springdoc-openapi-native) breaks native compilation.

To reproduce:

1. Ensure Springdoc libraries are commented out in `build.gradle`
2. Run `./gradlew bootBuildImage` - this should succeed
3. Verify image runs with `docker run docker.io/library/demo:0.0.1-SNAPSHOT`
4. Uncomment springdoc-openapi libraries
5. Run `./gradlew bootBuildImage` - this should fail with error:


```
> Task :generateAot
2022-01-11 14:31:17.212  INFO 23744 --- [           main] o.s.a.build.ContextBootstrapContributor  : Detected application class: com.example.demo.DemoApplication
2022-01-11 14:31:17.215  INFO 23744 --- [           main] o.s.a.build.ContextBootstrapContributor  : Processing application context
2022-01-11 14:31:21.104  INFO 23744 --- [           main] .s.d.r.c.RepositoryConfigurationDelegate : Bootstrapping Spring Data JPA repositories in DEFAULT mode.
2022-01-11 14:31:21.170  INFO 23744 --- [           main] .s.d.r.c.RepositoryConfigurationDelegate : Finished Spring Data repository scanning in 25 ms. Found 0 JPA repository interfaces.
2022-01-11 14:31:27.240  INFO 23744 --- [           main] o.s.a.build.ContextBootstrapContributor  : Processed 404 bean definitions in 10020ms
2022-01-11 14:31:27.627  INFO 23744 --- [           main] o.s.nativex.support.SpringAnalyzer       : Spring Native operating mode: native
2022-01-11 14:31:37.070  INFO 23744 --- [           main] o.s.aop.framework.ProxyGenerator         : Generating class file bytes for a proxy named org.springdoc.webmvc.ui.SwaggerUiHome$$SpringProxy$92421e34
2022-01-11 14:31:37.464  INFO 23744 --- [           main] o.s.aop.framework.ProxyGenerator         : Generating class file bytes for a proxy named org.springdoc.webmvc.api.OpenApiWebMvcResource$$SpringProxy$92b3879f
2022-01-11 14:31:37.674  INFO 23744 --- [           main] o.s.aop.framework.ProxyGenerator         : Generating class file bytes for a proxy named org.springdoc.webmvc.ui.SwaggerConfigResource$$SpringProxy$70ccf26d
2022-01-11 14:31:37.706  INFO 23744 --- [           main] o.s.aop.framework.ProxyGenerator         : Generating class file bytes for a proxy named org.springframework.boot.autoconfigure.web.servlet.error.BasicErrorController$$SpringProxy$a0f480bd
2022-01-11 14:31:37.763  INFO 23744 --- [           main] o.s.aop.framework.ProxyGenerator         : Generating class file bytes for a proxy named org.springframework.http.ResponseEntity$$SpringProxy$a5ccba95
2022-01-11 14:31:37.807  INFO 23744 --- [           main] o.s.aop.framework.ProxyGenerator         : Generating class file bytes for a proxy named org.springframework.web.servlet.ModelAndView$$SpringProxy$c41c6ba7
2022-01-11 14:31:37.873  INFO 23744 --- [           main] o.s.aop.framework.ProxyGenerator         : Generating class file bytes for a proxy named org.springdoc.webmvc.ui.SwaggerWelcomeWebMvc$$SpringProxy$71bfef47
2022-01-11 14:31:37.874  INFO 23744 --- [           main] o.s.aop.framework.ProxyGenerator         : Unable to proxy method [void org.springdoc.webmvc.ui.SwaggerWelcomeCommon.buildFromCurrentContextPath(javax.servlet.http.HttpServletRequest)] because it is package-visible across different ClassLoaders: All calls to this method via a proxy will NOT be routed to the target instance.
2022-01-11 14:31:37.926  INFO 23744 --- [           main] o.s.aop.framework.ProxyGenerator         : Generating class file bytes for a proxy named org.springdoc.webmvc.api.MultipleOpenApiWebMvcResource$$SpringProxy$1db58c2f
2022-01-11 14:31:37.951  INFO 23744 --- [           main] o.s.aop.framework.ProxyGenerator         : Generating class file bytes for a proxy named java.lang.String$$SpringProxy$4916742f
2022-01-11 14:31:37.952  INFO 23744 --- [           main] o.s.aop.framework.ProxyGenerator         : Unable to proxy method [byte[] java.lang.String.value()] because it is package-visible across different ClassLoaders: All calls to this method via a proxy will NOT be routed to the target instance.
2022-01-11 14:31:37.952  INFO 23744 --- [           main] o.s.aop.framework.ProxyGenerator         : Unable to proxy method [byte java.lang.String.coder()] because it is package-visible across different ClassLoaders: All calls to this method via a proxy will NOT be routed to the target instance.
2022-01-11 14:31:37.952  INFO 23744 --- [           main] o.s.aop.framework.ProxyGenerator         : Unable to proxy method [boolean java.lang.String.isLatin1()] because it is package-visible across different ClassLoaders: All calls to this method via a proxy will NOT be routed to the target instance.
2022-01-11 14:31:37.952  INFO 23744 --- [           main] o.s.aop.framework.ProxyGenerator         : Unable to proxy method [void java.lang.String.getBytes(byte[],int,byte)] because it is package-visible across different ClassLoaders: All calls to this method via a proxy will NOT be routed to the target instance.
java.lang.IllegalArgumentException: Cannot subclass primitive, array or final types: class java.lang.String
        at net.bytebuddy.ByteBuddy.subclass(ByteBuddy.java:406)
2022-01-11 14:31:37.952  INFO 23744 --- [           main] o.s.aop.framework.ProxyGenerator         : Unable to proxy method [void java.lang.String.getBytes(byte[],int,int,byte,int)] because it is package-visible across different ClassLoaders: All calls to this method via a proxy will NOT be routed to the target instance.
        at net.bytebuddy.ByteBuddy.subclass(ByteBuddy.java:379)
        at net.bytebuddy.ByteBuddy.subclass(ByteBuddy.java:276)
        at org.springframework.aop.framework.ProxyGenerator.getProxyBytes(ProxyGenerator.java:80)
        at org.springframework.aot.nativex.ConfigurationContributor.generateBuildTimeClassProxy(ConfigurationContributor.java:193)
        at org.springframework.aot.nativex.ConfigurationContributor.generateBuildTimeClassProxies(ConfigurationContributor.java:170)
        at org.springframework.aot.nativex.ConfigurationContributor.processBuildTimeClassProxyRequests(ConfigurationContributor.java:142)
        at org.springframework.aot.nativex.ConfigurationContributor.contribute(ConfigurationContributor.java:74)
        at org.springframework.aot.build.BootstrapCodeGenerator.generate(BootstrapCodeGenerator.java:91)
        at org.springframework.aot.build.BootstrapCodeGenerator.generate(BootstrapCodeGenerator.java:71)
        at org.springframework.aot.build.GenerateBootstrapCommand.call(GenerateBootstrapCommand.java:107)
        at org.springframework.aot.build.GenerateBootstrapCommand.call(GenerateBootstrapCommand.java:42)
        at picocli.CommandLine.executeUserObject(CommandLine.java:1953)
        at picocli.CommandLine.access$1300(CommandLine.java:145)
        at picocli.CommandLine$RunLast.executeUserObjectOfLastSubcommandWithSameParent(CommandLine.java:2352)
        at picocli.CommandLine$RunLast.handle(CommandLine.java:2346)
        at picocli.CommandLine$RunLast.handle(CommandLine.java:2311)
        at picocli.CommandLine$AbstractParseResultHandler.execute(CommandLine.java:2179)
        at picocli.CommandLine.execute(CommandLine.java:2078)
        at org.springframework.aot.build.GenerateBootstrapCommand.main(GenerateBootstrapCommand.java:112)
java.lang.IllegalStateException: Problem creating proxy with configuration: org.springframework.aop.framework.ProxyConfiguration@4916742f
        at org.springframework.aop.framework.ProxyGenerator.getProxyBytes(ProxyGenerator.java:94)
        at org.springframework.aot.nativex.ConfigurationContributor.generateBuildTimeClassProxy(ConfigurationContributor.java:193)
        at org.springframework.aot.nativex.ConfigurationContributor.generateBuildTimeClassProxies(ConfigurationContributor.java:170)
        at org.springframework.aot.nativex.ConfigurationContributor.processBuildTimeClassProxyRequests(ConfigurationContributor.java:142)
        at org.springframework.aot.nativex.ConfigurationContributor.contribute(ConfigurationContributor.java:74)
        at org.springframework.aot.build.BootstrapCodeGenerator.generate(BootstrapCodeGenerator.java:91)
        at org.springframework.aot.build.BootstrapCodeGenerator.generate(BootstrapCodeGenerator.java:71)
        at org.springframework.aot.build.GenerateBootstrapCommand.call(GenerateBootstrapCommand.java:107)
        at org.springframework.aot.build.GenerateBootstrapCommand.call(GenerateBootstrapCommand.java:42)
        at picocli.CommandLine.executeUserObject(CommandLine.java:1953)
        at picocli.CommandLine.access$1300(CommandLine.java:145)
        at picocli.CommandLine$RunLast.executeUserObjectOfLastSubcommandWithSameParent(CommandLine.java:2352)
        at picocli.CommandLine$RunLast.handle(CommandLine.java:2346)
        at picocli.CommandLine$RunLast.handle(CommandLine.java:2311)
        at picocli.CommandLine$AbstractParseResultHandler.execute(CommandLine.java:2179)
        at picocli.CommandLine.execute(CommandLine.java:2078)
        at org.springframework.aot.build.GenerateBootstrapCommand.main(GenerateBootstrapCommand.java:112)
Caused by: java.lang.IllegalArgumentException: Cannot subclass primitive, array or final types: class java.lang.String
        at net.bytebuddy.ByteBuddy.subclass(ByteBuddy.java:406)
        at net.bytebuddy.ByteBuddy.subclass(ByteBuddy.java:379)
        at net.bytebuddy.ByteBuddy.subclass(ByteBuddy.java:276)
        at org.springframework.aop.framework.ProxyGenerator.getProxyBytes(ProxyGenerator.java:80)
        ... 16 more


```