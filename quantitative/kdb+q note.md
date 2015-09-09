# KDB+Q NOTE


[TOC]



## Q语言介绍
> Q语言是专为量化投资和程序化交易开发的动态编程语言，兼具C++语言的灵活性和EasyLanguage语言的易用性，支持证券、期货、上海黄金交易所、渤海商品交易所所有指标的历史数据、实时行情、程序化交易；支持**恒生**、金仕达、顶点、金证、易盛、CTP、国外FIX等几乎所有的交易接口；同时还支持C++，C#、JAVA、MATLAB、R等多种语言的调用。
> 参考：[百度百科-Q语言](http://baike.baidu.com/item/Q%E8%AF%AD%E8%A8%80/18277998#viewPageContent "Q语言动态编程脚本")   
 

 

### 特点：

* 内存内的数据库：理解KDB的一种方式就是KDB是一个内存数据库，但拥有磁盘可持久化能力。
* 解释性语言 ：开发周期更短
* 列表是有顺序的 ：不同于数据库中的行，因为列表有序，所以数据表也有序
* 从右往左解析
* 面向表
* 面向列：关系型数据库按行处理数据和存储数据，kdb是按列存数据，对数据进行运算也是直接作用在列向量上。
* 强类型
* Null值拥有特殊含义
* 内置I/O的支持 
* 绝大部分的q结构和操作都可以理解为方法映射。对于一个函数，其输入值组成的域称为domain，值域称为range。


### 基础
* 如果一个g的值域是f的domain，那么这个函数组合可以写成f(g(x))。在下文中，我们称数学函数为map/mapping，称q方法为function。

* 变量: 在q中，变量的赋值符，并不是=，而是：。q变量必须以字母开头，可包含字母，_和数字。 

* 命名规则推荐: 使用动词表示操作符和方法，使用名词表示数据；使用较为简洁的名字。Use contexts for namespacing.比较推荐加空格的地方包括，逗号，分号和juxpositon后面。

* 注释：在q里面，语言的注释是通过/来完成的。另外注释符和表达式之间需要一个空格来分割。

* 变量类型：q的变量其实并不带类型信息。类型信息附属在变量的那种之上。
* 语言解析顺序：表达式从右向左解析。表达式是允许一个刚刚被赋值的变量参与运算的。

### 内置操作符
* 大部分情况下，系统内置的操作符都能够被写成函数的形式。eg:3+2  , +[3;2]
* 作符优先级:q中不存在优先级的概念。任何一个表达式都是自右往左进行解析。若改变，需括号。括号内的表达式总是被先执行。
* ～ 比较符：该操作的两端接受两个实体，返回一个bool值。主要是比较两个实体的数据类型和值大小。两者都满足后，返回1b，否则0b。
* 关系运算：往往应用在Atom上。一般情况下也返回bool值。
* = 和<>：相等运算，测量两个值的大小，允许值不同类，但要求类间可转化。因此symbol不可和char比较。比较运算还有：>  <  >=   <= 。
* not 取反：该运算总是将值与0进行比较。
* 基本数学运算：+ - * % 唯一要注意的是，q中除法不再使用/，因为/已经被用作注释。另外数学运算也要求运算的两侧都是Atom。
* 数学公式：sqrt, exp, log, xexp, xlog，mod, signum, reciprocal, floor, ceiling， abs，这些数学公式的输入可以是整数等各种数字类型，但返回值永远都是float。reciprocal 返回参数的倒数。signum返回数字的符号0或-1或1.mod可以处理浮点数，5.2 mod 1.1 = 0.8


### 函数分类
* monadic：只有一个参数
* dyadic：有两个参数
* nildic：没有参数

### 内置函数
* 方法的格式
> f:{[p1,p2...] e1;e2...;en} 注意方法中的语句是从左到右执行的。方法其实就是包括在{}这种的部分。所以方法可以没有名字。例如：{x+y}[4；3]
> ::[1 2 3] //返回1 2 3 本身，方法::是返回参数本身。
* 方法是名词，因此可以成为列表的元素，或者复制给其他名词。
* 方法的返回值即为最后一个语句的执行结果，或者是最后一个语句的值。
* 全局变量赋值：不能超过32个。通过::赋值 ， 例如f:{b::7;x*b}。一旦全局变量在方法体内被重新定义后，无论如何我们都不能分为外面的全局变量，除非通过set方法。例如：a:42  g:{a:98.6;`a set x}
* 方法的参数：一个没有中括号的方法体，默认接受三个参数，分别赋值给x,y,z。所以建议不要使用x,y,z作为你的变量。输入的参数数量极为该方法的valence。目前q的限制是8个。
* 作为map的方法:从list和function看来，取同一个值现在有两种方式，但是表示法可以是一致的。例如list的下标取法和function的参数取法。在q中，list和function都实现了map。

### Q data type 原子类型 Atom
* int型，使用4个字节保存数据。可正可负。
* short型，使用两个字节保存数据。结尾必须是h。
* float型，存储在8个字节中。结尾可以f也是可选的。float可保存15位进度的数据。pi:3.23423,浮点型数据，在q中还可以表示成科学表示法。f:1.23456789e-10
* real型，存储在4个字节中。结尾必须以e结尾。
* boolean的写法。0或者1，结尾是b。
* byte数据的表示，在q中是以0x开始，后面带两个16进制。
* char类型:将单个字符包含在双引号内。同时char还能包含特殊字符，包含特殊字符时，需要依靠反斜杠来输入这些变量。还可以使用8进制来表示char。如c:"\142".
* symbol类型:s1:`q ,symbol is the smallest unit of the data, can't be reduced.
* 无穷值:无限值有正负之分。无穷值也是有数学类型的。整数除以0，得到无限大，负数除以0得到无限小。当无穷值参与运算时，其原理是，转变成对于类型的最大bit表示，然后再进行运算。
* null值在sql语言中，表示缺少值。missing value。但是在q中随着类型的不同而有不同意义：char： 表示空字符,数学：表示missing value,二进制：表示值0.

### 列表
* 列表也是一个函数。该函数的domain是0-n,值就是列表的内容。
* 所有元素都在括号内，且用分号分隔开。不同顺序的列表所代表的含义不同。可赋值给变量。
* 列表分类：简单列表 simple list （指所有元素的类型统一）、普通列表 general list
* 普通列表转简单列表的方法：q对输入的列表在不损失精确度的情况下，尽量转化成simple list。
> 数字 转化成大的数据类型
> 日期 转化成第一个元素的类型
> 日期 转化成最后用符号指定后的类型 
> 01:02:03 12:34 11:59:59.999u --》 01:02 12:34 11:59
* 空列表:L:() L[]返回整个列表，由于::也表示空，所以L[::]取值相同。 so, there is no type id for null. 
* 单元素列表:L: enlist "a" 。不管enlist后面有几个元素，都将被认为是新普通列表中的单元素。
* 索引:从0开始，一直到-1 + count List.通过下标取值越界是不会出错的，但是赋值越界就会出错了。读取：L[i]，赋值：L[i]:`a ，赋值需注意，不能将simple list变成general list。索引除了可以是数字外，也可以是数字组成的列表，当然返回的结果也将是一个基于索引列表查找的结果列表。除了上述的index取法，还可以通过juxtaposition这类取法。即
L 1 2 或 L I
* 连接列表：逗号连接：1 2, 3 4 5 ；连接（）和x，总是可以返回x的内容。(enlist 2) ~ (),2 。
* List嵌套：List嵌套后，就出现了depth这个概念。Atom的depth是0，simple list的depth是1，以此类推即可。List嵌套的下标取法，可以写成L[1][2]或L[1;2]，此时下标中的;是不可省略的。
* List也可以认为是一种map。超出index范围的值其实是指向Null。这个时候List的mapping关系也可以理解成一个function。
* rectangular list - 长方形列表，是一种特殊的列表，其成员都是列表，而且成员列表的count函数返回值也是一致的。
* matrix是一个特别的rectangular list。matrix拥有多种维度.matrix要求成员的类型必须一致。维度为1的matrix就是simple list.维度为2的matrix可表示成
L2:("0923";"lkjh";"1234";"abcd") ，2维matrix的行，是指这个matrix的第2的元素。


## KDB+ 数据库操作

### Creating a table
```
q)n:1000000;
q)item:`apple`banana`orange`pear;
q)city:`beijing`chicago`london`paris;
q)tab:([]time:asc n?0D0;n?item;amount:n?100;n?city);
This code creates a table called tab which contains 1 million rows and 4 columns of random time-series sales data.
```

### Simple Query
> q)select from tab where item=`banana   
> q)select sum amount by city from tab 
> q)select sum amount by time.hh,item from tab 

### Sample 1 for create table and insert value
> trade:([]time:`time$();sym:`symbol$();price:`float$();size:`int$())
> `trade insert(09:30:00.000;`a;10.75;100)
> select sum size by sym from trade


## Reference
* http://code.kx.com/