---
title: 2020.11.14本周总结
date: 2020-11-14 18:17:40
tags: [总结]
categories: [总结]
description: 啥都有
---

本周遇到了五个问题，三个react的，剩下的是业务上的

#### 1. input change问题修复
文件上传部分是手写的，对于选择文件的变化一般是监听input(type="file")的change事件，但是当两次选择的文件的一样的时候是不会触发change事件的。如何修复它呢？借用react中的key，change事件中改变input的key，会使react重新创建input，保证每次的input的都是不一样的，就不会有上述的问题了
```js
const Rc = () => {
  const inputKey = useRef(v4())

  const changeFile = () => {
    inputKey.current = v4()
  }

  return (
    <input type="file" key={inputKey.current} onChange={changeFile} />
  )
}
```

#### 2. for中循环改变state
现在所有的开发已经全部拥抱hook了，除非hook搞不定。目前的场景是在一个函数中循环改变state的值，在class中使用前一定要先取值，但是hook的话在一个函数中，直接不取就直接用了，导致值只会改变一次，因为在函数运行的时候，state的值就行了。想清楚这个，我需要定义一个局部变量作为每次循环中的累积。
```js
const Rc = () => {
  const [tab, setTab] = useState(0)
  const handle = () => {
    let newTab = tab

    for (...) {
      newTab = ...
      setTab(newTab)
    }
  }
}
```

#### 3. useEffect对待多个effect不同的操作
首先需求上显示这并不是一个复杂的组件，所以还是选择hook而不是class来组织。但是遇到的问题是，两个fetch方法会被三个值影响，两个props，一个state，有的值改变会导致两个fetch，有的值会导致一个fetch。在class中你可以通过prev和current进一步区别到底是哪个值改变，但是hook是没有这个记忆功能的，一个组件都写完了我也不想改成class了，最后的解决是定义一个ref来保存之前的prev
```js
const Rc = () => {
  const prevTab = useRef(default)

  useEffect(() => {
    if (prevTab !== tab) {
      fetch1()
    }
    fetch2()
  }, [tab, value2, value3])
}

```


#### 4. 业务中二级关系变成了三级
这个在需求中应该是breakchange，按照正常的迭代节奏，必须数据库的表结构必须要兼容之前的老数据。如果当作新需求进行表结构设计非常简单。和后端同学交流后，考虑到灰度到上线两天内的数据，就必须要兼容，所以技术方案和之前比较纯粹的比显得复杂。

#### 5. 在包含不重复的数字数组中随机取出n个的数字
对待这个问题，想到一个最简单粗暴的方法，使用random + set，使用set的不存在重复值的特性
```js
const getNnumber = (list, n) => {
  const { length } = list
  const nums = new Set()

  while(nums.size < n) {
    const index = Math.ceil(Math.random() * length)
    nums.add(nums[index])
  }

  return nums
}
```
同事提供的方法是[这个](https://blog.csdn.net/qq_28476865/article/details/102606852)

不过在stackoverflow上又找到了另一个方法：可以先混洗数组，然后取前n个[how-to-get-random-elements-from-an-array](https://stackoverflow.com/questions/7158654/how-to-get-random-elements-from-an-array/7158691)，这个方法看起来挺好👍

```js
numbers.sort( function() { return 0.5 - Math.random() } );
```

