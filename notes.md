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
- 参数2，是`事件配置对象`，包含：`text`等属性。

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

###### 1.1.11.4

###### 1.1.11.5

##### 1.1.12

##### 1.1.13 响应性语句

##### 1.1.14 响应性语句

##### 1.1.15 响应性语句

##### 1.1.16 响应性语句

##### 1.1.17 响应性语句

##### 1.1.18 响应性语句

##### 1.1.19 响应性语句

##### 1.1.20 响应性语句

#### 1.2 Svelte 高级

#### 1.3 SvelteKit 基础

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
