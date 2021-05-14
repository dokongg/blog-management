---
layout:     post
title:      "[Java] Lambda"
date:       2021-05-12
categories: Java
---

코드 중간에 뜬금포로 중괄호를 둔 코드를 누가 물어봤다.

코드의 가독성을 좋게 하는 것도 있지만, 변수 범위를 정하기 위해 사용한다고 한다.

```java
{
  int a = 0;
}
System.out.println(a); // ERROR!
```
- - -
Java는 GC가 있는데 필요한 경우가 있을까? 생각해봐야겠다.   

+ 참고
(https://stackoverflow.com/questions/5466974/multiple-open-and-close-curly-brackets-inside-method-java)
