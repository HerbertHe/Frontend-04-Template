# 第二周学习笔记

本周的学习内容是

- 广义优先算法寻找最短路径问题
- LL语法分析构建`四则运算`的AST（抽象语法树）

## 广义优先算法寻求路径的问题

### 绘制地图编辑器

根据第一周的学习，使用一维数组保存`100 * 100`的地图，监听鼠标的`按下`、`移动`、`松开`
的事件绘制地图，并通过`contextmenu`事件监听干掉默认的菜单

### 广度优先算法

编写异步函数定义路径搜索保存的数据结构为`FIFO`的队列数据结构

> `insert`函数

- [x] 进行数据越界检查
- [x] 进行是否map中是否存在数据，如果是`1`或者是`2`则跳过

> 如果队列的长度不为零则循环

- [x] 取出队列的第一个元素
- [x] 如果是终点则返回`true`
- [x] 分别递归检查`左`、`下`、`右`、`上`

> 遍历结束之后返回`false`

### 寻求路径

> 延时函数可以更好的显示过程

```javascript
function sleep(t) {
    return new Promise((resolve) => {
        setTimeout(resolve, t)
    })
}
```

> 修改原来的结构

- [x] 通过`Object.create()`函数copy地图以备后续使用
- [x] 保存每一个搜索的前一个节点
- [x] 遍历当到终点时，不是起点时将节点加入`path`中返回
- [x] 检查八个方向的下一个节点

### 启发式搜索

启发式搜索定义`Sorted`的数据结构，返回经过加权排序之后的队列数据，通过根据`距离`加权之后的数据可以更少的进行搜索，对于广度优先进行搜索性能的优化（`A* 算法`）

## LL语法分析构建AST（抽象语法树）

LL语法分析构建AST的关键在于两个方面 `词法分析` 和 `语法分析`

### 词法分析部分

在词法分析部分最重要的是 `正则表达式`

课上示例的正则表达式 `/([0-9\.]+)|([ \t]+)|([\r\n]+)|(\*)|(\/)|(\+)|(\-)/g` 即是全局分别匹配数字、空格或者TAB、回车换行和加减乘除的符号

`tokenize`函数运用了新的ECMAScript的Generator语法，每次将生成 的token对象`yield`出来，没有结果、匹配不合法或者越界的时候干掉循环，结束之后使用通用的 `EOF (End Of File)` 标识符表示匹配的结束

使用`for...of`的语法取出被`yield`出来的可迭代的对象，然后打印token对象

### 语法分析部分

语法分析是对于winter老师课上给的PPT的语法表达式的实现

实现`MultiplicativeExpression`，`MultiplicativeExpression`包含了乘和除的操作，首先处理开头是数字的情况，然后再递归调用`MultiplicativeExpression`函数进行再次对于后续的表达式进行判断，移除已被判断的表达式之后移除等待判断的source数组，以同样的方法不断递归进行调用，直到全部完成返回结果

实现`AdditiveExpression`的方式与`MultiplicativeExpression`其实并没有太大的区别，只不过在逻辑的`node.children.push(source.shift())`函数的第二个之后又调用了`MultiplicativeExpression`函数，因为乘除的优先级更高；在`AdditiveExpression`逻辑判断完成之后，再次调用`MultiplicativeExpression`的逻辑避免最后是`MultiplicativeExpression`的情况

自此，LL对于四则运算的语法分析部分也得以完成
