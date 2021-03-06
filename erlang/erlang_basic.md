Erlang basic

### 相关链接：
erlang下载：http://www.erlang.org/download.html
erlang 中文手册： http://erldoc.com/

### 基础语法

% 注释符号
. 表达式结束符号： . 
- 模块注解： ， -module(a). -export([test/0]).
_ 匿名变量，可作占位符，同一模式中的不同地方，各个_绑定的值不必相同。
$ 表示字符的整数值。
= 模式匹配，首次绑定。
变量：大写字母开头。单一赋值变量不能被改变，绑定变量。具体实现上，一个绑定变量就是一个指针，指向值的存储区。
```
Erlang 是函数式语言，不存在可变状态，没有共享内存，也没有锁。
```
/  返回浮点数
div 整除
rem 取余
原子 表示不同的非数字常量值。全局有效，不需要使用宏定义或者包含文件。一个原子的值就是原子自身。
> 小写字母开头，后跟数字字母或下划线或邮件符号@，
> 单引号引起来的字符，首字母可以大写。可以将不需要使用引号的原子引起来。'a'与a等同。

元组：{} 可以嵌套。

erlang 使用垃圾搜集器回收没有使用的内存。元组不再使用时自动销毁。元组可以引用已绑定的变量。
可使用相同结构的模式来提取元组中的字段，只需要在提取的字段位置上使用未绑定的变量。
```
Pointt ={point,10,45}.
{point,X1,Y1}=Pointt.
{point,_,Y2}=Pointt.
```
列表：[] 方括号括起来逗号分隔的一组值。列表中各个元素可以有不同的类型。列表的第一个元素称作头，可以是任何东西，剩下的部分称作尾，通常还是一个列表。
[...|T] 构造列表，T也应当是一个列表，为正规列表，大多数库函数不能正确的处理非正规列表。
[H|T],H为列表的头，T为列表的尾。
[X|Y]=L，XY为自由变量，可以把列表的头提取到X，尾提取到Y。

字符串实际上是一个整数列表。双引号括起来一串字符。shell打印一串列表值时，只有列表中的所有整数都是可打印字符，才把这个列表当作字符串打印出来。
f():释放所有绑定过的变量。

函数
字句以分号隔开，最后的字句以句点做为结束符。每一个字句都有函数头和函数体。
函数头由函数名和随后以括号括起来的模式组成。函数体则由一系列表达式组成。如果函数头中的模式与调用参数匹配成功的话，对应表达式就会进行运算。模式将按他们出现在函数中的先后顺序进行匹配。

geometry.erl

-module(geometry).
-export([area/1]).
area({rectangle,Width,Ht})-> Width * Ht;
area({circle,R}) -> 3.14159 * R * R.

编译运行：
c(geometry)
geometry:area({rectangle,10,5}).

还需要考虑容错。
