# Babel 常见的 AST 节点

## Literal

Literal 是字面量的意思，比如 let name = 'guang'中，'guang'就是一个字符串字面量 StringLiteral。相应的还有 NumbericLiteral 数字字面量等等，可以简单的总结为 类型 + Literal，比如布尔字面量：BooleanLiteral

## Identifier
Identifer 是标识符的意思，`变量名、属性名、参数名等各种声明和引用的名字`，都是Identifer。

## Statement
statement 是语句，它是可以独立执行的单位。我们写的每一条可以独立执行的代码都是语句。

他们对应的 AST 节点有：

1. break: BreakStatement
1. return: ReturnStatement
1. {}: BlockStatement
...

总结一下也是： name + Statement

## Declaration
声明语句是一种特殊的语句，它执行的逻辑是在作用域内声明一个变量、函数、class、import、export 等。

比如：
1. cosnt a = 1 : VariableDeclaration
1. function b(){} : FunctionDeclaration
...

## Expression
expression 是表达式，特点是执行完以后有返回值，这是和语句 (statement) 的区别。

下面是一些常见的表达式:

```js
[1,2,3]
a = 1
1 + 2;
-1;
function(){};
() => {};
class{};
a;
this;
super;
a::b;
```

其实有的节点可能会是多种类型，我们判断 AST 节点是不是某种类型要看它是不是符合该种类型的特点，比如语句的特点是能够单独执行，表达式的特点是有返回值。

表达式语句解析成 AST 的时候会包裹一层 ExpressionStatement 节点，代表这个表达式是被当成语句执行的。

表达式的特点是有返回值，有的表达式可以独立作为语句执行，会包裹一层 ExpressionStatement。

## Class

class 的语法也有专门的 AST 节点来表示。

整个 class 的内容是 ClassBody，属性是 ClassProperty，方法是ClassMethod（通过 kind 属性来区分是 constructor 还是 method）。

## Modules

es module 是语法级别的模块规范，所以也有专门的 AST 节点。

## import

## export

## Program & Directive

## File & Comment
