---
title: 阿里巴巴Java开发手册(易忽略点记录)
date: 2017-12-27 10:02
categories: Java
tags: [Java]
---
最近在看阿里巴巴出版的<<阿里巴巴Java开发手册>>，于是写篇文章记录下编写代码时常忽略的地方，[下载地址]{http://techforum-img.cn-hangzhou.oss-pub.aliyun-inc.com/Java_1512024443940.pdf}

### 1、编程规约
- Service / DAO 层方法命名规约
1 ） 获取单个对象的方法用 get 作前缀。
2 ） 获取多个对象的方法用 list 作前缀。
3 ） 获取统计值的方法用 count 作前缀。
4 ） 插入的方法用 save/insert 作前缀。
5 ） 删除的方法用 remove/delete 作前缀。
6 ） 修改的方法用 update 作前缀。

- 使用索引访问用 String 的 split 方法得到的数组时，需做最后一个分隔符后有无
内容的检查，否则会有抛 IndexOutOfBoundsException 的风险。
```
String str = "a,b,c,,";
String[] ary = str.split(",");
// 预期大于 3，结果是 3
System.out.println(ary.length);

```
<!-- more -->
- 循环体内，字符串的连接方式，使用 StringBuilder 的 append 方法进行扩展
- 关于 hashCode 和 equals 的处理，遵循如下规则：
1） 只要重写 equals ，就必须重写 hashCode.
2） 因为 Set 存储的是不重复的对象，依据 hashCode 和 equals 进行判断，所以 Set 存储的对象必须重写这两个方法.
3） 如果自定义对象作为 Map 的键，那么必须重写 hashCode 和 equals 。
说明： String 重写了 hashCode 和 equals 方法，所以我们可以非常愉快地使用 String 对象作为 key 来使用.





