---
layout: post
title: Writing a task manager on Java and React - Part 1
date: 2022-11-19 21:45:00 +0400
categories: java react tutorial
---

Hello everybody! Today I'm going to start a series of articles about writing a task manager application from scratch!
I believe these tutorials will be helpful for those people who want to start learning programming but don't know how and what to do. Being a follower of the principle that the best way to learn something is practice, I decided to share my knowledge and experience here.
Hope it will be interesting. Let's start!

To start off with, we have to prepare our environment. We need the following tools:
- IntelliJ IDEA Community Edition
- Docker + Docker-Compose plugin
- Git
- Hands :)

After installing the essential tools let's create a project. We are not going to do it manually, there is an amazing Spring Initializer ([https://start.spring.io](https://start.spring.io)) that can do all dirty job for us. Configure all fields as it shown below:

![Create project](/assets/img/spring-initializer.png)

We are creating a project using the latest OpenJDK 19 and Maven. Why Maven? Because unfortunately, on the moment of writing this article there is no Gradle version that supports JDK 19. I decided not to torture you by installing a custom version of JDK. Don't let Maven bother you, Maven and Gradle are the same, but Gradle requires less boilerplate. If you learn Maven, you will move to Gradle easily :) 

Press "Generate" and download the archive file with pre-generated structure of project. Then extract the archive to any folder on your disk (I chose `~/dev/samples/task-manager`) and open it in your IDEA using Open Project function. You should get the next project structure:

![Initial project structure](/assets/img/initial-project-structure.png)

Let's break it down in detail:
1. `.mvn` - this folder contains a wrapper of specific version of Maven. We don't need it in our work. It is usefull in case when you're developing the application for some specific version of Maven and you expect that others can try to start your project on machines without this version. This wrapper can make their life simplier. We can just remove this folder.
2. `TaskManagerAplication.java` - the root file of our application. This is the entry point of the application. We shouldn't touch it. The file must be similar to this:
    ![TaskManagerApplication.java](/assets/img/initial-main.png) 
3. `application.properties` - this is the configuration file where we will place the settings of our application. By default there is nothing. Let's rename it to application.yml in order to use more flexible YAML syntax - Spring can operate both of these config types.
4. `TaskManagerApplicationTests.java` - the blank for tests. We are going to write tests in our tutorials too. This file must look so:
    ![TaskManagerApplicationTests.java](/assets/img/initial-tests.png) 
5. `HELP.md, mvnw, mvnw.cmd` - useless trash (in our case), just remove it. They need only if you don't have Maven on your machine and don't want to install it.
6. `pom.xml` - very important file. All packages that we will include to our application during the development are stored there. It is the main config of Maven. By default it looks so:
   ![Initial pom.xml](/assets/img/initial-pom.png) 
7. `.gitignore` - file that defines which files are excluded from Git tracking. We are going to become familiar with Git and .gitingore a bit later.
8. `task-manager.iml` - file with IDEA's metadata, don't care about it (but don't delete it too!).

Thus, after removing all useless files and folders we should get the following project structure:

![Fixed structure](/assets/img/fixed-initial-structure.png)

We have one last step before start to write code. In order to compile our project and test that it works correctly we need to create build configuration in IDEA. Find on the top left corner of the screen a selector with text "Current file", press it and choose "Edit configurations".

![Create build configuration 1](/assets/img/create-build-configuration-1.png)

Click "Add configuration" and choose "Maven" from drop-down list. You will get such pre-defined configuration:

![Create build configuration 2](/assets/img/create-build-configuration-2.png)

In the "Run" field put `spring-boot:run --debug`. This is the command that launchs the application. Parameter `--debug` enriches our console output by Spring's debug information, it might be helpful if you got some unexpected exceptions.
Now we can run our application. The console will be opened and you should get the following text:

```
  .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::                (v2.7.5)

2022-11-19 23:49:32.134  INFO 29946 --- [           main] c.g.m.t.TaskManagerApplication           : Starting TaskManagerApplication using Java 19.0.1 on masleshov-ws with PID 29946 (/home/masleshov/dev/samples/task-manager/task-manager/target/classes started by masleshov in /home/masleshov/dev/samples/task-manager/task-manager)
2022-11-19 23:49:32.137  INFO 29946 --- [           main] c.g.m.t.TaskManagerApplication           : No active profile set, falling back to 1 default profile: "default"
2022-11-19 23:49:32.597  INFO 29946 --- [           main] c.g.m.t.TaskManagerApplication           : Started TaskManagerApplication in 0.79 seconds (JVM running for 1.063)
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  2.591 s
[INFO] Finished at: 2022-11-19T23:49:32+04:00
[INFO] ------------------------------------------------------------------------

Process finished with exit code 0
```

It means that our project has been compiled and we are ready to write our task manager!
