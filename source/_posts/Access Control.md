---
title: Access Control
date: 2018-12-01 19:47:39
tags: [Java, Back-end]
categories: Java
---

| Modifier       | Class | Package | Subclass | World |
| -------------- | ----- | ------- | -------- | ----- |
| public         | Y     | Y       | Y        | Y     |
| protected      | Y     | Y       | Y        | N     |
| *No Modifiler* | Y     | Y       | N        | N     |
| private        | Y     | N       | N        | N     |

+ The first data column indicates whether the class itself has access to the member defined by the access level. As you can see, a class always has access to its own members. 

+ The second column indicates whether classes in the same package as the class (regardless of their parentage) have access to the member. 

+ The third column indicates **whether subclasses of the class declared outside this package have access to the member**.  Also named *package private*

+ The fourth column indicates whether all classes have access to the member.

[Access Control Tutorial](https://docs.oracle.com/javase/tutorial/java/javaOO/accesscontrol.html/)

