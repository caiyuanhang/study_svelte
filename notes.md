# Svelte 学习

## 一、介绍

### 1、交互式教程知识点

[API 文档]("https://svelte.dev/docs/introduction")、[示例]("https://svelte.dev/examples/hello-world")

如需快速创建项目，可通过此命令：`npm init svelte`

**Svelte 是什么？**

- Svelte 是一个构建 web 应用程序的工具。和其他框架一样，它允许你用组合了标签、样式和行为的组件来声明式地构建应用；
- 这些组件被编译成小而高效的 JavaScript 模块，从而消除了传统上与 UI 框架相关的开销（比如：diff 对比）；
- 可以使用 Svelte 构建整个应用程序(例如，使用 [SvelteKit]("https://kit.svelte.dev") 等应用程序框架，下面将介绍)，或者可以将其增量添加到现有代码库中。还可以将组件作为可以在任何地方使用的独立包发布。

**本次交互式教程知识点总共分为 4 节**
每一节将提供一个练习，旨在说明一个特性。后面的练习建立在前面的练习中获得的知识之上，所以可以从头到尾进行练习。

#### 1.1 Svelte 基础

##### 1.1.1 你的第一个组件

在 Svelte 中，应用程序由一个或多个组件组成。组件是一个可重用的自包含代码块（.svelte 文件），它包含了 HTML、CSS 和 JavaScript。下面的 `App.svelte` 文件是一个简单的组件。

App.svelte 包含了 HTML、JS，在 html 的“{}”中，可以放入任何我们想要的 js。

```svelte
<script>
 let name = 'Darren';
</script>

<h1>Hello {name.toUpperCase()}!</h1>
```

###### **总结知识点：**

- 一个`.svelte`组件，由 html 标签、script 标签、style 标签组成；
- 标签内引用 js 变量，需要通过“{}”来引用。

##### 1.1.2 动态属性

和使用花括号来控制文本一样，也可以使用它们来控制元素的动态属性。如：图像缺少一个 src，那就可以添加一个动态属性“{src}”。

**注意：**img 标签需要有 alt 属性（为了确保它们能够被最广泛的用户群访问，包括视力或运动受损的人，或者没有强大硬件或良好互联网连接的人。），如果没有写，svelte 将通过警告提供帮助。

在构建 web 应用程序时，重要的是要确保它们能够被最广泛的用户群访问，包括视力或运动受损的人，或者没有强大硬件或良好互联网连接的人。可访问性(简写为 a11y)并不总是容易得到正确的处理，但是如果您编写了不可访问的标签，Svelte 将通过警告提供帮助。

```svelte
<script>
 let src = '/tutorial/image.gif';
 const name = 'Darren';
</script>

<!-- {src} is short for src={src} -->
<img {src} alt="{name} dancing" />
```

###### 1.1.2.1 总结知识点

- 动态属性也是通过“{}”的方式使用，一种是非字符串形式，如：`src={src}`；一种是字符串形式，如：`alt="{name} dancing"`；
- 动态属性的 key 如果和 value 名字一样，则可以将`src={src}`缩略为`{src}`。

##### 1.1.3 样式

在`.svelte`组件中，可以通过`<style></style>`标签添加样式，在一个`.svelte`组件中添加的 style 标签的样式只会作用于当前的这个`.svelte`组件。

```svelte
<p>Styled!</p>

<style>
 p {
  color: red;
  font-size: 32px;
 }
</style>
```

###### 1.1.3.1 总结知识点

可以通过`<style></style>`标签给`.svelte`组件添加样式，并且该样式只会作用于这个`.svelte`组件

##### 1.1.4 嵌套组件

可以通过`import`来导入其他组件使用。

Nested.svelte

```svelte
<p>...don't affect this element</p>
```

App.svelte

```svelte
<script>
 import Nested from './Nested.svelte';
</script>

<p>These styles...</p>
<Nested />

<style>
 p {
  color: purple;
  font-family: 'Comic Sans MS', cursive;
  font-size: 2em;
 }
</style>
```

###### 1.1.4.1 总结知识点

其他组件可以通过 `import` 导入使用，**需要注意的是：**组件名需要大写，且导入组件内和当前组件相同的标签，不受当前样式的影响，例如上面案例中：`Nested.svelte`组件内的 p 标签样式不受`App.svelte`组件样式的影响。

##### 1.1.5 HTML 标识符

标识符可以将字符串中的 `<` 和 `>` 识别成标签。

```svelte
<script>
 let string = `this string contains some <strong>HTML!!!</strong>`;
</script>

<p>{@html string}</p>
```

###### 1.1.5.1 总结知识点

@html 标识符，可以将字符串中的`<` 和 `>`识别成标签。

##### 1.1.6 响应性-声明赋值

Svelte 的核心是一个强大的响应系统，用于保持 DOM 与应用程序状态(例如：响应事件)的同步。

```svelte
<script>
 let count = 0;

 function increment() {
  count += 1;
 }
</script>

<button on:click={increment}>
 Clicked {count}
 {count === 1 ? 'time' : 'times'}
</button>
```

###### 1.1.6.1 总结知识点

- 通过声明变量定义一个响应式状态，每当赋值操作发生改变状态时，状态的更新都会出发视图的更新；
- 定义事件的方式是，on:事件名。

##### 1.1.7 响应性-语句

当组件的状态改变时，Svelte 会自动更新 DOM。

通常，某些组件状态或语句需要以其他组件状态作为基础计算得出(例如：从名字和姓氏派生的全名)，并在它们更改时重新计算执行，这时就需要我们用到响应式语句，其以`$:`开头，后面紧跟`变量`和`表达式`。

###### 1.1.7.1 值

**重点：**响应式语句会对未声明的变量自动注入 let 声明。示例如下：

```svelte
<script>
 let count = 0;

 $: doubled = count * 2 // 这里的doubled未声明就直接使用了，正常是会报错，但是svelte会自动为我们注入let doubled的声明，所以不会报错。
</script>
```

**注意：**响应式声明、语句在其他脚本代码之后，组件标签渲染之前执行。

```svelte
<script>
 let count = 0;

 function increment() {
  count += 1;
 }

 $: doubled = count * 2;
</script>

<button on:click={increment}>
 Clicked {count}
 {count === 1 ? 'time' : 'times'}
</button>
<p>{doubled} is {count} double</p>
```

###### 1.1.7.2 任意语句

- 语句

响应式语句除了运用在值的动态计算上，还能用于任何语句。示例如下：

```svelte
<script>
let count = 0;

$: console.log(`数量是：${count}`) // 当count发生变化时，会重新执行这条语句
</script>
```

- 代码块

如果你需要的响应式语句是一个代码块，可以用`{}`把响应式语句的代码包裹起来，示例如下：

```svelte
<script>
let count = 0;

// 当count发生变化时，会重新执行这条代码块的内容
$: {
    console.log(`数量是：${count}`);
    console.log("代码块");
}
</script>
```

- if 语句

`$:`后面也可以放置 if、while、for 等语句

```svelte
<script>
let count = 0;

// 当count发生变化时，会重新执行这条if语句
$: if (count > 5) {
    alert("$: 后面放置if语句")
}
</script>
```

###### 1.1.7.3 总结知识点

- 响应式语句的语法是（`$:`后面跟着赋值表达式或任意语句，如：if、while、for 等）：`$: doubled = count * 2`、`$: console.log(普通语句)`、`$: { console.log("代码块"); }`、`$: if (count > 5) { console.log("if 语句"); }`；
- 每当响应式语句依赖的状态发生变化时，响应式语句都会重新执行一遍；
- 响应式声明、语句在其他脚本代码之后，组件标签渲染之前执行。

##### 1.1.8 响应性-更新数组和对象

###### 1.1.8.1 数组

svelte 的响应式状态是由赋值触发的，所以使用像 push 和 splice 这样的数组方法不会触发视图更新。

例如下面代码，即使 numbers 打印出来是 push 数字成功，numbers 发生了变化，但是页面显示的还是`[1, 2, 3, 4]`：

```svelte
<script>
 let numbers = [1, 2, 3, 4];

 function addNumber() {
  numbers.push(numbers.length + 1);
  alert(numbers)
 }
</script>

<p>{numbers}</p>
<button on:click={addNumber}>
 添加数字
</button>
```

解决这个问题的一个方法是在`numbers.push(numbers.length + 1);`之后添加一个赋值操作`numbers = numbers`，就会触发视图更新。

```svelte
<script>
 let numbers = [1, 2, 3, 4];

 function addNumber() {
  numbers.push(numbers.length + 1);
  numbers = numbers
 }
</script>

<p>{numbers}</p>
<button on:click={addNumber}>
 添加数字
</button>
```

对此代码改造的另一个友好方式是：`numbers = [...numbers, numbers.length + 1]`

```svelte
<script>
 let numbers = [1, 2, 3, 4];

 function addNumber() {
  numbers = [...numbers, numbers.length + 1]
 }
</script>

<p>{numbers}</p>
<button on:click={addNumber}>
 添加数字
</button>
```

所以，对于数组的操作更新，可以使用此类方式来代替`pop`、`shift`、`unshift`、`splice`操作。

然后，对于数组和对象中属性的赋值操作，其本质和赋值本身的操作方式相同。比如：

```svelte
<script>
 let numbers = [1, 2, 3, 4];

//  function addNumber() {
//   numbers = [...numbers, numbers.length + 1]
//  }
// 也可以改为
 function addNumber() {
  numbers[numbers.length] = numbers.length + 1
 }
</script>

<p>{numbers}</p>
<button on:click={addNumber}>
 添加数字
</button>
```

###### 1.1.8.2 对象

关于对象的响应式更新，在完成对象的属性赋值之后，就可以更新视图。

但是有一种情况不会更新视图，当拿对象的某个属性（该属性仍为对象）赋值给一个新变量后（这个变量其实是拿到对象该属性的引用），改变这个新变量中其中一个属性的值，页面引用原始对象的视图部分并不会发生响应式更新。具体代码如下：

```svelte
<script>
 let obj = { foo: { bar: 1 } };
 const foo = obj.foo;

 function changeObj() {
  foo.bar +=1;
  // obj.foo.bar +=1;
  alert(`${foo.bar}, ${obj.foo.bar}` )
 }
</script>

<!-- 因为是foo.bar改变，虽然foo.bar是obj.foo.bar的引用，但视图部分是foo.bar变化，obj.foo.bar不会变化，否则反之。 -->
<p>{obj.foo.bar}</p> 
<p>{foo.bar}</p>

<button on:click={addNumber}>
 Add a number
</button>
```

###### 1.1.8.3 总结知识点

- 数组的响应式更新，包括数组本身更新和内部元素更新，都需要通过赋值来触发响应式视图更新（推荐用法是`...扩展`语法，而尽量不要用`pop`、`shift`、`unshift`、`push`）；
- 对象的响应式更新也需要通过直接赋值（也就是避免上面对象部分示例代码的情况）触发视图更新。

##### 1.1.9 props

###### 1.1.9.1 声明props

当数据要从一个组件传递给子组件时，我们就需要对传递的属性进行声明，一般是通过`export`关键字实现，示例如下：

App.svelte

```svelte
<script>
 import Nested from './Nested.svelte';
</script>

<Nested answer={42} />
```

Nested.svelte

```svelte
<script>
 export let answer;
</script>

<p>The answer is {answer}</p>
```

###### 1.1.9.2 定义props默认值

当传递的属性没有给定值时，我们又需要一个默认值时，可以通过给到处的属性赋值一个值作为默认值，示例如下：

App.svelte

```svelte
<script>
 import Nested from './Nested.svelte';
</script>

<Nested answer={42} />
<Nested />
```

Nested.svelte

```svelte
<script>
 export let answer = "this is default";
</script>

<p>The answer is {answer}</p>
```

###### 1.1.9.3 传递props

传递props时，可以通过单个属性逐个传递，也可以通过`...扩展语法`传递

App.svelte

```svelte
<script>
 import PackageInfo from './PackageInfo.svelte';

 const pkg = {
  name: 'svelte',
  speed: 'blazing',
  version: 4,
  website: 'https://svelte.dev'
 };
</script>

<PackageInfo
 name={pkg.name}
 speed={pkg.speed}
 version={pkg.version}
 website={pkg.website}
/>
<!-- 或者通过...扩展语法传递属性 -->
<!-- <PackageInfo
 {...pkg}
/> -->
```

PackageInfo.svelte

```svelte
<script>
 export let name;
 export let version;
 export let speed;
 export let website;

 $: href = `https://www.npmjs.com/package/${name}`;
</script>

<p>
 The <code>{name}</code> package is {speed} fast. Download version {version} from
 <a {href}>npm</a> and <a href={website}>learn more here</a>
</p>
```

对于一些`export`没有声明的属性，如果也想在子组件中拿到这部分内容，可以通过`$$props`来获取传递过来的所有props，通常不推荐这样做，因为Svelte很难优化，但在极少数情况下它是有用的。

App.svelte

```svelte
<script>
 import PackageInfo from './PackageInfo.svelte';

 const pkg = {
  name: 'svelte',
  speed: 'blazing',
  version: 4,
  website: 'https://svelte.dev',
  fix: 123,
  hide: false
 };
</script>

<PackageInfo {...pkg} />
```

PackageInfo.svelte

```svelte
<script>
 export let name;
 export let version;
 export let speed;
 export let website;

 console.log("$$props==", $$props) // $$props是一个props对象，包含了所有传递过来的属性。
</script>
```

###### 1.1.9.4 总结知识点

- 通过`export let title;`来声明一个组件的props属性；
- 当需要定义一个props的默认值时，可以通过`export let title = "this is title";`来定义；
- 正常需要传递多少个props，就声明多少个props属性，当你需要引用所有传入组件的props，包括那些没有通过export声明的，你可以通过直接访问`$$props`来实现。但通常不推荐这样做，因为Svelte很难优化，但在极少数情况下它是有用的。

##### 1.1.10 逻辑

HTML 没有表达逻辑的方式，比如：条件和循环等。当我们需要有条件的渲染一些标签时，就需要用到逻辑块。

###### 1.1.10.1 if 逻辑块

在标签中可以使用 `if` 逻辑块来决定某个标签的渲染逻辑。

```svelte
<script>
 let count = 0;

 function increment() {
  count += 1;
 }
</script>

<button on:click={increment}>
 Clicked {count}
 {count === 1 ? 'time' : 'times'}
</button>

{#if count >= 5}
 <p>此时{count}大于等于5</p>
{/if}
```

###### 1.1.10.2 else 逻辑块

和 js 一样，svelte 的 `if` 逻辑块后面也有 `else` 逻辑块。

```svelte
<script>
 let count = 0;

 function increment() {
  count += 1;
 }
</script>

<button on:click={increment}>
 Clicked {count}
 {count === 1 ? 'time' : 'times'}
</button>

{#if count >= 5}
 <p>此时{count}大于等于5</p>
{:else}
 <p>{count}小于5</p>
{/if}
```

**重点：** `#`字符总是表示块的开始，`/`表示块的结束，`:`表示块的延续。

###### 1.1.10.3 else if 逻辑块

当有多个逻辑的时候，可以使用 `else if` 逻辑块表示。

```svelte
<script>
 let count = 0;

 function increment() {
  count += 1;
 }
</script>

<button on:click={increment}>
 Clicked {count}
 {count <= 1 ? 'time' : 'times'}
</button>

{#if count > 8}
 <p>count大于8</p>
{:else if count <4}
 <p>此时count小于4</p>
{:else}
 <p>count在4和8之间</p>
{/if}
```

###### 1.1.10.4 each 逻辑块

当我们遇到类似列表的重复标签时，我们可以通过 `each` 逻辑块来遍历生成列表标签。示例如下：

```svelte
<script>
 const colors = ['red', 'orange', 'yellow', 'green', 'blue', 'indigo', 'violet'];
 let selected = colors[0];
</script>

<h1 style="color: {selected}">Pick a colour</h1>

<div>
 {#each colors as color, i}
 <button
  aria-current={selected === color}
  aria-label={color}
  style="background: {color}"
  on:click={() => selected = color}
 >{i + 1}</button>
 {/each}
</div>

<style>
 h1 {
  transition: color 0.2s;
 }

 div {
  display: grid;
  grid-template-columns: repeat(7, 1fr);
  grid-gap: 5px;
  max-width: 400px;
 }

 button {
  aspect-ratio: 1;
  border-radius: 50%;
  background: var(--color, #fff);
  transform: translate(-2px,-2px);
  filter: drop-shadow(2px 2px 3px rgba(0,0,0,0.2));
  transition: all 0.1s;
 }

 button[aria-current="true"] {
  transform: none;
  filter: none;
  box-shadow: inset 3px 3px 4px rgba(0,0,0,0.2);
 }
</style>
```

只要是可迭代数据都可以用 `each` 逻辑块来遍历。

**注意：**each逻辑块一定要添加key，这样能确保准确地更新到对应的DOM。添加key的方式是在遍历的变量后面添加 `(key)`。

示例如下：

App.svelte

```svelte
<script>
 import Thing from './Thing.svelte';

 let things = [
  { id: 1, name: 'apple' },
  { id: 2, name: 'banana' },
  { id: 3, name: 'carrot' },
  { id: 4, name: 'doughnut' },
  { id: 5, name: 'egg' }
 ];

 function handleClick() {
  things = things.slice(1);
 }
</script>

<button on:click={handleClick}>
 Remove first thing
</button>

<!-- 添加key的方式如下 -->
{#each things as thing (thing.id)}
 <Thing name={thing.name} />
{/each}
```

Thing.svelte

```svelte
<script>
 const emojis = {
  apple: '🍎',
  banana: '🍌',
  carrot: '🥕',
  doughnut: '🍩',
  egg: '🥚'
 };

 export let name; // 每当props值更新时，name都会更新。

 const emoji = emojis[name];  // 但是 emoji 变量在组件初始化时是固定的，因为它使用的是 const 而不是 $:，所以当后面组件的 name 变化时，emoji 还是之前的，然后因为 things 数据只有4条，就只渲染前面4个 Thing 组件。
</script>

<p>{emoji} = {name}</p>
```

###### 1.1.10.5 await/then/catch 逻辑块

当我们需要在标签中处理异步数据时，我们就需要用到 `await/then/catch` 逻辑块，`await/then/catch` 逻辑块会帮我们处理最新的 `Promise`，对于之前触发的相同 `Promise`，Svelte会帮我们作取消处理，从而让我们不用担心 `竞态条件`。

`竞态条件`：是指两个或多个操作几乎同时发生，并且它们的执行顺序对最终结果有重要影响的情况，在Svelte的await block中，由于只关注最新的promise，因此你不需要担心竞态条件。这意味着无论用户如何快速地触发异步操作，你都可以确保组件最终显示的是最新操作的结果，而不是之前某个未完成操作的结果。

示例代码如下：

App.svelte

```svelte
<script>
 import { getRandomNumber } from './utils.js';

 let promise = getRandomNumber();

 function handleClick() {
  promise = getRandomNumber();
 }
</script>

<button on:click={handleClick}>
 generate random number
</button>

{#await promise}
<p>...waiting</p>
{:then number}
<p>this is {number}</p>
{:catch error}
<p style="color: red">报错啦</p>
{/await}

<!-- 当确确保Promise不会走catch，可以省略 catch 逻辑块，如果在 then 成功之前不需要显示什么内容，则这部分也可以省略 -->
{#await promise then number }
 <p>The number is {number}</p>
{/await}
```

utils.js

```js
export async function getRandomNumber() {
    const res = await fetch('/random-number');

    if (res.ok) {
        return await res.text();
    } else {
        throw new Error('Request failed');
    }
}
```

###### 1.1.10.6 总结知识点

- 标签中的逻辑渲染可以通过 `if`、`else`、`else if`、`each`、`await`、`then`、`catch` 等逻辑块来处理。
- `#` 表示逻辑块的开始、`:` 表示逻辑块的延续、`/` 表示逻辑块的结束；
- `if` 逻辑块具体语法为：`{#if 条件}内容{/if}`；
- `else` 逻辑块具体语法为：`{#if 条件} 内容1 {:else} 内容2 {/if}`；
- `else if` 逻辑块具体语法为：`{#if 条件1} 内容1 {:else if 条件2} 内容2 {/if}`；
- `each` 逻辑块具体语法为：`{#each datas as item (item.id), index} 内容 {/each}`；
- `await/then/catch` 逻辑块具体语法为：1）一般写法：`{#await Promise} 等待的内容1 {:then response} 结果 {:catch error} 错误的内容 {/await}`；2）缩略写法（省略等待、错误显示的内容）：`{#await Promise then response } 结果 {/await}`。

##### 1.1.11 事件

###### 1.1.11.1 绑定 DOM 事件

当我们需要给DOM添加一个事件时，可以通过 `on:事件名` 来添加，示例如下：

```svelte
<script>
 let m = { x: 0, y: 0 };

 function handleMove(event) {
  m.x = event.clientX;
  m.y = event.clientY;
 }
</script>

<div on:pointermove={handleMove}>
 The pointer is at {m.x} x {m.y}
</div>

<style>
 div {
  position: fixed;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  padding: 1rem;
 }
</style>
```

除了声明引用事件，也可以声明行内事件，示例如下：

```svelte
<script>
 let m = { x: 0, y: 0 };
</script>

<div on:pointermove={(e) => { m = { x: e.clientX, y: e.clientY } }}>
 The pointer is at {m.x} x {m.y}
</div>

<style>
 div {
  position: fixed;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  padding: 1rem;
 }
</style>
```

**备注：**在某些框架中，您可能会看到出于性能原因而避免内联事件的建议，特别是在循环中。这个建议并不适用于 Svelte，无论选择哪种形式，编译器总是会做正确的事情。

###### 1.1.11.2 事件修饰符

如果要对事件进行修饰，可以通过事件修饰符来操作，事件修饰符的语法为：`on:事件名|修饰符1|修饰符2...`，多个事件修饰符之间可以通过 `|` 相连接。

所有的`事件修饰符`如下：

- `preventDefault`，在运行事件处理程序之前调用event.preventDefault()。例如，用于客户端表单处理；
- `stopPropagation`，调用event.stoppropagation()，防止事件到达下一个元素；
- `passive`，改善 touch / scrolling 事件的滚动性能（Svelte会在安全的情况下自动添加）；
- `nonpassive`，显示设置 `passive` 为false；
- `capture`，在捕获阶段而不是冒泡阶段触发处理程序；
- `once`，在事件处理程序第一次运行后删除它；

```svelte
<button on:click|once={() => alert('clicked')}>
 Click me
</button>
```

- `self`，仅在目标元素自身的事件受到触发时处理程序；
- `trusted`，只有当 event.isTrusted 为 true 时才触发处理程序，这意味着事件是由用户操作触发的，而不是因为某些 js 调用 element.dispatchEvent 而触发，示例如下：

```svelte
<script>  
  function handleClick(event) {  
    alert('123')
  }  
  
  function triggerClickProgrammatically() {  
    const button = document.querySelector('#myButton');  
    const event = new MouseEvent('click', {  
      bubbles: true,  
      cancelable: true,  
      view: window  
    });  
  
    // 这个事件不会被handleClick捕获，因为event.isTrusted为false  
    button.dispatchEvent(event);  
  }  
</script>  
  
<button id="myButton" on:click|trusted={handleClick}>  
  Click me!  
</button>  
  
<button on:click={triggerClickProgrammatically}>  
  Trigger Click Programmatically  
</button>  
```

###### 1.1.11.3 创建和派发组件自定义事件

组件可以派发事件，但是需要创建一个事件派发器，这个可以通过 `svelte` 里面的 `createEventDispatcher` 方法来创建，`createEventDispatcher` 方法调用后会返回一个 `dispatch` 方法，该方法接收两个参数：

- 参数1，是`事件名`（任意定义，但要确认 `dispatch` 和 `on` 的事件名是一致的，才能触发对应的时间执行函数）；
- 参数2，是传递的参数。

在组件内部调用 `dispatch` 方法之后，记得要在组件外部（父组件）使用 `on:事件名` 来监听 `dispatch` 的事件，`dispatch`函数的第二个参数传递的内容，可以在监听时间的事件对象 `event` 里面的 `detail`属性中看到。

具体示例代码如下：

App.svelte

```svelte
<script>
 import Inner from './Inner.svelte';

 function handleMessage(event) {
  alert(event.detail.text);  // 这里的 event.detail 可以拿到 dispatch 第二个参数传递的所有内容。 
 }
</script>

<Inner on:message={handleMessage} />

<div on:message={handleMessage}>123</div>
```

Inner.svelte

```svelte
<script>
 import { createEventDispatcher } from "svelte"

 const dispatch = createEventDispatcher()
 
 function sayHello() {
  dispatch('message', {
   text: 'Hello!'
  });
 }
</script>

<button on:click={sayHello}>
 Click to say hello
</button>
```

**注意：**`createEventDispatcher` 必须在组件第一次实例化时调用（也就是在 script 标签的上下文中调用），不能稍后在 setTimeout 回调中调用（因为这些后续逻辑可能发生在组件的生命周期之外，或者与组件的当前实例状态不同步）。简单说，就是 `createEventDispatcher` 应该在组件实例化时立即调用，并且它的调用应该与组件的生命周期同步。这是为了确保自定义事件的派发与组件的当前状态和行为保持一致，从而保持应用的逻辑清晰和可预测。在实际开发中，应该在组件的 `<script>` 部分顶部调用 `createEventDispatcher`，并立即将返回的 `dispatch` 函数保存到组件的作用域中，以便在后续的逻辑中使用它来派发事件。这样做可以避免潜在的错误和混淆，使你的 Svelte 应用更加健壮和易于维护。

###### 1.1.11.4 事件转发

与 DOM event 不同，component event 不会冒泡。如果想要监听某个深度嵌套组件上的事件，中间组件必须转发该事件。

在中间组件转发事件上，svelte 提供了一种简便的缩略写法：中间转发时间的 component 只需要提供 `on:事件名` 就可以转发事件处理函数，而不需要将函数也传递到下一层，这可以让我们减写很多代码，示例如下：

App.svelte

```svelte
<script>
 import Outer from './Outer.svelte';

 function handleMessage(event) {
  alert(event.detail.text);
 }
</script>

<Outer on:message={handleMessage} />
```

Outer.svelte

```svelte
<script>
 import Inner from './Inner.svelte';
</script>

<!-- 通过 on:message 直接转发 component 事件，而不用连函数也传递 -->
<Inner on:message />
```

Inner.svelte

```svelte
<script>
 import { createEventDispatcher } from 'svelte';

 const dispatch = createEventDispatcher();

 function sayHello() {
  dispatch('message', {
   text: 'Hello!'
  });
 }
</script>

<button on:click={sayHello}>
 Click to say hello
</button>
```

事件转发也适用于 DOM 事件，DOM 事件的事件转发跟 component 事件的事件转发。

App.svelte

```svelte
<script>
 import BigRedButton from './BigRedButton.svelte';
 import horn from './horn.mp3'; // 任意 mp3 文件

 const audio = new Audio();
 audio.src = horn;

 function handleClick() {
  audio.load();
  audio.play();
 }
</script>

<BigRedButton on:click={handleClick} />
```

BigRedButton.svelte

```svelte
<button on:click>
 Push
</button>

<style>
 button {
  font-size: 1.4em;
  width: 6em;
  height: 6em;
  border-radius: 50%;
  background: radial-gradient(circle at 25% 25%, hsl(0, 100%, 50%) 0, hsl(0, 100%, 40%) 100%);
  box-shadow: 0 8px 0 hsl(0, 100%, 30%), 2px 12px 10px rgba(0,0,0,.35);
  color: hsl(0, 100%, 30%);
  text-shadow: -1px -1px 2px rgba(0,0,0,0.3), 1px 1px 2px rgba(255,255,255,0.4);
  text-transform: uppercase;
  letter-spacing: 0.05em;
  transform: translate(0, -8px);
  transition: all 0.2s;
 }

 button:active {
  transform: translate(0, -2px);
  box-shadow: 0 2px 0 hsl(0, 100%, 30%), 2px 6px 10px rgba(0,0,0,.35);
 }
</style>
```

###### 1.1.11.5 总结知识点

- 通过 `on:事件名` 来绑定 DOM 事件；
- 通过 `on:事件名|事件修饰符` 来对事件进行修饰，同个事件可以装配多个事件修饰符，事件修饰符有：`preventDefault`、`stopPropagation`、`passive`、`nonpassive`、`capture`、`once`、`self`、`trusted`；
- 创建自定义事件可以通过 `svelte` 中的 `createEventDispatcher`，需要注意一点，该方法应该放在 `script` 标签的顶部调用，并将返回的 `dispatch` 方法保存在当前 script 标签的作用域中，以给后面使用。`dispatch` 方法接收两个参数，参数1是自定义事件的名称，参数2是传递的参数，要在组件外部订阅自定义事件，需要通过 `on:自定义事件名` 来进行；
- 深度嵌套的 component 事件转发在流经的中间组件中，可以通过 `on:自定义事件名` 简写形式转发。

##### 1.1.12 双向绑定

**备注：**Svelte 中的数据流是自顶向下

和 Vue 一样，svelte 也有自己的双向绑定，通过在 `value` 前天添加 `bind:` 即可将表单元素的值和状态双向绑定。

当 `bind:value={value}` 时，可以简写为 `bind:value`。

###### 1.1.12.1 Text inputs

```svelte
<script>
 let name = 'world';
</script>

<input bind:value={name} />

<h1>Hello {name}!</h1>
```

###### 1.1.12.2 Numeric inputs

在DOM中，所有输入值默认都是字符串，通过在 Svelte 中使用 `bind:value`，可以让 Svelte 自动处理这些值的类型转换，以让我们在使用时直接拿到数字类型的值，而不需要做字符串到数字的转换处理。

```svelte
<script>
 let a = 1;
 let b = 2;
</script>

<label>
 <input type="number" bind:value={a} min="0" max="10" />
 <input type="range" bind:value={a} min="0" max="10" />
</label>

<label>
 <input type="number" bind:value={b} min="0" max="10" />
 <input type="range" bind:value={b} min="0" max="10" />
</label>

<p>{a} + {b} = {a + b}</p>
```

###### 1.1.12.3 Checkbox inputs

单个 checkbox 的双向绑定不是绑定到 value，而是绑定到 checked。

```svelte
<script>
 let yes = false;
</script>

<label>
 <input type="checkbox" bind:checked={yes} />
 Yes! Send me regular email spam
</label>

{#if yes}
 <p>
  Thank you. We will bombard your inbox and sell
  your personal details.
 </p>
{:else}
 <p>
  You must opt in to continue. If you're not
  paying, you're the product.
 </p>
{/if}

<button disabled={!yes}>Subscribe</button>
```

###### 1.1.12.4 Select bindings

当 svelte 的 `bind.value` 为 `undefined` 时，svelte 默认会选中第一个 option，这一默认行为只适用于在组件初始化时，`bind.value` 的值为 `undefined` 情况。

```svelte
<script>
 let questions = [
  {
   id: 1,
   text: `Where did you go to school?`
  },
  {
   id: 2,
   text: `What is your mother's name?`
  },
  {
   id: 3,
   text: `What is another personal fact that an attacker could easily find with Google?`
  }
 ];

 let selected;

 let answer = '';

 function handleSubmit(e) {
  console.log("e==", e)
  alert(
   `answered question ${selected.id} (${selected.text}) with "${answer}"`
  );
 }
</script>

<h2>Insecurity questions</h2>

<form on:submit|preventDefault={handleSubmit}>
 <select
  bind:value={selected}
  on:change={() => (answer = '')}
 >
  {#each questions as question}
   <option value={question}>
    {question.text}
   </option>
  {/each}
 </select>

 <input bind:value={answer} />

 <button disabled={!answer} type="submit">
  Submit
 </button>
</form>

<p>
 selected question {selected
  ? selected.id
  : '[waiting...]'}
</p>
```

###### 1.1.12.5 Group inputs

对于多个 `radio` 或者 `checkbox`，可以将 `bind:group` 与 `value` 属性一起使用。
对于 `radio`，`bind:group` 的值是 `radio` 中单一的 `value`。
对于 `checkbox`，`bind:group` 的值是 `checkbox` 中已选定的 `value`。

```svelte
<script>
 let scoops = 2;
 let flavours = [];

 const formatter = new Intl.ListFormat('en', { style: 'long', type: 'conjunction' });
</script>

<h2>Size</h2>

{#each [1, 2, 3] as number}
 <label>
  <input
   type="radio"
   name="scoops"
   value={number}
   bind:group={scoops}
  />

  {number} {number === 1 ? 'scoop' : 'scoops'}
 </label>
{/each}

<h2>Flavours</h2>

{#each ['cookies and cream', 'mint choc chip', 'raspberry ripple'] as flavour}
 <label>
  <input
   type="checkbox"
   name="flavours"
   value={flavour}
   bind:group={flavours}
  />

  {flavour}
 </label>
{/each}

{#if flavours.length === 0}
 <p>Please select at least one flavour</p>
{:else if flavours.length > scoops}
 <p>Can't order more flavours than scoops!</p>
{:else}
 <p>
  You ordered {scoops} {scoops === 1 ? 'scoop' : 'scoops'}
  of {formatter.format(flavours)}
 </p>
{/if}
```

###### 1.1.12.6 Select multiple

当 select 添加了 `multiple` 属性后，`bind:value` 获取到的值是一个数组。

**注意：**当 `option` 上的 value 和内容一样时，可以省略 `option` 里面的 value 属性。

```svelte
<script>
 let scoops = 1;
 let flavours = [];

 const formatter = new Intl.ListFormat('en', { style: 'long', type: 'conjunction' });
 $: console.log("flavours", flavours) // ['cookies and cream', 'mint choc chip', 'raspberry ripple']
</script>

<h2>Size</h2>

{#each [1, 2, 3] as number}
 <label>
  <input
   type="radio"
   name="scoops"
   value={number}
   bind:group={scoops}
  />

  {number} {number === 1 ? 'scoop' : 'scoops'}
 </label>
{/each}

<h2>Flavours</h2>

<select multiple bind:value={flavours}>
 {#each ['cookies and cream', 'mint choc chip', 'raspberry ripple'] as flavour}
  <option>{flavour}</option>
 {/each}
</select>

{#if flavours.length === 0}
 <p>Please select at least one flavour</p>
{:else if flavours.length > scoops}
 <p>Can't order more flavours than scoops!</p>
{:else}
 <p>
  You ordered {scoops} {scoops === 1 ? 'scoop' : 'scoops'}
  of {formatter.format(flavours)}
 </p>
{/if}
```

###### 1.1.12.7 Textarea inputs

和 `input` 一样都是通过 `bind:value` 来进行双向绑定。

```svelte
<script>
 import { marked } from 'marked';
 let value = `Some words are *italic*, some are **bold**\n\n- lists\n- are\n- cool`;
</script>

<div class="grid">
 input
 <textarea bind:value></textarea>

 output
 <div>{@html marked(value)}</div>
</div>

<style>
 .grid {
  display: grid;
  grid-template-columns: 5em 1fr;
  grid-template-rows: 1fr 1fr;
  grid-gap: 1em;
  height: 100%;
 }

 textarea {
  flex: 1;
  resize: none;
 }
</style>
```

###### 1.1.12.8 总结知识点

- `Text`、`Numeric`（DOM默认所有输入值都是字符串，对于输入值为数字，svelte 会帮我们做字符串到数字的类型转换）、`Select`（当 `option` 的 `value` 和内容相同时，可以省略 `value`）、`Textarea` 都是通过 `bind:value` 来进行双向绑定，单个 `Checkbox` 则是使用 `checked` 来双向绑定，当有多个 `radio` or `checkbox` 时，通过 `bind:group` 来进行双向绑定，此时 `bind:group` 可以和 `radio` or `checkbox` 的 `value` 同时存在。
- 当 `bind:value={value}` 时，可以简写为 `bind:value`。

##### 1.1.13 组件生命周期

每个组件都有一个生命周期，从它被创建时开始，到它被销毁时结束。有一些函数允许您在生命周期的关键时刻运行代码。

###### 1.1.13.1 onMount

`onMount` 生命周期函数，它的回调函数会在组件首次渲染到 DOM 后运行，同时，该回调函数的返回值也是一个函数，此函数会在组件卸载时执行。

```svelte
<script>
 import { onMount } from "svelte";
 import { paint } from './gradient.js';

 onMount(() => {
  const canvas = document.querySelector("canvas");
  const context = canvas.getContext("2d");
  let frame = requestAnimationFrame(function loop (t) {
    frame = requestAnimationFrame(loop)
    paint(context,t)
  })

  return () => {
    cancelAnimationFrame(frame)
  }
 })
</script>

<canvas width={32} height={32}></canvas>

<style>
 canvas {
  position: fixed;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  background-color: #666;
  mask: url(./svelte-logo-mask.svg) 50% 50% no-repeat;
  mask-size: 60vmin;
  -webkit-mask: url(./svelte-logo-mask.svg) 50% 50% no-repeat;
  -webkit-mask-size: 60vmin;
 }
</style>
```

gradient.js

```js
export function paint(context, t) {
 const { width, height } = context.canvas;
 const imageData = context.getImageData(0, 0, width, height);

 for (let p = 0; p < imageData.data.length; p += 4) {
  const i = p / 4;
  const x = i % width;
  const y = (i / width) >>> 0;

  const red = 64 + (128 * x) / width + 64 * Math.sin(t / 1000);
  const green = 64 + (128 * y) / height + 64 * Math.cos(t / 1000);
  const blue = 128;

  imageData.data[p + 0] = red;
  imageData.data[p + 1] = green;
  imageData.data[p + 2] = blue;
  imageData.data[p + 3] = 255;
 }

 context.putImageData(imageData, 0, 0);
}
```

###### 1.1.13.2 beforeUpdate 和 afterUpdate

`beforeUpdate` 函数在 DOM 更新之前触发，`afterUpdate`则用于在 DOM 与数据同步后运行代码。

```svelte
<script>
 import Eliza from 'elizabot';
 import {
  beforeUpdate,
  afterUpdate
 } from 'svelte';

 let div;
 let autoscroll = false;

 beforeUpdate(() => {
  if (div) {
   const scrollableDistance = div.scrollHeight - div.offsetHeight;
   autoscroll = div.scrollTop > scrollableDistance - 20
  }
 });

 afterUpdate(() => {
  if(autoscroll){
   div.scrollTo(0, div.scrollHeight)
  }
 });

 const eliza = new Eliza();
 const pause = (ms) => new Promise((fulfil) => setTimeout(fulfil, ms));

 const typing = { author: 'eliza', text: '...' };

 let comments = [];

 async function handleKeydown(event) {
  if (event.key === 'Enter' && event.target.value) {
   const comment = {
    author: 'user',
    text: event.target.value
   };

   const reply = {
    author: 'eliza',
    text: eliza.transform(comment.text)
   };

   event.target.value = '';
   comments = [...comments, comment];

   await pause(200 * (1 + Math.random()));
   comments = [...comments, typing];

   await pause(500 * (1 + Math.random()));
   comments = [...comments, reply].filter(comment => comment !== typing);
  }
 }
</script>

<div class="container">
 <div class="phone">
  <div class="chat" bind:this={div}>
   <header>
    <h1>Eliza</h1>

    <article class="eliza">
     <span>{eliza.getInitial()}</span>
    </article>
   </header>

   {#each comments as comment}
    <article class={comment.author}>
     <span>{comment.text}</span>
    </article>
   {/each}
  </div>

  <input on:keydown={handleKeydown} />
 </div>
</div>

<style>
 .container {
  display: grid;
  place-items: center;
  height: 100%;
 }

 .phone {
  display: flex;
  flex-direction: column;
  width: 100%;
  height: 100%;
 }

 header {
  display: flex;
  flex-direction: column;
  height: 100%;
  padding: 4em 0 0 0;
  box-sizing: border-box;
 }

 h1 {
  flex: 1;
  font-size: 1.4em;
  text-align: center;
 }

 .chat {
  height: 0;
  flex: 1 1 auto;
  padding: 0 1em;
  overflow-y: auto;
  scroll-behavior: smooth;
 }

 article {
  margin: 0 0 0.5em 0;
 }

 .user {
  text-align: right;
 }

 span {
  padding: 0.5em 1em;
  display: inline-block;
 }

 .eliza span {
  background-color: var(--bg-1);
  border-radius: 1em 1em 1em 0;
  color: var(--fg-1);
 }

 .user span {
  background-color: #0074d9;
  color: white;
  border-radius: 1em 1em 0 1em;
  word-break: break-all;
 }

 input {
  margin: 0.5em 1em 1em 1em;
 }

 @media (min-width: 400px) {
  .phone {
   background: var(--bg-2);
   position: relative;
   font-size: min(2.5vh, 1rem);
   width: auto;
   height: 36em;
   aspect-ratio: 9 / 16;
   border: 0.2em solid #222;
   border-radius: 1em;
   box-sizing: border-box;
   filter: drop-shadow(1px 1px 0px #222) drop-shadow(2px 2px 0px #222) drop-shadow(3px 3px 0px #222)
  }

  .phone::after {
   position: absolute;
   content: '';
   background: #222;
   width: 60%;
   height: 1em;
   left: 20%;
   top: 0;
   border-radius: 0 0 0.5em 0.5em
  }
 }

 @media (prefers-reduced-motion) {
  .chat {
   scroll-behavior: auto;
  }
 }
</style>
```

###### 1.1.13.3 tick

`tick` 函数不同于 Svelte 中的其他生命周期函数，它可以在组件的任何时刻被调用，而不仅仅是在组件初始化时。

调用 `tick` 会返回一个 Promise，这个 Promise 会在所有待处理的状态更改应用到 DOM 之后 resolve（如果没有待处理的状态更改，则立即 resolve ）。

在 Svelte 中，当你更新组件的状态时，这种更新不会立即反映到 DOM 上。相反，Svelte 会等待直到下一个微任务（microtask）阶段，这时它会检查是否还有其他更改需要应用到 DOM 上，包括其他组件的更改。

关于 tick 的示例：

- 问题
在 `<textarea>` 元素中选中一段文本后按 `Tab` 键，由于 `<textarea>` 的值发生了变化，当前的文本选择会被清除，并且光标会跳到文本的末尾，这通常不是用户期望的行为。
- 解决方案
通过使用 `tick` 函数，我们可以在 Svelte 中解决这个问题。具体来说，我们可以在触发状态更新（如：`<textarea>` 的 value 变化）后，使用 `tick` 函数等待 DOM 更新完成，然后执行一些额外的 DOM 操作（如恢复文本选择或设置光标位置）。由于 `tick` 会等待所有待处理的 DOM 更改都完成，所以在这个 Promise resolve 之后，我们可以安全地访问和操作DOM，而不用担心状态更新尚未完成。

```svelte
<script>
 import { tick } from "svelte"
 let text = `Select some text and hit the tab key to toggle uppercase`;

 async function handleKeydown(event) {
  if (event.key !== 'Tab') return;

  event.preventDefault();

  const { selectionStart, selectionEnd, value } = this;
  console.log("this=",this)
  const selection = value.slice(selectionStart, selectionEnd);

  const replacement = /[a-z]/.test(selection)
   ? selection.toUpperCase()
   : selection.toLowerCase();

  text =
   value.slice(0, selectionStart) +
   replacement +
   value.slice(selectionEnd);

  await tick()
  this.selectionStart = selectionStart;
  this.selectionEnd = selectionEnd;
 }
</script>

<textarea
 value={text}
 on:keydown={handleKeydown}
></textarea>

<style>
 textarea {
  width: 100%;
  height: 100%;
  resize: none;
 }
</style>
```

###### 1.1.13.4 总结知识点

- 在组件挂载前执行一些操作，可以使用 `onMount` 函数，它接收一个回调函数，回调函数的内容会在挂载前执行，当给它一个返回函数时，返回函数会在组件卸载时执行；
- 当需要在 DOM 更新前触发一些内容，可以用 `beforeUpdate` 函数，如果需要在 DOM 渲染后并与数据同步后运行代码，则可以用 `afterUpdate` 函数；
- 当你需要等待状态变更应用到 DOM 上后，再执行下一步的代码，可以使用 `tick` 函数，它可以在组件的任何时候被调用，调用 `tick` 会返回一个 Promise，该 Promise 会在所有待处理的状态应用到 DOM 之后 resolve。

##### 1.1.14 stores

并非所有的状态都属于 component 层次结构，当多个不相关的组件需要共享同一状态时，就需要用到 `stores`。

`stores` 是一个具有 `subscribe` 方法的对象，它允许你在 `stores` 更新时通知 `subscribe` 的各方。

###### 1.1.14.1 writable stores

`svelte/store` 中的 `writable` 方法，可以给不同组件提供数据可写共享，其接收一个 `共享状态`，返回值是一个 `对象`，有如下几个属性：

- `set` 方法，用于设置状态；
- `update` 方法，接收一个回调函数，回调函数参数为当前状态，返回值是新的状态；
- `subscribe` 方法，是一个订阅方法，其接收一个回调函数，回调函数的参数是当前状态。此外，它还有一个返回值是一个调用后取消订阅的方法。

App.svelte

```svelte
<script>
 import { count } from './stores.js';
 import Incrementer from './Incrementer.svelte';
 import Decrementer from './Decrementer.svelte';
 import Resetter from './Resetter.svelte';

 let count_value;

 count.subscribe((value) => {
  count_value = value;
 });
</script>

<h1>The count is {count_value}</h1>

<Incrementer />
<Decrementer />
<Resetter />

```

Incrementer.svelte

```svelte
<script>
 import { count } from './stores.js';

 function increment() {
  count.update(num => num + 1)
 }
</script>

<button on:click={increment}>
 +
</button>
```

Decrementer.svelte

```svelte
<script>
 import { count } from './stores.js';

 function decrement() {
  count.update((num) => num - 1)
 }
</script>

<button on:click={decrement}>
 -
</button>
```

Resetter.svelte

```svelte
<script>
 import { count } from './stores.js';

 function reset() {
  count.set(0)
 }
</script>

<button on:click={reset}>
 reset
</button>

```

store.js

```js
import { writable } from 'svelte/store';

export const count = writable(0);
```

在上面的案例中，我们提供了一个 `subscribe` 方法，但是却都没有提供取消订阅，如果组件创建并销毁多次，将导致内存泄露。

所以，我们在订阅了一个 `store` 之后，还需要在组件卸载销毁时，取消订阅。让我们稍微改造下 `App.svelte`：

App.svelte

```svelte
<script>
 import { onDestroy } from "svelte"
 import { count } from './stores.js';
 import Incrementer from './Incrementer.svelte';
 import Decrementer from './Decrementer.svelte';
 import Resetter from './Resetter.svelte';

 let count_value;

 const unsubscribe = count.subscribe((value) => {
  count_value = value;
 }); // 拿到取消订阅的方法

 onDestroy(unsubscribe) // 组件卸载时取消订阅
</script>

<h1>The count is {count_value}</h1>

<Incrementer />
<Decrementer />
<Resetter />
```

###### 1.1.14.2 auto subscriptions

对于 `writable stores` 中，订阅和取消订阅的方式，当引入的 `stores` 变多时，代码就会变得臃肿，svelte 给我们提供了一套自动订阅和取消订阅的便捷写法，`只需要在 store 的前面加上一个$符号`，即可自动订阅和取消订阅。

让我们改造下 App.svelte

```svelte
<script>
 import { onDestroy } from "svelte"
 import { count } from './stores.js';
 import Incrementer from './Incrementer.svelte';
 import Decrementer from './Decrementer.svelte';
 import Resetter from './Resetter.svelte';
</script>

<h1>The count is {$count}</h1>

<Incrementer />
<Decrementer />
<Resetter />
```

**注意：**

- 自动订阅仅适用于在组件的顶级作用域（在 script 开始和结束标签之间）声明(或导入)的 store。如果是在某个函数或 if 语句内声明的 store，则需要显式订阅 store；
- `$store`，除了可以直接在模板中用，也可以在 script 中的任何地方用，比如：事件处理程序、响应声明等；
- 任何以 `$` 开头的名称都被认为指向一个 `store`。它实际上是一个`保留字符`，Svelte 将阻止您使用 `$` 前缀声明自己的变量。

###### 1.1.14.3 Readable stores

当你不需要在外部来改变一个 `store` 时， 可以用 `readable store`。

通过 `svelte/store` 可以导出一个 `readable` 函数，它的第一个参数是一个初始值，当没有初始值时可以是 `undefined` 或 `null`，第二个参数是一个 `start` 函数，它接收一个 `set` 回调函数，并返回一个 `stop` 函数，当 `store` 获得第一个订阅者时，调用 `start` 函数，当最后一个订阅者取消订阅时，将调用 `stop` 函数。

App.svelte

```svelte
<script>
 import { time } from './stores.js';

 const formatter = new Intl.DateTimeFormat(
  'en',
  {
   hour12: true,
   hour: 'numeric',
   minute: '2-digit',
   second: '2-digit'
  }
 );
</script>

<h1>The time is {formatter.format($time)}</h1>
```

stores.js

```js
import { readable } from 'svelte/store';

export const time = readable(new Date(), function start(set) {
 const interval = setInterval(()=>{
  set(new Date())
 })

 return function stop() {
  clearInterval(interval)
 };
});
```

###### 1.1.14.4 Derived stores

当一个 `store` 需要从另一个或多个其他 `store` 派生出来，可以通过 `svelte/derived` 来实现。

`derived` 函数，接收三个参数：

- 参数1，是一个或多个 `store` （多个时为数组）；
- 参数2，是一个回调函数，该函数会返回派生值，回调函数的参数1是当前 `store`，参数2是 `set` 方法，参数3是 `update` 方法（回调可以通过第2个参数 `set` 和第3个参数 `update` 来异步设置值，并在适当的时候调用其中一个或两个参数）。**注意：**该回调函数会在第一个订阅者订阅时运行，然后在 `store` 依赖项更改时运行。
- 参数3，可以设置一个初始值（在用 `set` 和 `update` 的情况下）。

```ts
function derived<S extends Stores, T>(
 stores: S,
 fn: (
  values: StoresValues<S>,
  set: (value: T) => void,
  update: (fn: Updater<T>) => void
 ) => Unsubscriber | void,
 initial_value?: T | undefined
): Readable<T>;

function derived<S extends Stores, T>(
 stores: S,
 fn: (values: StoresValues<S>) => T,
 initial_value?: T | undefined
): Readable<T>;
```

示例代码：

App.svelte

```svelte
<script>
 import { time, elapsed } from './stores.js';

 const formatter = new Intl.DateTimeFormat(
  'en',
  {
   hour12: true,
   hour: 'numeric',
   minute: '2-digit',
   second: '2-digit'
  }
 );
</script>

<h1>The time is {formatter.format($time)}</h1>

<p>
 This page has been open for
 {$elapsed}
 {$elapsed === 1 ? 'second' : 'seconds'}
</p>
```

stores.js

```js
import { readable, derived } from 'svelte/store';

export const time = readable(new Date(), function start(set) {
 const interval = setInterval(() => {
  set(new Date());
 }, 1000);

 return function stop() {
  clearInterval(interval);
 };
});

const start = new Date();

export const elapsed = derived(
 time,
 ($time) => Math.round(($time - start) - 1000)
);
```

###### 1.1.14.5 custom stores

我们也可以在 `store` 的基础上封装 `custom store`。

App.svelte

```svelte
<script>
 import { count } from './stores.js';
</script>

<h1>The count is {$count}</h1>

<button on:click={count.increment}>+</button>
<button on:click={count.decrement}>-</button>
<button on:click={count.reset}>reset</button>
```

stores.js

```js
import { writable } from 'svelte/store';

function createCount() {
 const { subscribe, set, update } = writable(0);

 return {
  subscribe,
  increment: () => update(n => n + 1),
  decrement: () => update(n => n - 1),
  reset: () => set(0)
 };
}

export const count = createCount();
```

###### 1.1.14.6 store bindings

`store` 也可以双向绑定，并且 `store` 的赋值 `$name += '!'` 等于 `name.set($name += '!')`。示例如下：
App.svelte

```svelte
<script>
 import { name, greeting } from './stores.js';
</script>

<h1>{$greeting}</h1>
<input bind:value={$name} />

<button on:click={() => $name += '!'}>
 Add exclamation mark!
</button>
```

stores.js

```js
import { writable, derived } from 'svelte/store';

export const name = writable('world');
export const greeting = derived(name, ($name) => `Hello ${$name}!`);
```

###### 1.1.14.7 总结知识点

- 当需要在外部订阅修改一个 `store` 时，可以用 `writable`，它调用后的返回一个对象，其下 `set`、`update`、`subscribe` 方法允许你在外部获得、设置和更新 `store`。**注意：**订阅了store，记得要在组件的 `onDestroy` 中调用 `subscribe` 方法的返回值（返回值是一个取消订阅的方法）；
- 当只是在外部订阅一个 `store` 时，可以用 `readable` store，其接收两个参数，参数1是初始值，参数2是回调函数，该回调函数接收一个 `set` 方法，用于内部修改 `store`；
- 当需要基于一个或多个 `store` 派生一个新的 `store`，可以用 `derived`，它接收3个参数，参数1是一个或多个 `store` （多个时为一个数组）；参数2是回调函数，分别接收当前`store`、`set`、`update`，回调函数会返回派生的 `store`值；参数3是初始值；
- 以上 `store` 都可以通过在前面添加 `$` 来自动订阅 `store`，如：`$name` 就是自动订阅 `store name`；通过 `$name = $name + 'a'` 来便携式完成 `name.set($name + 'a')` 的操作。
- 有时候为了方便我们使用 `store`，我们可以在原来 `store` 的基础上做二次封装；
- `store` 也能双向绑定到 `bind:value` 上。

#### 1.2 Svelte 高级

#### 1.3 SvelteKit 基础

Svelte 是一个组件框架，而 SvelteKit 是一个 app 框架，它可以帮助我们解决生产构建的棘手问题，比如：

- 路由
- 服务端渲染
- 数据获取
- service workers
- ts集成
- 预渲染
- spa
- lib package
- 生产优化构建
- 部署到不同的 hosting providers
- ...

SvelteKit app 默认是服务器渲染（像传统的“多页面应用程序”或MPAs），具有出色的首次加载性能和 SEO 特征，然后也可以过渡到客户端导航（像现代的“单页面应用程序”），以避免在用户导航时重新加载所有内容（包括第三方分析代码等内容），它们可以在 js 运行的任何地方运行。

##### 项目结构

- src
  - routes
    - +pages.svelte
  - app.html
- static
  - favicon.png
- package.json
- svelte.config.js
- vite.config.js

1. `src` 是 app 源代码的存放位置；
2. `src/app.html` 是你的页面模板；
3. `src/routes` 定义了 app 的路由；
4. `static` 包含 app 部署时的所有 assets，如：favicon.png等。
5. `package.json`，它列出了项目的依赖项，包括 svelte、@sveltejs/kit、以及和 SvelteKit CLI 交互的各种 scripts。**注意**，它还指定了 "type": "module"，这意味着 `.js` 文件在默认情况下被视为原生 `js模块`，而不是传统的 `cjs` 格式。
6. `svelte.config.js`，包含项目配置， [svelte config 文档]("https://kit.svelte.dev/docs/configuration")。
7. `vite.config.js`，包含 Vite 配置。因为 SvelteKit 中使用了 Vite，所以可以使用 Vite 的特性，比如：热模块替换、TypeScript支持、静态资产处理等等。 [vite 文档]("https://cn.vitejs.dev")。

##### 1.3.1 Routing

###### 1.3.1.1 Pages

SvelteKit 使用基于文件系统的路由，这意味着你 app 的路由是由代码库中的目录定义的。

每个位于 `src/routes` 目录中的 `+page.svelte` 文件都会在 app 中创建一个页面。

假设我们目前 `app` 中的 `src` 结构是这样：

- src
  - routes
    - +pages.svelte
  - app.html

在 app 中，我们目前只有一个页面 `src/routes/+page.svelte`，它映射到根目录 `/`。如果我们导航到 `/about`，将会看到一个 `404 - not found` error。

那要解决这个问题，就是在 `routes` 下面新增 `about` 文件夹，并添加 `+page.svelte` 文件，最终目录结构是这样：

- src
  - routes
    - about
      - +page.svelte
    - +pages.svelte
  - app.html

然后就可以在 `/` 和 `/about` 之间导航了，记得要添加导航标签：`<a href="/">home</a>` 和 `<a href="/about">about</a>`。

###### 1.3.1.2 Layouts

app 的不同页面通常会共享相同的 UI，对于这些相同的 UI，在每个 +page.svelte 组件中重复实现这些UI是不明智的，我们可以创建一个 +layout.svelte 组件，并将其应用于同一目录下的所有路由。

在当前案例中，我们有两个路由，`src/routes/+page.svelte` 和 `src/routes/about/+page.svelte`，它们包含相同的导航 UI，现在我们创建一个新的文件 `src/routes/+layout.svelte`。

现在的文件结构变成这样：

- src
  - about
    - page.svelte
  - +layout.svelte
  - +page.svelte

然后导航内容从 `+page.svelte` 移动到 `+layout.svelte` 中。`<slot></slot>` 元素是页面内容的渲染位置。

一个 `+layout.svelte` 文件适用于每个子路由，包括兄弟的 `+page.svelte`（如果它存在）。你可以将布局嵌套到任意深度。

修改后的三个文件的内容如下：

`src/routes/+page.svelte`

```svelte
<h1>home</h1>
<p>this is the home page.</p>
```

`src/routes/+layout.svelte`

```svelte
<nav>
 <a href="/">home</a>
 <a href="/about">about</a>
</nav>

<slot></slot>
```

`src/routes/about/+page.svelte`

```svelte
<h1>about</h1>
<p>this is the about page.</p>
```

###### 1.3.1.3 Route parameters

##### 1.3.2 loading data

##### 1.3.3 headers and cookies

##### 1.3.4 shared modules

##### 1.3.5 forms

##### 1.3.6 api routes

##### 1.3.7 stores

##### 1.3.8 errors and redirects

#### 1.4 SvelteKit 高级

### 2、创建新项目

可以通过 [SvelteKit]("https://kit.svelte.dev")来创建，这是官方公认的应用框架。

```cmd
npm create svelte@latest myapp
cd myapp
npm i
npm run dev
```

SvelteKit 将处理调用 Svelte 编译器将 .Svelte 文件转换为 .js 文件，这些文件创建 DOM 和 .css 文件。它还提供了构建 web 应用程序所需的所有其他部分，例如：开发服务器、路由、部署和 SSR 支持。SvelteKit 使用 Vite 来构建代码。

**SvelteKit 的替代品：**
如果你出于某种原因不想使用 SvelteKit，你也可以通过运行 `npm create vite@latest` 并选择 Svelte 选项来使用 Svelte 与 Vite（但不使用 SvelteKit）。这样，npm run build 就会在 dist 目录下生成 HTML、JS 和 CSS 文件。在大多数情况下，您可能还需要选择一个路由库。
另外，所有主要的 web 打包器都有处理 Svelte 编译的插件——它会输出。js 和。css，你可以把它们插入到你的 HTML 中——但大多数其他的插件不会处理 SSR。

**Svelte 的 vscode 扩展工具：** `Svelte for VS Code`
