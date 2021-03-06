---
title: FreeCodeCamp 初级算法题 - 检查字符串结尾
date: 2017-03-20 10:18:14
tags: [FreeCodeCamp,初级,算法]
categories: FCC
---
# 检查字符串结尾 (Confirm the Ending)
## 题目链接
- [中文链接](https://www.freecodecamp.cn/challenges/confirm-the-ending)
- [英文链接](https://www.freecodecamp.com/challenges/confirm-the-ending)
- 级别：初级 (Basic Algorithm Scripting)

## 问题解释
- 这个 `function` 接收两个参数，第一个参数为待检查的字符串 `str`，第二个参数为要比较的目标字符串 `target`。需要确认 `str` 是否以 `target` 结尾，如果是则返回 `true`，否则返回 `false`
- 比如接收的是 `"Hello"` 与 `"o"`，那么输出就是 `true`。如果接收的是 `"Hello"` 与 `"e"`，那么输出就是 `false`

<!-- more -->

## 比较字符串的 substr, slice 与 substring 方法
既然说到截取字符串，还是先来比较一下 `substr`, `slice` 和 `substring` 这三个方法吧。一开始很容易搞混，其实并不复杂
- 大体上来说，这三个方法可以根据参数分为两类。第一类是 `substring` 和 `slice`，他们的两个参数分别是 Start Index (起始索引) 和 End Index (终点索引)。第二类是 `substr`，第一个参数是 Start Index (起始索引)，第二个参数是 Length (截取长度)
- 共同点在于，以上这三个方法，第二个参数都是可以省略的，而且，很显然，他们都返回截取后的字符串。另外，他们都不会改变原字符串
- 先来说 `substr`
- 首先，需要注意，它的第二个参数是截取长度，而非结尾的索引
- 第一个参数可以为负数，负数代表以结尾为基准
- 如果第二个参数为负数，那么会返回空字符 `""`
- 再来对比一下 `slice` 与 `substring`
- 相同点：
  - 如果第一个参数与第二个相等，那么 `slice` 与 `substring` 都返回空字符 `""`
  - 如果只传入了第一个参数，那么都会截取到终点
  - 如果任何一个参数大于字符串长度，那就会使用字符串长度作这个参数
- 不同点：
  - 如果传入负数，`substring` 会当成 0 去计算，而 `slice` 会以字符串结尾为参考进行计算
  - 如果第一个参数大于第二个，`substring` 会把两个参数换位然后计算，而 `slice` 则会返回空字符 `""`
- 因此，个人认为，如果要截取特定长度的字符串，就用 `substr`，否则就推荐用 `slice`。之所以更推荐 `slice`，是因为，首先它支持负数作为索引，在一些情况下会很方便。只要我们在用 `slice` 的时候，保证第一个数小于第二个数，那就不会出问题

# 基本解法
## 思路提示
- 这道题目难度不大，而且解决方案很多
- 我们不需要分割字符串，只需要用字符串的 `substr` 或 `slice` 方法，获取到 `str` 的最后 n 位，再与 `target` 比较即可
- 需要注意的是，由于 `target` 长度不定，因此要根据它的长度来截取 `str`

## 参考链接
- [String.substr()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/substr)
- [String.slice()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/slice)
- [String.substring()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/substring)

## 代码
```js
function confirmEnding(str, target) {
    // 计算出开始的索引
    var startIndex = str.length - target.length;
    // 从开始索引，一直截取到字符串终点
    var strEnd = str.slice(startIndex);
    return strEnd === target;
}
```

## 解释
- 根据上面的解释，其实写成 `str.substr(startIndex)` 或者 `str.substring(startIndex)` 都行
- 如果你不能理解，请打开浏览器 console 试试以上提到的三种方法

# 优化
## 思路提示
- 上面我们声明了两个变量，但其实完全可以不声明，直接返回结果就好了
- 既然 `slice` 可以传入负数作为参数，题目要求也是检测结尾，所以，可以直接从结尾来截取

## 代码
```js
function confirmEnding(str, target) {
    return str.slice(-target.length) === target;
}
```

## 解释
- 当然，把 `slice` 换成 `substr` 也是没问题的。只是，绝对不要换成 `substring`

# 利用正则来解决
## 参考链接
- [RegExp()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/RegExp)
- [RegExp.test()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/RegExp/test)
```js
function confirmEnding(str, target) {
    return RegExp(target + '$').test(str);
}
```

## 解释
- 注意，并不是说这种解法效率高，事实上正则效率很低。放在这里只是多提供一种思路
- 如果你不理解正则构造器 `RegExp` 的用法，请点击上面的链接读一读
- 举个例子，如果传入的 `target` 是 `"abc"`，那么通过 `RegExp` 构造器就生成了 `/abc$/`。`$` 符号就表示字符串结尾
- 然后我们用这个正则的 `test` 方法来测试 `str`，返回测试结果
