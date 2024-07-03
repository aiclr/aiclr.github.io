+++
title = 'spring-framework'
date = '2024-05-26'
author = 'aiclr'
categories = ['develop']
tags = ['spring']
Summary='The Spring Framework provides a comprehensive programming and configuration model for modern Java-based enterprise applications - on any kind of deployment platform.'
+++

[Spring](https://spring.io/) \
[Spring Framework](https://spring.io/projects/spring-framework) \
[Spring Boot](https://spring.io/projects/spring-boot) \
[Spring Cloud](https://spring.io/projects/spring-cloud) \
[Spring Guides](https://spring.io/guides) \
[Spring initializr](https://start.spring.io/)

fork into [my github](https://github.com/aiclr/spring-framework)

下载源码
```sh
git clone git@github.com:aiclr/spring-framework.git
```
切换到想编译的分支
```sh
git checkout -b 5.3.x origin/5.3.x
```

## [5.3.20](https://github.com/aiclr/spring-framework/tree/5.3.20)

[编译源码 官方说明文档](https://github.com/spring-projects/spring-framework/wiki/Build-from-Source)

env:
- archlinux
- git
- jdk / openjdk 11
- idea 2022.2

直接运行 ***./gradlew build*** 不出意外的话会出现下面列出的[意外](#error)
解决问题后使用 ***idea** 打开项目

### error

执行命令
```sh
./gradlew clean build
```
异常日志
```text
spring-framework/spring-core/src/main/java/org/springframework/core/CoroutinesUtils.java:74: warning: [deprecation] isAccessible() in AccessibleObject has been deprecated
                if (method.isAccessible() && !KCallablesJvm.isAccessible(function)) {
                          ^
error: warnings found and -Werror specified
1 error
1 warning
```

解决方案: \
添加 ***@SuppressWarnings("deprecation")***
```sh
vim spring-core/src/main/java/org/springframework/core/CoroutinesUtils.java
```
code:
```java
	/**
	 * Invoke a suspending function and converts it to {@link Mono} or
	 * {@link Flux}.
	 */
	@SuppressWarnings("deprecation")
	public static Publisher<?> invokeSuspendingFunction(Method method, Object target, Object... args) {
		KFunction<?> function = Objects.requireNonNull(ReflectJvmMapping.getKotlinFunction(method));
		if (method.isAccessible() && !KCallablesJvm.isAccessible(function)) {
			KCallablesJvm.setAccessible(function, true);
		}
        //......
    }

```

## bug

### spring 文件是否为空 `boolean isEmpty();`

- org.springframework.web.multipart.MultipartFile
	- org.springframework.web.multipart.support.StandardMultipartHttpServletRequest
	- org.springframework.web.multipart.commons.CommonsMultipartFile
	- org.springframework.mock.web.MockMultipartFile
- MultipartFile 的 isEmpty() 方法是根据文件大小进行判断，不是判断对象是否为 null。 规范使用防止 NPE

### source code

```java
    //StandardMultipartHttpServletRequest
    @Override
    public boolean isEmpty()
    {
        return (this.part.getSize() == 0);
    }
    //CommonsMultipartFile
    @Override
    public boolean isEmpty()
    {
        return (this.size == 0);
    }
    //MockMultipartFile
    @Override
    public boolean isEmpty()
    {
        return (this.content.length == 0);
    }
```