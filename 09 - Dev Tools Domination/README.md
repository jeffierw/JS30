# 09 - Dev Tools Domination 笔记

> 介绍：[JavaScript30](https://javascript30.com) 是 [Wes Bos](https://github.com/wesbos) 推出的一个 30 天挑战。项目免费提供了 30 个视频教程、30 个挑战的起始文档和 30 个挑战解决方案源代码。目的是帮助人们用纯 JavaScript 来写东西，不借助框架和库，也不使用编译器和引用。完整笔记在 [GitHub](https://github.com/jeffierw/JavaScript30)

## 实现效果

熟悉[Console](https://developer.mozilla.org/zh-CN/docs/Web/API/Canvas_API)控制台对象的一些调试工具。

[在线查看效果](https://jeffierw.github.io/JavaScript30/09%20-%20Dev%20Tools%20Domination/index-YepW.html)按 F12 查看各种输出结果。

## console封装小技巧

工程开发时，可能打印了很多log，手动删除或注释很麻烦，像下面这样小小的封装一下，开发完毕后，isDev属性关闭后就OK了。
```js
        // 封装自己的console对象 用于开发模式 解决开发完成后删除和注释console很麻烦的问题 直接false掉isDev
        let console = {
            isDev: true,
            log(...args) {
                if (!this.isDev) return
                window.console.log(...args)
            },
            warn(...args) {
                if (!this.isDev) return
                window.console.warn(...args)
            },
            error(...args) {
                if (!this.isDev) return
                window.console.error(...args)
            },
            info(...args) {
                if (!this.isDev) return
                window.console.info(...args)
            },
            assert(...args) {
                if (!this.isDev) return
                window.console.assert(...args)
            },
            clear(...args) {
                if (!this.isDev) return
                window.console.clear(...args)
            },
            dir(...args) {
                if (!this.isDev) return
                window.console.dir(...args)
            },
            groupCollapsed(...args) {
                if (!this.isDev) return
                window.console.groupCollapsed(...args)
            },
            groupEnd(...args) {
                if (!this.isDev) return
                window.console.groupEnd(...args)
            },
            count(...args) {
                if (!this.isDev) return
                window.console.count(...args)
            },
            time(...args) {
                if (!this.isDev) return
                window.console.time(...args)
            },
            timeEnd(...args) {
                if (!this.isDev) return
                window.console.timeEnd(...args)
            },
            table(...args) {
                if (!this.isDev) return
                window.console.table(...args)
            },
        }
```

## console对象相关知识点

### 常见的.log方法

这个是最常用的，但它还有一些更多功能：比如参数支持类似 C 语言的字符串替换模式（建议还是用模板字符串啦）。

- `%s` 字符串
- `%d` 整数
- `%f` 浮点值
- `%o` Object
- `%c` 设定输出的样式，在之后的文字将按照第二个参数里的值进行显示

```js
console.log("输出一个字符串 %s ", "log");
console.log("输出一个整数是 %d ", 1.23); //1
console.log("输出一个小数是 %f ", 1.23); //1.23
console.log("%c不同样式的输出效果", "color: #00fdff; font-size: 2em;");
```

### 不同的输出效果

除了常规的 `log` 之外，还有一些其他已设定好的样式，区别在于图标或者颜色不一样：

```js
// warning!
console.warn("三角感叹号图标，淡黄色背景")
// Error :|
console.error("红叉图标，红字红色背景");
// Info
console.info("蓝色圆形感叹号图标"); // Chorme没有感叹号 FF是有的
```

### 打印输出 DOM 元素

获取 DOM 元素之后，也可以打印输出。

```js
const p = document.querySelector('p');
console.log(p);
console.dir(p);
```


不同的地方在于，`log` 输出这个 DOM  的 HTML 标签，而 `dir` 则会输出这个 DOM 元素的属性列表。

### 清空 console 面板输出内容

要清空已经打印输出的内容，有两种方式，一种是 JavaScript 语句： `console.clear()`。另一个是快捷键 Ctrl ＋ L。

### asset 方法进行测试

接受一个表达式作为参数，如果参数返回值是 false，则会输出第二个参数中的内容。

```js
console.assert(1 ===1, "（这句发布时删除）");
console.assert(1 ===0, "看看看，失策了吧");
console.assert(p.innerHTML.match("她"), "我这儿才没有她这个人");
```

### 以更清晰的形式输出数据

此前的文章中已经提到了 `console.table()` 方法，可以将数组、对象以表格的形式打印输出，如果只输出其中的某一列，可以加上第二个参数，示例如下。

```js
console.table(dogs);
console.table(dogs, ["age"]);
```

除了按表格，还可以将数据分组展示，直接看例子：

```js
const dogs = [{ name: 'Snickers', age: 2 }, { name: 'hugo', age: 8 }];
dogs.forEach(dog => {
	console.group();		
//	console.groupCollapsed();  // 收起列表
	console.log(`${dog.name}`);
	console.log(`${dog.age}`);
	console.log(`${dog.name} 有 ${dog.age} 岁了`);
	console.groupEnd();
});
```

这个例子中，`group()`/`groupCollapsed()` 与 `groupEnd()` 之间的内容会自动分组，区别在于是否自动展开。效果类似于 Excel 中的分类汇总的简易版。

### `count` 计数

这里的计数对象仅限于由 `count()` 输出的内容，并非所有 console 中的输出。

### `time` 计时

用 `time("name")` 和 `timeEnd("name")` 分别控制开始点和结束点，它们两的参数表示当前计时的名称，可以自定义但需要保持相同。所以如果想看异步获取数据花了多场时间，可以这样写：

````js
console.time('fetch my data');
fetch("https://api.github.com/users/soyaine")
  .then(data => data.json())
  .then(data => {
  console.timeEnd('fetch my data');
  console.log(data);
});
````

如果 timeEnd 中的名称如果和上面不一样，得到的数据是系统当前时间换算后的毫秒值。