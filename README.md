---
description: 在object2文件里面只是简单的先引入了object1的声明，而没有引入实体，这样就能解决头文件相互包含的问题
---

# 两个头文件互相包含的问题

在object1.h中包含object2.h

\#include "object2.h"

class object1

{

}

在object2的头文件这么写

class object1

class object2

{

}

然后在object2的CPP文件中添加

\#include"object1.h"

****
