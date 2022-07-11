# ES7~ES12新语法

## ES7 

### 1 Array.prototype.includes（常用）

用于判断数组中是否包含某个元素

**语法**

``arr.includes(valueToFind,index)``

第一个参数是要包括的值

第二个参数是开始搜索的位置

**例子**

```js
[1, 2, 3].includes(2);     // true
[1, 2, 3].includes(4);     // false
[1, 2, 3].includes(3, 3);  // false
[1, 2, 3].includes(3, -1); // true
[1, 2, NaN].includes(NaN); // true
```

`includes()` 方法有意设计为通用方法。它不要求 `this` 值是数组对象，所以它可以被用于其他类型的对象（比如类数组对象）。下面的例子展示了在函数的 `arguments` 对象上调用 `includes()` 方法。

### 2 Exponentiation Operator

求幂运算符（`**`）返回将第一个操作数加到第二个操作数的幂的结果。它等效于 `Math.pow`，不同之处在于它也接受 `BigInt` 作为操作数。 

> 该提案目前处于 Stage 3 阶段。

**语法**

 `var1 ** var2`

求幂运算符是是右结合的: `a ** b ** c` 等于 `a ** (b ** c)`。

**例子**

```js
2 ** 3   // 8
3 ** 2   // 9
3 ** 2.5 // 15.588457268119896
10 ** -1 // 0.1
NaN ** 2 // NaN

2 ** 3 ** 2   // 512
2 ** (3 ** 2) // 512
(2 ** 3) ** 2 // 64

-(2 ** 2) // -4
(-2) ** 2 // 4
```

## ES8

### 1 async和await（常用）

异步的语法糖

**语法**

```js
const asyncReadFile = async function () {
  const f1 = await readFile('/etc/fstab');
  const f2 = await readFile('/etc/shells');
  console.log(f1.toString());
  console.log(f2.toString());
};
```

### 2 Object.values和Object.entries

`Object.values()` 方法返回一个给定对象自身的所有可枚举属性值的数组，值的顺序与使用 for...in 循环的顺序相同（区别在于 for...in 循环枚举原型链中的属性）。

`Object.entries()` 方法返回一个给定对象自身可枚举属性的键值对数组，其排列与使用 for...in 循环遍历该对象时返回的顺序一致（区别在于 for...in 循环还会枚举原型链中的属性）。

> 改提案目前处于 Stage 4 阶段。

**语法**

```js
Object.value(obj)
Object.entries(obj)
```

**例子**

```js
Object.values({a: 1, b: 2, c: 3});  // [1, 2, 3]
Object.keys({a: 1, b: 2, c: 3});    // [a, b, c]
Object.entries({a: 1, b: 2, c: 3}); // [["a", 1], ["b", 2], ["c", 3]]
```

### 3 Object.getOwnPropertyDescriptors

获取一个对象所有自身属性的描述符，如果没有任何自身属性，则返回空对象。

> 该提案目前处于 Stage 4 阶段。

**语法**

``Object.getOwnPropertyDescriptors(obj)``

**例子**

```js
const a = { 
  b: 123,
  c: '文字内容',
  d() { return 2; }
};
Object.getOwnPropertyDescriptors(a);
// 输出结果
{
  b:{
    configurable: true,
    enumerable: true,
    value: 123,
    writable: true,
    [[Prototype]]: Object,
  }
  c:{
    configurable: true,
    enumerable: true,
    value: "文字内容",
    writable: true,
    [[Prototype]]: Object,
  }
  d:{
    configurable: true,
    enumerable: true,
    value: ƒ d(),
    writable: true,
    [[Prototype]]: Object,
  },
  [[Prototype]]: Object,
}
```

### 4 String.prototype.padStart 和 String.prototype.padEnd

`padStart()` 方法用另一个字符串填充当前字符串（如果需要的话，会重复多次），以便产生的字符串达到给定的长度。从当前字符串的开头（左侧）开始填充。

`padEnd()` 方法会用一个字符串填充当前字符串（如果需要的话则重复填充），返回填充后达到指定长度的字符串。从当前字符串的末尾（右侧）开始填充。

> 该提案目前处于 Stage 4 阶段。

**语法**

``str.padStart(targetLength, padString)``

- `targetLength`：当前字符串需要填充到的目标长度。如果这个数值小于当前字符串的长度，则返回当前字符串本身。
- `padString`：可选的。填充字符串。如果字符串太长，使填充后的字符串长度超过了目标长度，则只保留最左侧的部分，其他部分会被截断。此参数的默认值为 `" "`（U+0020）。

``str.padEnd(targetLength, padString)``

- `targetLength`：当前字符串需要填充到的目标长度。如果这个数值小于当前字符串的长度，则返回当前字符串本身。
- `padString`：可选的。填充字符串。如果字符串太长，使填充后的字符串长度超过了目标长度，则只保留最左侧的部分，其他部分会被截断。此参数的缺省值为 `" "`（U+0020）。

**例子**

```js
'abc'.padStart(10);         // "       abc"
'abc'.padStart(10, "foo");  // "foofoofabc"
'abc'.padStart(6,"123465"); // "123abc"
'abc'.padStart(8, "0");     // "00000abc"
'abc'.padStart(1);          // "abc"
```

```js
'abc'.padEnd(10);          // "abc       "
'abc'.padEnd(10, "foo");   // "abcfoofoof"
'abc'.padEnd(6, "123456"); // "abc123"
'abc'.padEnd(1);           // "abc"
```

## ES9

### 1 for await...of

循环迭代异步对象构成的数组等可迭代对象

**语法**

async function process(iterable) {  for await (variable of iterable) { // Do something. } }

- `variable`：在每次迭代中，将不同属性的值分配给变量。变量有可能以 const、let 或者 var 来声明。
- `iterable`：被迭代枚举其属性的对象。与 for...of 相比，这里的对象可以返回 Promise，如果是这样，那么 variable 将是 Promise 所包含的值，否则是值本身。

**例子**

```js
function getTime(seconds){
  return new Promise(resolve=>{
    setTimeout(() => {
      resolve(seconds)
    }, seconds);
  })
}
async function test(){
  let arr = [getTime(500), getTime(300), getTime(3000)];
  for await (let x of arr){
    console.log(x); // 500  300 3000 按顺序的
  }
}

test()
```

### 2 Promise.prototype.finally

`finally()` 方法返回一个 Promise。在 Promise 结束时，无论结果是 fulfilled 还是 rejected，都会执行指定的回调函数。这为在 Promise 是否成功完成后都需要执行的代码提供了一种方式。

这避免了同样的语句需要在 `then()` 和 `catch()` 中各写一次的情况。

**语法**

```js
Promise.resolve().then().catch(e => e).finally(onFinally);

onFinally：Promise 结束后调用的 Function。
```

**例子**

```js
let isLoading = true;
fetch(myRequest).then(function(response) {
  const contentType = response.headers.get("content-type");
  if(contentType && contentType.includes("application/json")) {
    return response.json();
  }
  throw new TypeError("Oops, we haven't got JSON!");
})
.then(function(json) { /* process your JSON further */ })
.catch(function(error) { console.log(error); })
.finally(function() { isLoading = false; });
```

注意：由于无法知道 promise 的最终状态，所以 finally 的回调函数中不接收任何参数，它仅用于无论最终结果如何都要执行的情况。

### 3 Object Rest/Spread Properties（常用）

将 `Rest` 语法添加到解构中。`Rest` 属性收集那些尚未被解构模式拾取的剩余可枚举属性键。

将 `Spread` 语法添加到对象字面量中。将已有对象的所有可枚举（ `enumerable` ）属性拷贝到新构造的对象中。

> 该提案目前处于 Stage 4 阶段。

**语法**

Rest

```javascript
const { x, y, ...z } = { x: 1, y: 2, a: 3, b: 4 };
x; // 1
y; // 2
z; // { a: 3, b: 4 }
```

Spread

```javascript
let n = { x, y, ...z };
n; // { x: 1, y: 2, a: 3, b: 4 }
```

## ES10

### 1 Array.prototype.flat 和 Array.prototype.flatMap

`flat()` 方法会按照一个可指定的深度递归遍历数组，并将所有元素与遍历到的子数组中的元素合并为一个新数组返回。

`flatMap()` 方法首先使用映射函数映射每个元素，然后将结果压缩成一个新数组。

`flatMap` 方法与 `map` 方法结合深度 `depth` 为 `1` 的 `flat` 几乎相同。

> 该提案目前处于 Stage 4 阶段。

#### 语法

arr.flat(depth)

- `depth`：可选的。指定要提取嵌套数组的结构深度，默认值为 `1`。

arr.flatMap( function callback( currentValue[, index[, array]]) { // return element for new_array }[, thisArg])

- callback：可以生成一个新数组中的元素的函数，可以传入三个参数：

  - `currentValue`：当前正在数组中处理的元素。

  - `index`：可选的。数组中正在处理的当前元素的索引。

  - `array`：可选的。数组中正在处理的当前元素的索引。

- `thisArg`：可选的。执行 callback 函数时 使用的 `this` 值。

**例子：**

```javascript
[1, 2, [3, 4]].flat(1); 
// [1, 2, 3, 4]

[1, 2, [3, 4, [5, 6]]].flat(2); 
// [1, 2, 3, 4, 5, 6]

[1, 2, [3, 4, [5, 6]]].flat(Infinity); 
// [1, 2, 3, 4, 5, 6]
```



**例子：**

```javascript
[1, 2, 3, 4].flatMap(a => [a ** 2]); 
// [1, 4, 9, 16]
map()` 与 `flatMap()
const arr1 = [1, 2, 3, 4];

arr1.map(x => [x * 2]);
// [[2], [4], [6], [8]]

arr1.flatMap(x => [x * 2]);
// [2, 4, 6, 8]

arr1.map(x => [x * 2]).flat(1);
// [2, 4, 6, 8]

// only one level is flattened
arr1.flatMap(x => [[x * 2]]);
// [[2], [4], [6], [8]]
let arr1 = ["it's Sunny in", "", "California"];

arr1.map(x => x.split(" "));
// [["it's","Sunny","in"],[""],["California"]]

arr1.flatMap(x => x.split(" "));
// ["it's","Sunny","in", "", "California"]
```

### 2 String.prototype.trimStart 和 String.prototype.trimEnd

去除字符串首尾的连续空白符。

`trimStart()` 方法从字符串的开头删除空白符。`trimLeft()` 是此方法的别名。

`trimEnd()` 方法从字符串的结尾删除空白符。`trimRight()` 是此方法的别名。

> 该提案目前处于 Stage 4 阶段。

**例子**

```js
const greeting = '   Hello world!   ';
console.log(greeting.trimStart());
// "Hello world!   ";
console.log(greeting.trimEnd());
// "   Hello world!";
```

## ES11

### 1 Dynamic Import

动态导入模块

标准用法的 `import` 导入的模块是静态的，会使所有被导入的模块，在加载时就被编译（无法做到按需编译，降低首页加载速度）。有些场景中，你可能希望根据条件导入模块或者按需导入模块，这时你可以使用动态导入代替静态导入。下面的是你可能会需要动态导入的场景：

- 当静态导入的模块很明显的降低了代码的加载速度且被使用的可能性很低，或者并不需要马上使用它。
- 当静态导入的模块很明显的占用了大量系统内存且被使用的可能性很低。
- 当被导入的模块，在加载时并不存在，需要异步获取。
- 当导入模块的说明符，需要动态构建。（静态导入只能使用静态说明符）。
- 当被导入的模块有副作用（这里说的副作用，可以理解为模块中会直接运行的代码），这些副作用只有在触发了某些条件才被需要时。（原则上来说，模块不能有副作用，但是很多时候，你无法控制你所依赖的模块的内容）。

**语法**

``import()``

```js
import('/modules/my-module.js')  
  .then((module) => {    
  *// Do something with the module.*  
	});
// 支持await
const module = await import('/modules/my-module.js');

```

注意事项：

请不要滥用动态导入（只有在必要情况下采用）。 静态框架能更好地初始化依赖，而且更有利于静态分析工具和 `tree shaking` 发挥作用。

与静态 `import` 差异：

- `import()` 可以从脚本中使用，而不仅仅是从模块中使用。
- 如果 `import()` 在模块中使用，它可以出现在任何级别的任何地方，并且不会被提升。
- `import()` 接受任意字符串（此处显示了运行时确定的模板字符串），而不仅仅是静态字符串文字。
- 模块中存在 `import()` 并不会建立依赖关系，在对包含的模块求值之前，必须先获取并求值依赖关系。
- `import()` 不建立可以静态分析的依赖关系（但是，在诸如 `import('./foo.js')` 这样的简单情况下，实现可能仍然能够执行推测性抓取）。

### 2 Promise.allSettled

`Promise.allSettled()` 方法返回一个在所有给定的 `promise` 都已经 `fulfilled` 或 `rejected` 后的 `promise`，并带有一个对象数组，每个对象表示对应的 `promise` 结果。

当您有多个彼此不依赖的异步任务成功完成时，或者您总是想知道每个 `promise` 的结果时，通常使用它。

相比之下，`Promise.all()` 更适合彼此相互依赖或者在其中任何一个 `reject` 时立即结束。

> 该提案目前处于 Stage 4 阶段。

| api                | 描述                          |                    |
| ------------------ | ----------------------------- | ------------------ |
| Promise.allSettled | 不会短路                      | 本提案 🆕           |
| Promise.all        | 当存在输入值 rejected 时短路  | 在 ES2015 中添加 ✅ |
| Promise.race       | 当存在输入值 稳定 时短路      | 在 ES2015 中添加 ✅ |
| Promise.any        | 当存在输入值 fulfilled 时短路 | 在 ES2021 中添加 ✅ |

### 3 Optional Chaining（常用）

可选链操作符（`?.`）允许读取位于连接对象链深处的属性的值，而不必明确验证链中的每个引用是否有效。`?.` 操作符的功能类似于 `.` 链式操作符，不同之处在于，在引用（`null` 或者 `undefined`）的情况下不会引起错误，该表达式短路返回值是 `undefined`。与函数调用一起使用时，如果给定的函数不存在，则返回 `undefined`。

> 该提案目前处于 Stage 4 阶段。

**语法

```
obj?.prop
obj?.[expr]
arr?.[index]
func?.(args)
```

**例子**

```js
let user = {};
let u1 = user.childer.name;
// TypeError: Cannot read property 'name' of undefined
let u1 = user?.childer?.name;
// undefined
```

## ES12

### 1 String.prototype.replaceAll

`replaceAll()` 方法返回一个新字符串，新字符串所有满足 `pattern` 的部分都已被 `replacement` 替换。`pattern` 可以是一个字符串或一个 `RegExp`， `replacement` 可以是一个字符串或一个在每次匹配被调用的函数。

> 该提案目前处于 Stage 4 阶段。

**语法**

``String.prototype.replaceAll(searchValue, replaceValue);``

**用法**

```js
const queryString = 'q=query+string+parameters'; 
const withSpaces = queryString.replaceAll('+', ' ');
```

### 2 Promise.any

`Promise.any()` 接收一个 `Promise` 可迭代对象，只要其中的一个 `promise` 成功，就返回那个已经成功的 `promise`。如果可迭代对象中没有一个 `promise` 成功（即所有的 `promises` 都失败/拒绝），就返回一个失败的 `promise`。

> 该提案目前处于 Stage 4 阶段。

**例子**

```
const promise1 = new Promise((resolve, reject) => reject('失败的Promise1'));
const promise2 = new Promise((resolve, reject) => reject('失败的Promise2'));
const promiseList = [promise1, promise2];
Promise.any(promiseList)
.then(values=>{
  console.log(values);
})
.catch(error=>{
  console.log(error);
});
```

### 3 Numeric Separators

数字分隔符

它是将其早期草案与 `Christophe Porteneuve` 的提案 `numeric underscores` 合并的结果，以扩展现有的 `NumericLiteral` 以允许数字之间的分隔符。

> 该提案目前处于 Stage 4 阶段。

**用法**

使用下划线 `_`（U+005F）在数字间进行分隔。

**语法**

```js
const price1 = 1_000_000_000;
const price2 = 1000000000;
price1 === price2; // true

const float1 = 0.000_001;
const float2 = 0.000001;
float1 === float2; // true;
```

