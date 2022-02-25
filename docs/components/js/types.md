# 内置类型

## 概述

ECMAScript 规范规定语言类型有六种 `Undefined，Null，Boolean，String，Number，和 Object`，
ES6 又添加了一种基本类型，叫`Symbol`。

其中 Object 是引用类型，其他是基本类型。他们的划分方式其实是其是否可以表示为固定长度，
比如`Undefined，Null，Boolean，String，Number` 这些可以有固定长度，因此是基本类型，并且保存到了栈上。
`Object` 由于不可预知长度，并且可以 mutate，因此算引用类型，会被分配到了另一块区域，我们称之为堆（heap）。
有同学就会疑惑了`String`类型的长度是不固定的啊？但是**字符串是不可变的，因此被认为有固定长度。** 就好比武术招式，就算你能一分钟内打一个招式七八遍，总归还是一招，因为你的招式没有变化。（:point_right::point_right:译注：如，JavaScript 中对字符串的操作一定返回了一个新字符串，原始字符串并没有被改变）。

![基本类型存储](../../assets/type1.png)
![引用类型](../../assets/type2.png)

## Undefined

一个没有被赋值的变量会有个默认值`undefined`。如果方法或者是语句中**操作的变量没有被赋值，则会返回undefined**

```
function test(t) {
  if (t === undefined) {
    return 'Undefined value!';
  }
  return t;
}
let x;
console.log(test(x));// expected output: "Undefined value!"
```

undefined是全局对象的一个属性。也就是说，它是全局作用域的一个变量。undefined的最初值就是原始数据类型undefined。

## 布尔类型

布尔表示一个逻辑实体，可以有两个值：`true` 和 `false`。

## Null 类型

值 null 是一个字面量，不像 undefined，它不是全局对象的一个属性。null 是表示缺少的标识，指示变量未指向任何对象。把 null 作为尚未创建的对象，也许更好理解。在 API 中，null 常在返回类型应是一个对象，但没有关联的值的地方使用。

**null 与 undefined 的不同点：**

> 通过一系列判断看看他们的不同点

```
typeof null        // "object" (typeof null的结果为Object的原因是一个bug。在 javascript 的最初版本中，使用的 32位系统，js为了性能优化，使用低位来存储变量的类型信息。)
typeof undefined   // "undefined"
null === undefined // false
null  == undefined // true
null === null // true
null == null // true
!null //true
isNaN(1 + null) // false
isNaN(1 + undefined) // true
```

> :star: 总结 :star:

- `typeof`判`null`得object；
- if判`null`得false；
- `null`与`undefined`隐式相等，但不全的等。
- `Number(null)`输出0；`null` + 123 等于123
- `String(null)`输出‘null’;`null` + 'abc' 得 'nullabc'

## Number

在 JavaScript 中, Number 是一种 定义为 64位双精度浮点型的数字数据类型。
关于`Number`就不做过多的讲述，需要注意的是 :warning: **`Number`与`String`相加会变成字符串拼接**,所以咯可别算钱呈现指数式增多哦:cop::open_hands::cop:

### BigInt

#### 简介

  据我所知`BigInt`目前是第3阶段提案， 一旦添加到规范中，它就是JS 第二个数字数据类型，也将是 JS 第8种基本数据类型。在工作还未有过多的使用，所以在这里就打肿脸充胖子了:no_mouth::no_mouth:，自做些简单描述。本人始终坚信不经一番寒彻骨，怎得梅花扑鼻香。

#### 描述

  BigInt 是一种内置对象，它提供了一种方法来表示大于 2^53 - 1 的整数。这原本是 Javascript中可以用 Number 表示的最大数字。BigInt 可以表示任意大的整数。

  >**可以用在一个整数字面量后面加 n 的方式定义一个 BigInt ，如：10n，或者调用函数BigInt()。**

```
  typeof 1n === 'bigint'; // true
  typeof BigInt('1') === 'bigint'; // true
```

```
const alsoHuge = BigInt(9007199254740991);
// ↪ 9007199254740991n

const hugeHex = BigInt("0x1fffffffffffff");
// ↪ 9007199254740991n

const hugeBin = BigInt("0b11111111111111111111111111111111111111111111111111111");
// ↪ 9007199254740991n

> const x = 2n ** 53n;
9007199254740992n
> const y = x + 1n;
9007199254740993n
```

## String

JavaScript 的 String 类型用于表示文本数据。它是一组 16 位无符号整数值的“元素”。String 中的每个元素在 String 中占据一个位置。第一个元素位于 index 0，下一个位于 index 1，依此类推。String 的长度是其中元素的数量。

与某些编程语言（例如 C）不同，JavaScript 字符串是不可变的。这意味着一旦创建了字符串，就无法修改它。

## Symbol

具有符号数据类型的值可以称为“符号值”。在 JavaScript 运行时环境中，通过调用函数创建符号值，该函数Symbol动态生成匿名的唯一值。符号可以用作对象属性。
符号可以有一个可选的描述，但仅用于调试目的。

以上是MDN官方描述，这句话我读了几遍也没摸到它的精髓，通过查资料才对这`Symbol`类型有个大概的理解。咱们结合代码去了解它:

> 每个从Symbol()返回的symbol值都是唯一的

```
let Sym1 = Symbol("Sym")
let Sym2 = Symbol("Sym")
console.log(Sym1 === Sym2) // returns "false"
```



## 参考文档

- [JavaScript 深入了解基本类型和引用类型的值](https://www.runoob.com/w3cnote/javascript-basic-types-and-reference-types.html)
- [MDN:JavaScript 数据类型和数据结构](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Data_structures)
