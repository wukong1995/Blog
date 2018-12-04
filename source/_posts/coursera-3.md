---
title: coursera第三周
date: 2017-09-03 00:02:29
tags: ['编程语言']
categories: ['编程语言']
---

### 淡淡的忧伤
three time 是三倍的意思

### 学习一门新的语言
目前学习一门新的语言仅仅停留在`Syntax`，但是真的要把代码写的优雅，你更要了解它的`Type-checking`和`Evalustion`，了解这两者之后，你能知道代码在哪里可以优化，而不是代码的优化是在写代码中或者看到别人的代码时看到的
目前这个方面需要改变，思想的转变...
人的思想都是在肤浅向深沉转变😄

###  应用类型
下面是一段java代码，乍看上去没有问题，但是使用`p.getAllowesUsers[0] = p.currentUser`，这一句代码使权限形同虚设。
原因是：`getAllowesUsers`中直接返回的是`allowedUsers`，我尝试修改`p.getAllowesUsers`的值, 实际上修改了示例的私有变量的`allowedUsers`的值
修改： 在`getAllowesUsers`中直接返回的是`allowedUsers`的一个克隆
```java
class ProtextedResource {
  private Resource theResource = ...;
  private String[] allowedUsers = ...;
  public String[] getAllowesUsers () {
    return allowedUsers;
  }
  public String currentUser() {
    // ...
  }
  public void useTheResource() {
    for(int i = 0; i< allowedUsers.length; i++) {
      if (currentUser().equals(allowedUsers[i])) {
        ... // access allowed: use it
        return ;
      }
    }
    throw new IllegalAccessException();
  }
}
```
之前在项目中，我定义了一个`default`的值，合并对象选的是`lodash`的`extend`,因为没有看文档就直接使用了，`extend(default, options)`，发生了bug。我的本意是default是一个不可变的值，它作为一个函数的默认值。经过长时间😅的调试，我发现`default`的值改变了，去看文档发现`extend`会改变第一个参数的值。于是更改为`extend({}, default, options)`
当应用类型作为函数的参数，一定要特别注意，尽量使用实参的clone值，要不在函数中修改参数的值，就相当于修改实参的值了...

### 尾递归
递归在一定程度上可以代替`loop`，一方面它看起来更优雅，另一方面...
下面的代码上面的函数是我们经常使用的方式，通俗易懂。下面的递归称为尾递归。两种函数实现的功能相同。但是若要分析到stack处，你会发现下面的函数比上面的函数有更好的性能；首先上面的函数涉及到每次递归都要去进栈和出栈后才能得到就过，但是下面的函数却没有这样的操作。
而且在`Evalustion`阶段，上方的函数最终结果的数据类型需要依赖上一步的结果；下面的函数时不需要上一步的计算结果
```ml
fun sum1 xs =
    case xs of
        [] => 0
      | i::xs’ => i + sum1 xs’

(* tail recursion*)
fun sum2 xs =
    let fun f (xs,acc) =
            case xs of
                [] => acc
              | i::xs’ => f(xs’,i+acc)
    in
    f(xs,0)
    end

```

就到这吧...😪
