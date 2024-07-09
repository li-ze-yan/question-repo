# 面试整理



#### 哪种定位会脱离文档流

在CSS中，`position`属性用于指定一个元素在文档中的定位方式。其中，以下两种定位方式会使元素脱离正常的文档流：

- 绝对定位 (`position: absolute;`)：元素的位置相对于其最近的非静态定位（即`position`不是`static`）的祖先元素进行定位。绝对定位的元素会从文档流中完全脱离出来，不占据空间，也不影响其他元素的布局；
- 固定定位 (`position: fixed;`)：元素的位置相对于浏览器窗口进行定位，即使页面滚动，元素也会保持在指定的位置。固定定位的元素同样会脱离文档流，不占据空间，不影响其他元素的布局；
- 粘性定位 (`position: sticky;`)：粘性定位可以被看作是相对定位和固定定位的混合体。元素根据正常文档流进行定位，但在达到某个滚动位置时会表现得像固定定位一样。粘性定位的元素在"粘"住之前会占据空间，之后则不占据原文档流中的空间。



#### 描述在浏览器中输入URL到展示网页的过程

1. DNS解析：浏览器将域名解析为服务器的IP地址。如果浏览器缓存中没有这个信息，它会向DNS服务器发起请求来找出对应的IP地址；
2. 建立连接：浏览器通过IP地址找到服务器，并通过TCP协议建立连接。如果网站支持HTTPS，还需要进行TLS握手过程以建立安全连接；
3. 发送HTTP请求：浏览器向服务器发送一个HTTP请求，请求中包含了想要获取的资源的路径、请求方法（如GET或POST）、头部信息等；
4. 服务器处理请求并返回HTTP响应：服务器接收到请求后，根据请求的路径和方法处理请求，然后返回一个HTTP响应。响应中包含了状态码（如200表示成功），响应头部信息，以及请求的资源内容（如HTML文档）；
5. 浏览器解析HTML：浏览器接收到HTML内容后，开始解析HTML文档，构建DOM树；
6. CSS解析和样式计算：浏览器解析所有的CSS样式信息（包括外部CSS文件和内联样式），并计算出每个DOM节点的最终样式，构建CSSOM树；
7. 渲染树构建：浏览器将DOM树和CSSOM树结合起来，创建渲染树（Render Tree），渲染树只包含需要显示的节点和它们的样式信息；
8. 布局（Layout/Reflow）：浏览器计算渲染树中每个节点的大小和位置；
9. 绘制（Paint）：浏览器根据渲染树和布局信息，将每个节点绘制到屏幕上；
10. 合成（Composite）：如果页面中有复杂的动画和过渡效果，浏览器会将各个层分别绘制，然后合成到一起，以提高性能。



#### 什么是回流，重绘

回流是浏览器为了重新渲染部分或全部页面，重新计算文档中元素的位置和几何结构的过程。

重绘是当元素的外观被改变，但没有改变布局的情况下，浏览器对这些元素的外观进行重新绘制的过程。

应当尽量避免回流，减少计算量。



#### 什么是防抖

防抖是在规定时间内，只有最后一次调用是有效的。

```javascript
function debounce(func, delay) {
	let timer = null;
	return function() {
		const context = this;
		const args = arguments;
		clearTimeout(timer);
		timer = setTimeout(() => {
			func.apply(context, args);
		}, delay)
	}
}

const debounce = (func, delay) => {
	let timer = null;
	return (...args) => {
		clearTimeout(timer);
		timer = setTimeout(() => {
			func.apply(this, args);
		}, delay);
	}
}
```



#### 什么是节流

节流是在规定时间内，只有一次调用是有效的。

```javascript
function throttling(func, delay) {
	let inThrottle;
	return function() {
		if(!inThrottle) {
            const context = this;
            const args = arguments;
            func.apply(this. args);
            inThrottle = true;
            setTimeout(function() {
                inThrottle = false;
            }, delay)
		}
	}
}

const throttling = (func, delay) => {
	let inThrottle;
	return (...args) => {
		if(!inThrottle) {
			func.apply(this, args);
			inThrottle = true;
			setTimeout(() => {
				inThrottle = false;
			}, delay)
		}
	}
}
```



#### 实现一个深拷贝

```javascript
function deepClone(source) {
  if (typeof source !== 'object' || source == null) {
    return source;
  }
  const target = Array.isArray(source) ? [] : {};
  for (const key in source) {
    if (Object.prototype.hasOwnProperty.call(source, key)) {
      if (typeof source[key] === 'object' && source[key] !== null) {
        target[key] = deepClone(source[key]);
      } else {
        target[key] = source[key];
      }
    }
  }
  return target;
}
```



#### 如果使用 `map` 处理数组，不用 `return` 会返回什么

`map`方法会返回一个新数组，但该数组的所有元素都会是`undefined`，因为`map`方法依赖于回调函数的返回值来构建新数组中的元素，如果回调函数没有返回值（即默认返回`undefined`），那么新数组的对应位置就会被填充为`undefined`。



#### 数组 `API`

`forEach` `find` `findIndex` `indexOf` `filter` `map` `reduce` `push` `pop` `shift` `unshift` `splice` `sort` `reverse` `concat` `slice` `join` `includes`



#### 字符串 `API`

`includes` `startsWith` `endsWith` `indexOf` `lastIndexOf` `match` `search` `replace` `slice` `substring` `toLowerCase` `toUpperCase` `trim` `trimStart` `trimEnd` `concat` `split` `charAt` `charCodeAt` `fromCharCode`



#### 什么是`this`指向

在JavaScript中，`this`关键字是一个特殊变量，它指向函数执行时的当前对象。`this`的值主要取决于函数是如何被调用的，而不是如何被定义或声明。这意味着在不同的调用上下文中，`this`可以指向不同的对象。

全局上下文

在全局执行上下文中（在任何函数体外部），`this`指向全局对象。在浏览器中，全局对象是`window`，而在Node.js中，全局对象是`global`。

```javascript
console.log(this === window); // 在浏览器中返回 true
```

函数上下文

函数中`this`的值取决于函数的调用方式。

**在普通函数调用中**，`this`指向全局对象（非严格模式下）或者`undefined`（严格模式下）。

```javascript
function show() {
  console.log(this);
}
show(); // 在非严格模式下，这将显示全局对象；在严格模式下，这将显示undefined
```

**在对象的方法中**，`this`指向调用该方法的对象。

```javascript
const obj = {
  show: function() {
    console.log(this);
  }
};
obj.show(); // 显示 obj
```

**在构造函数中**，`this`指向新创建的对象。

```javascript
function Person(name) {
  this.name = name;
}
const person = new Person('John');
console.log(person.name); // John
```

**在箭头函数中**，`this`被词法地绑定到它所在的上下文。这意味着箭头函数不会创建自己的`this`上下文，而是继承父作用域中的`this`值。

```javascript
const obj = {
  show: () => {
    console.log(this);
  }
};
obj.show(); // 在浏览器中，这将显示window对象，因为箭头函数没有自己的this，它从外部作用域（全局作用域）继承了this
```

显式绑定

可以使用`call()`、`apply()`和`bind()`方法显式地设置`this`的值。

**call()** 和 **apply()** 立即执行函数，并允许你指定`this`的值和参数。`call()`接受参数列表，而`apply()`接受一个参数数组。

```javascript
function show() {
  console.log(this.name);
}
const obj = {name: 'John'};
show.call(obj); // John
show.apply(obj); // John
```

**bind()** 返回一个新函数，允许你指定`this`的值和初始参数。

```javascript
const boundShow = show.bind(obj);
boundShow(); // John
```

`this`的值不是静态的，它取决于函数的调用方式。



#### 什么是跨域

跨域（Cross-Origin Resouce Sharing, CORS）是一种安全机制，用于限制一个域下的文档或脚本如何与另一个源的资源进行交互。它是由浏览器强制实施的安全策略，旨在防止恶意网站读取另一个网站的敏感数据。

跨域的基本概念

- **同源策略（Same-Origin Policy）**：Web安全模型的一部分，限制来自不同源的文档或脚本对当前源上的文档或脚本的交互。如果两个URL的协议、域名和端口号都相同，则它们具有相同的源。
- **跨源HTTP请求**：从一个源向另一个源发出的HTTP请求。例如，从`http://example.com`页面上的JavaScript尝试请求`http://api.another-example.com`数据。

跨域的问题

当你尝试从一个源的网页访问另一个源的资源时（例如，通过AJAX或Fetch API），浏览器会检查目标服务器的CORS策略。如果目标服务器没有通过CORS头部明确允许当前源的请求，浏览器将阻止该请求，导致跨域错误。

解决跨域问题

为了安全地实现跨源请求，服务器需要在其响应中包含适当的CORS头部，指示浏览器允许来自特定源的请求。

- **Access-Control-Allow-Origin**：此响应头指定哪些源可以访问资源。例如，`Access-Control-Allow-Origin: *`允许所有源访问资源，而`Access-Control-Allow-Origin: http://example.com`只允许来自`http://example.com`的请求。
- **Access-Control-Allow-Methods**：指定允许的HTTP方法（如GET, POST）。
- **Access-Control-Allow-Headers**：在预检请求（preflight request）中，指定浏览器允许在实际请求中访问的头部。

跨域是浏览器安全策略的一部分，旨在防止恶意网站访问敏感数据。通过服务器设置适当的CORS响应头，可以安全地允许跨源请求。



#### 如何使用vite优化前端项目

启用压缩

在`vite.config.js`中配置压缩选项，使用`vite-plugin-compress`插件来压缩资源

```javascript
import { defineConfig } from 'vite';
import compressPlugin from 'vite-plugin-compress';

export default defineConfig({
  plugins: [
    compressPlugin({
      // 使用gzip压缩
      ext: '.gz',
    }),
  ],
});
```

代码分割和动态导入

在你的应用中，使用动态`import()`来实现代码分割，减少初始加载时间。

```javascript
// 假设有一个大的组件或库需要按需加载
const LargeComponent = React.lazy(() => import('./LargeComponent'));

function App() {
  return (
    <React.Suspense fallback={<div>Loading...</div>}>
      <LargeComponent />
    </React.Suspense>
  );
}
```

使用ES模块

确保项目中尽可能使用ES模块导入导出，以利用Vite的快速冷启动和按需编译特性。

```javascript
// 使用ES模块导入
import { myFunction } from './myModule';

// 使用ES模块导出
export const myFunction = () => {
  // 函数体
};
```

图片和静态资源优化

使用Vite插件来优化图片和静态资源，例如`vite-plugin-imagemin`。

```javascript
import { defineConfig } from 'vite';
import viteImagemin from 'vite-plugin-imagemin';

export default defineConfig({
  plugins: [
    viteImagemin({
      gifsicle: {
        optimizationLevel: 7,
        interlaced: false,
      },
      optipng: {
        optimizationLevel: 7,
      },
      mozjpeg: {
        quality: 20,
      },
      pngquant: {
        quality: [0.8, 0.9],
        speed: 4,
      },
      svgo: {
        plugins: [
          {
            name: 'removeViewBox',
          },
          {
            name: 'removeEmptyAttrs',
            active: false,
          },
        ],
      },
    }),
  ],
});
```

配置环境变量和条件编译

在[`.env`](vscode-file://vscode-app/d:/tools/vscode/Microsoft VS Code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html)文件中定义环境变量，并在代码中使用这些变量来实现条件编译。

```javascript
// .env.production
VITE_API_URL=https://api.example.com
```

预构建依赖

Vite会自动预构建第三方依赖，但你可以通过配置来优化这一过程。

```javascript
// vite.config.js
export default defineConfig({
  optimizeDeps: {
    include: ['some-big-dependency'],
  },
});
```



#### 如何做`seo`优化

语义化的HTML结构。

使用合适的HTML标签（如`<header>`, `<nav>`, `<section>`, `<article>`, `<footer>`等）来构建页面结构，这有助于搜索引擎理解页面内容。

元数据优化。

确保每个页面有合适的`<title>`和`<meta name="description">`标签，这些是搜索引擎用来理解页面内容的重要信息。

使用`SSR`或`SSG`。

对于单页面应用（`SPA`），使用服务器端渲染（`SSR`）或静态站点生成（`SSG`）技术，以确保搜索引擎能够抓取到页面的内容。

确保内容可被抓取。

避免重要内容通过JavaScript动态生成，因为搜索引擎可能不会执行JavaScript。如果必须使用JavaScript渲染内容，请确保使用SSR或预渲染技术。

优化URL结构。

使用易于理解和有意义的URL结构，避免使用过长或含有大量参数的URL。

使用富媒体和alt标签。

对于图片和视频，确保使用`alt`属性来描述媒体内容，这有助于搜索引擎理解媒体文件的内容。

内容优化。

确保页面内容丰富、有价值，并且包含关键词。但要避免关键词堆砌，内容应自然流畅。

移动友好。

确保网站对移动设备友好，使用响应式设计，提高在移动设备上的用户体验。

使用`robots.txt`和`sitemap.xml`。

使用`robots.txt`文件来告诉搜索引擎哪些页面可以抓取，哪些不可以。同时，提供`sitemap.xml`文件，帮助搜索引擎发现网站上的所有页面。

社交媒体元数据。

使用`Open Graph`和`Twitter Cards`等社交媒体元数据，可以在社交媒体平台上更好地展示你的内容。

```javascript
<meta property="og:title" content="页面标题">
<meta property="og:description" content="页面描述">
<meta property="og:image" content="图片URL">
```



#### `get` `post`的区别，各自的使用场景是什么

`HTTP`协议定义了多种请求方法，其中`GET`和`POST`是最常用的两种。它们在发送数据到服务器时有不同的用途和行为。

`GET`请求

- **用途**: 主要用于请求数据。`GET`请求通常用于查询操作，比如从服务器获取数据。
- **数据传输**: 数据通过URL传递，附加在URL后面，形式为查询字符串参数。
- **安全性**: 由于数据在URL中可见，因此不适合传输敏感信息。
- **幂等性**: `GET`请求是幂等的，意味着多次执行相同的`GET`请求，资源的状态不会改变。
- **缓存**: `GET`请求的结果可以被浏览器或服务器缓存。
- **长度限制**: URL长度有限制，因此发送的数据量有限。

`POST`请求

- **用途**: 主要用于提交数据。`POST`请求通常用于创建或更新资源。
- **数据传输**: 数据包含在请求体中，不会显示在URL中，允许发送更多的数据。
- **安全性**: 相对于`GET`更安全，因为数据不会显示在URL中，适合传输敏感信息。
- **幂等性**: `POST`请求不是幂等的，多次执行相同的`POST`请求可能会每次都创建或修改资源。
- **缓存**: `POST`请求的响应通常不被缓存。
- **长度限制**: 理论上没有数据大小限制，适合传输大量数据。

何时使用`GET`

- 当你需要从服务器检索数据时，如用户请求查看某个页面或搜索结果。
- 当你需要的操作是安全和幂等的，即不会修改资源状态。

何时使用`POST`

- 当你需要向服务器提交数据以创建或更新资源时，如用户提交表单数据。
- 当你需要发送大量数据或二进制数据，如文件上传。
- 当操作有副作用，即会改变服务器上的资源状态时。

总结，选择`GET`还是`POST`请求方法取决于操作的性质：是否需要从服务器获取数据（使用`GET`），还是需要向服务器发送数据以执行某些操作（使用`POST`）。



#### `CSS`有哪些选择器，权重如何排列

1. **类型选择器（如`div`, `p`）**: 权重为 0,0,0,1。
2. **类选择器（如`.class`）**: 权重为 0,0,1,0。
3. **ID选择器（如`#id`）**: 权重为 0,1,0,0。
4. **属性选择器（如`[type="text"]`）**: 权重为 0,0,1,0，与类选择器相同。
5. **伪类选择器（如`:hover`）**: 权重为 0,0,1,0，与类选择器相同。
6. **伪元素选择器（如`::before`）**: 权重为 0,0,0,1，与类型选择器相同。
7. **通配符选择器（如`\*`）**: 权重为 0,0,0,0，最低权重。
8. **相邻兄弟选择器（如`h1 + p`）**: 权重根据选择器本身计算，例如，如果使用类型选择器，则为 0,0,0,2。
9. **子选择器（如`ul > li`）**: 权重根据选择器本身计算。
10. **后代选择器（如`div p`）**: 权重根据选择器本身计算。
11. **通用兄弟选择器（如`h1 ~ p`）**: 权重根据选择器本身计算。

! important >  行内样式 > ID选择器 > 类选择器 > 标签选择器 > 通配符选择器



#### `React`和`Vue`实现原理和使用上的区别

`React`和`Vue`都是现代前端开发中非常流行的`JavaScript`库和框架，它们各自拥有独特的实现原理和使用方式。以下是`React`和`Vue`在实现原理和使用上的一些主要区别：

实现原理的区别

1. **虚拟`DOM`**:
   - **`React`**: 使用虚拟`DOM`来优化`DOM`操作，通过比较新旧虚拟`DOM`树的差异（`diffing`算法），来决定如何高效地更新真实`DOM`。
   - **`Vue`**: 同样使用虚拟`DOM`，但`Vue`在组件依赖跟踪方面采用了不同的机制。`Vue`的响应式系统可以精确知道哪些组件需要重新渲染，从而减少不必要的虚拟`DOM`比较。
2. **响应式系统**:
   - **`React`**: 主要通过状态（`state`）和属性（`props`）来管理组件的响应式更新。使用`Hooks`或类组件的`setState`方法来更新状态，触发组件的重新渲染。
   - **`Vue`**: 使用响应式系统自动跟踪依赖，并在数据变化时触发视图更新。`Vue` 3引入了`Composition API`，提供了更灵活的代码组织方式，同时保留了其原有的`Options API`。

使用上的区别

1. **模板语法**:
   - **`React`**: 使用`JSX`（`JavaScript XML`）作为模板语法，允许在`JavaScript`代码中写标记语言。这意味着你可以使用`JavaScript`的全部能力来创建`UI`。
   - **`Vue`**: 使用基于`HTML`的模板语法，使得`HTML`结构更清晰。`Vue`也支持`JSX`，但其主要是通过模板来定义组件的结构。
2. **组件编写方式**:
   - **`React`**: 提供了函数组件和类组件两种方式。随着`Hooks`的引入，函数组件变得更加强大和受欢迎。
   - **`Vue`**: 提供了`Options API`和`Composition API`两种方式来定义组件。`Options API`通过对象的方式组织代码，而`Composition API`则提供了更灵活的逻辑复用和代码组织能力。
3. **生态系统和工具**:
   - **`React`**: 拥有庞大的生态系统，包括路由库（`React Router`）、状态管理库（`Redux`、`MobX`）等。`React`生态系统中的工具和库通常更加灵活和多样化。
   - **`Vue`**: 也有一个丰富的生态系统，包括官方支持的路由库（`Vue Router`）、状态管理库（`Vuex`）。`Vue`的生态系统提供了更多的官方解决方案，保证了工具之间的兼容性和一致性。



#### 说一下闭包

闭包允许一个函数访问并操作函数外部的变量

在一个外部函数中定义一个内部函数，内部函数引用外部函数的变量，外部函数返回这个内部函数。

闭包的用途

**创建私有变量**：在`JavaScript`中，利用闭包可以模拟私有变量的行为。

**模块化代码**：闭包允许组织代码为可重用的模块。

**函数柯里化**：通过闭包，可以实现函数柯里化，创建预设参数的函数。



#### 说一下函数柯里化

函数柯里化（`Currying`）是一种将使用多个参数的函数转换成一系列使用一个参数的函数的技术。

```javascript
function curry(fn) {
    return function curried(...args) {
        if (args.length >= fn.length) {
            return fn.apply(this, args);
        } else {
            return function(...args2) {
                return curried.apply(this, args.concat(args2));
            }
        }
    };
}

// 使用示例
function sum(a, b, c) {
    return a + b + c;
}

const curriedSum = curry(sum);
console.log(curriedSum(1)(2)(3)); // 输出: 6
console.log(curriedSum(1, 2)(3)); // 输出: 6
console.log(curriedSum(1)(2, 3)); // 输出: 6
```



#### 说一下 `js` 变量作用域

`JavaScript`中的变量作用域决定了变量的可访问性。作用域有两种主要类型：全局作用域和局部作用域，局部作用域又可以分为函数作用域和块级作用域。

函数作用域

- 在函数内部定义的变量具有函数作用域，只能在函数内部访问这些变量。
- 每次当函数被调用时，都会为该函数创建一个新的作用域。

块级作用域

- `ES6`引入了`let`和`const`关键字，允许我们定义具有块级作用域的变量。
- 块级作用域是指变量在声明它们的块或语句或表达式之内有效。
- 任何由一对花括号包裹的代码块（如`if`语句、循环语句）都会创建一个新的块级作用域。

​	当访问一个变量时，`JavaScript`会首先在当前作用域中查找该变量，如果没有找到，它会继续到外层作用域中查找，直到找到该变量或达到全局作用域，这种由多个作用域层级组成的链条被称为作用域



#### 说一下`HTTP`

`HTTP`（超文本传输协议）是一种用于分布式、协作式、超媒体信息系统的应用层协议。它是互联网上应用最为广泛的一种网络协议，是万维网的数据通信的基础。

基本特点

- **简单快速**：客户向服务器请求服务时，只需传送请求方法和路径。请求方法常用的有`GET`、`POST`。每种方法规定了客户与服务器联系的类型不同。
- **灵活**：`HTTP`允许传输任意类型的数据对象。正在传输的类型由`Content-Type`头部指定。
- **无状态**：`HTTP`协议是无状态协议。协议对于事务处理没有记忆能力。这意味着如果后续处理需要前面的信息，则它必须重传，这可能导致每次连接传送的数据量增大。虽然`HTTP/1.1`版本增加了持久连接的概念，以减少这种情况，但协议本身不保存任何信息。

工作流程

1. **客户端连接到Web服务器**：用户输入一个`URL`，或者点击一个网页上的链接后，`HTTP`客户端建立一个到服务器指定端口（通常是80）的`TCP`连接。
2. **发送`HTTP`请求**：客户端向服务器发送一个`HTTP`请求。请求消息包括：请求方法、`URI`、协议版本、可接受的响应内容类型以及请求头等信息。
3. **服务器接受请求并返回`HTTP`响应**：服务器解析请求并处理请求，然后返回一个`HTTP`响应消息。响应包含状态行、响应头和响应正文。
4. **释放连接`TCP`连接**：若使用`HTTP/1.0`协议，每个请求/响应客户端和服务器都要新建一个连接，完成后立即断开连接。`HTTP/1.1`支持持久连接。
5. **客户端浏览器解析`HTML`内容**：客户端浏览器首先解析状态行，查看表明请求是否成功的状态码，然后解析响应内容类型，并处理后续内容。

`HTTP`状态码

状态码是服务器告诉客户端，网页是正常还是有错误。状态码的第一个数字代表当前响应的类型：

- **1xx**：指示信息--表示请求已接收，继续处理。
- **2xx**：成功--表示请求已被成功接收、理解、接受。
- **3xx**：重定向--要完成请求必须进行更进一步的操作。
- **4xx**：客户端错误--请求有语法错误或请求无法实现。
- **5xx**：服务器错误--服务器未能实现合法的请求。



#### `HTTP`和`HTTPS`什么关系

`HTTP`（超文本传输协议）和HTTPS（超文本传输安全协议）的主要区别在于`HTTPS`是`HTTP`的安全版本。以下是它们之间的关键关系和区别：

基本关系

- **`HTTPS`*是`HTTP`的扩展**：`HTTPS`在`HTTP`的基础上增加了`SSL/TLS`协议，`SSL/TLS`是用于在两个通信应用之间提供安全通信的协议。
- **默认端口不同**：`HTTP`的默认端口是80，而`HTTPS`的默认端口是443。
- **数据加密**：`HTTPS`通过`SSL/TLS`协议对数据进行加密，保护数据在传输过程中的隐私和完整性，防止数据被窃听或篡改。

安全性

- **`HTTP`**：在`HTTP`协议下，数据以明文形式传输，没有加密措施，容易被中间人攻击，比如窃听、篡改数据。
- **`HTTPS`**：在`HTTPS`协议下，数据传输过程是加密的，即使数据被拦截，攻击者也无法直接读懂数据内容。此外，`HTTPS`还提供了身份验证，确保用户与正确的服务器进行通信。

性能

- **`HTTP`**：由于不进行数据加密，`HTTP`的速度理论上比`HTTPS`快一些。
- **`HTTPS`**：虽然加密过程会增加一些计算开销，导致`HTTPS`比`HTTP`慢，但随着技术进步，特别是`HTTP/2`和`HTTP/3`的推广，这种差异越来越小。

应用场景

- **`HTTP`**：对于不涉及敏感数据的网站，如一些只提供信息浏览的网站，可以使用`HTTP`。但现在，越来越多的网站即使是只提供信息浏览也倾向于使用`HTTPS`，以提高安全性和用户信任。
- **`HTTPS`**：对于需要处理敏感信息的网站，如电子商务、在线银行和个人信息等，使用`HTTPS`是必须的，以保护数据安全和用户隐私。



说一下`tcp`

`TCP`（传输控制协议）是一种面向连接的、可靠的、基于字节流的传输层通信协议，旨在确保数据的可靠传输。它是互联网协议套件的核心协议之一，与`IP`协议共同构成了互联网的基础。以下是`TCP`的一些关键特点和工作原理：

关键特点

- **面向连接**：在数据传输开始之前，`TCP`需要源和目标之间建立一个连接。这个过程通常被称为“三次握手”。
- **可靠传输**：`TCP`通过序列号、确认应答、重传机制等确保数据的完整性和正确性。如果数据在传输过程中丢失或出错，`TCP`会重新传输这些数据。
- **流量控制**：`TCP`使用滑动窗口协议来控制发送方的数据传输速率，防止接收方的缓冲区溢出。
- **拥塞控制**：`TCP`还实现了拥塞控制机制，以避免网络中的过度拥塞。这包括慢启动、拥塞避免、快速重传和快速恢复等算法。
- **数据排序**：`TCP`通过序列号对接收到的数据包进行排序，确保数据以正确的顺序交付给应用层。
- **全双工通信**：`TCP`允许数据在两个方向上同时传输，实现全双工通信。

工作原理

1. **三次握手**：建立连接时，客户端和服务器之间会进行三次握手。首先，客户端发送一个带有SYN标志的数据包给服务器。服务器接收后回复一个带有`SYN/ACK`标志的数据包。最后，客户端再发送一个带有`ACK`标志的数据包给服务器，完成握手过程。

2. **数据传输**：一旦连接建立，数据就可以在`TCP`连接上双向传输。`TCP`负责将数据切分成合适大小的数据包进行发送。

3. **四次挥手**：当数据传输完成后，`TCP`连接需要被关闭。这个过程通常被称为“四次挥手”。首先，发起关闭请求的一方发送一个`FIN`数据包。接收方确认这个`FIN`后，发送一个`ACK`。然后，接收方也发送一个`FIN`给发起方。最后，发起方发送一个`ACK`，完成连接的关闭。



#### `HTML`有哪些块级元素，与行内元素的区别

`HTML`元素大致分为块级元素（`Block-level elements`）和行内元素（`Inline elements`），它们在页面布局和文档结构中扮演着不同的角色。

块级元素

块级元素通常用于创建文档的结构，它们在页面上以块的形式展现，即自动占据一整行，一个块级元素后的元素会显示在新的一行上。常见的块级元素包括：

- `<div>`：用于分组和布局的通用容器。
- `<p>`：段落。
- `<h1>`, `<h2>`, `<h3>`, `<h4>`, `<h5>`, `<h6>`：六级标题。
- `<ol>`：有序列表。
- `<ul>`：无序列表。
- `<li>`：列表项。
- `<table>`：表格。
- `<form>`：表单。

行内元素

行内元素不会独占一行，它们通常用于文本内部，可以在段落或其他块级元素内部使用，不会导致文本换行。常见的行内元素包括：

- `<span>`：用于文本的分组和布局的通用容器，没有特定的语义。
- `<a>`：链接。
- `<img>`：图像。
- `<b>`：粗体文本（不推荐使用，用CSS代替）。
- `<strong>`：重要的文本。
- `<i>`：斜体文本（不推荐使用，用CSS代替）。
- `<em>`：强调文本。
- `<br>`：换行。
- `<input>`：输入框。

区别

- **布局特性**：块级元素会占据一整行，而行内元素只占据它所需的空间。
- **默认宽高**：块级元素可以设置宽度和高度，而行内元素的宽度和高度默认由内容决定。
- **嵌套规则**：块级元素可以包含行内元素和其他块级元素（有些例外，如`<p>`元素不能包含块级元素）。行内元素通常不包含块级元素，只包含其他行内元素。
- **用途**：块级元素用于结构布局，行内元素用于内容标记。



#### `JavaScript`代码实现数组去重

使用`Set`

利用`Set`对象的唯一性特性，可以轻松实现数组去重。

```javascript
const array = [1, 2, 2, 3, 4, 4, 5];
const uniqueArray = [...new Set(array)];
```

使用`filter`方法

通过`filter`方法结合`indexOf`，返回元素首次出现的位置与当前位置相同的元素。

```javascript
const array = [1, 2, 2, 3, 4, 4, 5];
const uniqueArray = array.filter((item, index) => array.indexOf(item) === index);
```

使用`reduce`方法

使用`reduce`方法累加数组，结合`includes`方法检查元素是否已经被添加到结果数组中。

```javascript
const array = [1, 2, 2, 3, 4, 4, 5];
const uniqueArray = array.reduce((acc, current) => {
  if (!acc.includes(current)) {
    acc.push(current);
  }
  return acc;
}, []);
```

使用`Map`

利用`Map`的键的唯一性，可以实现数组去重。这种方法不仅可以去重，还可以保留最后一次出现的元素。

```javascript
const array = [1, 2, 2, 3, 4, 4, 5];
const uniqueArray = Array.from(new Map(array.map(item => [item, item])).values());
```

使用`forEach`和`includes`

结合`forEach`和`includes`方法，手动检查并构建一个包含唯一元素的新数组。

```javascript
const array = [1, 2, 2, 3, 4, 4, 5];
let uniqueArray = [];
array.forEach(item => {
  if (!uniqueArray.includes(item)) {
    uniqueArray.push(item);
  }
});
```



#### `JavaScript`类怎么实现，继承，私有变量怎么实现

在`JavaScript`中，类的实现、继承以及私有变量的实现可以通过不同的方式完成。以下是一些基本示例：

类的实现

使用`class`关键字可以定义一个类。构造函数`constructor`用于初始化新创建的对象。

```javascript
class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }

  greet() {
    console.log(`Hello, my name is ${this.name} and I am ${this.age} years old.`);
  }
}
```

类的继承

使用`extends`关键字可以实现类的继承。子类可以通过`super`关键字调用父类的构造函数和方法。

```javascript
class Student extends Person {
  constructor(name, age, grade) {
    super(name, age); // 调用父类的constructor
    this.grade = grade;
  }

  study() {
    console.log(`${this.name} is studying.`);
  }
}
```

私有变量的实现

在`ES2019`之前，JavaScript类中的私有变量通常是通过约定（例如，前缀`_`）来实现的，但这并不是真正的私有属性。`ES2019`引入了私有字段，通过在字段名称前加`#`来声明。

```javascript
class Person {
  #name;
  #age;

  constructor(name, age) {
    this.#name = name;
    this.#age = age;
  }

  greet() {
    console.log(`Hello, my name is ${this.#name} and I am ${this.#age} years old.`);
  }
}
```

在这个例子中，`#name`和`#age`是私有字段，只能在类的内部访问。

使用闭包实现私有变量（`ES2019`之前的方法）

在`ES2019`引入私有字段之前，可以使用闭包来模拟私有变量。

```javascript
function Person(name, age) {
  let _name = name;
  let _age = age;

  this.greet = function() {
    console.log(`Hello, my name is ${_name} and I am ${_age} years old.`);
  };
}
```

在这个例子中，`_name`和`_age`变量对外部是不可访问的，只有通过`Person`对象的公共方法才能访问，从而实现了私有变量的效果。



#### `AJAX`原理

`AJAX`（`Asynchronous JavaScript and XML`）是一种在无需重新加载整个页面的情况下，能够更新部分网页的技术。它允许网页与服务器异步交换数据和更新部分网页内容。`AJAX`的工作原理可以分为以下几个步骤：

1. **创建`XMLHttpRequest`对象**：这是实现`AJAX`的关键。`XMLHttpRequest`是浏览器内置的一个对象，允许`JavaScript`发送`HTTP`请求到服务器并接收到数据。
2. **发送请求**：通过`XMLHttpRequest`对象的`open`方法和`send`方法，可以向服务器发送请求。`open`方法用于指定请求的类型（如`GET`或`POST`）、`URL`以及是否异步。`send`方法用于发送请求。
3. **服务器处理请求**：服务器接收到请求后，根据请求处理数据，并准备响应。
4. **接收响应**：当请求被发送并且服务器已经准备好响应时，`XMLHttpRequest`对象的状态会发生变化。可以通过设置一个事件监听器来监听`readystatechange`事件，当对象的`readyState`属性变为4（请求完成）并且`status`属性为200（成功的HTTP响应）时，表示服务器响应已经准备好被处理。
5. **更新网页内容**：一旦接收到服务器的响应，可以使用`JavaScript`来更新网页的部分内容，而无需重新加载整个页面。这通常通过操作`DOM`来实现。

```javascript
// 创建 XMLHttpRequest 对象
var xhr = new XMLHttpRequest();

// 配置请求类型、URL以及是否异步
xhr.open('GET', 'your-server-endpoint', true);

// 设置响应的类型（可选）
xhr.responseType = 'json';

// 设置状态变化回调处理响应
xhr.onreadystatechange = function () {
  if (xhr.readyState === 4 && xhr.status === 200) {
    // 处理响应数据
    console.log(xhr.responseText);
    // 更新网页内容
  }
};

// 发送请求
xhr.send();
```

`AJAX`的核心优势在于其异步性，它允许用户界面无需等待服务器响应就能进行其他操作，极大地提高了用户体验。尽管`X`代表`XML`，但现在`JSON`是更常用的数据格式，因为它更轻量，且易于处理。



#### 怎么合并数组，删除数组指定位置的元素

在`JavaScript`中，合并数组和删除数组指定位置的元素可以通过以下方法实现：

合并数组

可以使用扩展运算符`...`或`Array.prototype.concat`方法来合并数组。

使用扩展运算符

```javascript
const array1 = [1, 2, 3];
const array2 = [4, 5, 6];
const mergedArray = [...array1, ...array2];
```

使用`concat`方法

```javascript
const array1 = [1, 2, 3];
const array2 = [4, 5, 6];
const mergedArray = array1.concat(array2);
```

删除数组指定位置的元素

可以使用`Array.prototype.splice`方法来删除数组中指定位置的元素。`splice`方法可以用于添加或删除数组中的元素。

删除指定位置的元素

```javascript
const array = [1, 2, 3, 4, 5];
// 删除索引为2的元素（即数字3），第二个参数表示删除的元素数量
array.splice(2, 1);
```

这段代码会修改原数组，将其改变为`[1, 2, 4, 5]`。

综合上述方法，合并数组和删除数组指定位置的元素的操作可以根据具体需求灵活使用。



#### 说一下事件委托

事件委托是一种常用的事件处理模式，它利用了事件冒泡的原理，将事件监听器设置在父节点上，而不是直接绑定到每个子节点上。这样，当子节点上的事件被触发时，事件会冒泡到父节点，由父节点上的监听器处理事件。这种方法可以减少事件监听器的数量，提高性能，特别是在处理动态元素时非常有用。

在事件委托中，`event.target`和`event.currentTarget`代表不同的元素：

- **`event.target`**：指向触发事件的元素。无论事件监听器绑定在哪个元素上，`event.target`总是指向实际发生事件动作的那个最具体的子元素。
- **`event.currentTarget`**：指向绑定事件监听器的元素。在事件委托的情况下，`event.currentTarget`指的是父节点，因为事件监听器是绑定在这个父节点上的。



#### 怎么阻止事件冒泡和浏览器默认行为 

在JavaScript中，可以通过事件对象提供的方法来阻止事件冒泡和浏览器的默认行为：

阻止事件冒泡

- 使用`event.stopPropagation()`方法可以阻止事件继续传播，防止事件冒泡到父元素。

阻止浏览器的默认行为

- 使用`event.preventDefault()`方法可以取消事件的默认动作。这对于阻止链接的跳转、提交表单等浏览器默认行为非常有用。



#### 事件循环

`JavaScript`的事件循环是其异步行为的核心机制，允许`JavaScript`在单线程中执行非阻塞操作。事件循环的工作原理可以分解为以下几个关键点：

1. **调用栈（`Call Stack`）**：
   - `JavaScript`代码执行时首先会进入调用栈。同一时间，调用栈只能执行一个任务，执行完毕后从栈顶移除。
2. **事件队列（`Event Queue`）**：
   - 当异步事件（如：`setTimeout`、点击事件等）发生时，对应的回调函数会被放入事件队列中等待执行。
3. **事件循环（`Event Loop`）**：
   - 事件循环的作用是监视调用栈和事件队列。如果调用栈为空，事件循环会从事件队列中取出第一个事件，将其对应的回调函数放入调用栈中执行。
4. **微任务（`Microtasks`）和宏任务（`Macrotasks`）**：
   - 在事件循环中，任务被分为宏任务和微任务。宏任务包括：`setTimeout`、`setInterval`、`I/O`、`UI`渲染等。微任务包括：`Promise.then`、`MutationObserver`等。
   - 每次宏任务执行完毕后，如果存在微任务队列，事件循环会优先处理所有微任务队列中的任务，然后再执行下一个宏任务。

事件循环的步骤：

1. 执行全局脚本代码，同步代码按顺序加入调用栈执行。
2. 遇到异步代码（如：`setTimeout`），将其回调函数放入相应的宏任务队列等待执行。
3. 当调用栈清空，即全局代码和当前宏任务执行完毕，事件循环检查微任务队列。如果微任务队列中有任务，执行所有微任务。
4. 微任务执行完毕后，如果调用栈为空，事件循环会从宏任务队列中取出一个任务，执行其回调函数。

微任务（`microtasks`）先于宏任务（`macrotasks`）执行。

在每个宏任务执行完毕后，`JavaScript`引擎会处理所有在该宏任务执行期间产生的微任务。这意味着微任务队列会在当前宏任务结束后、下一个宏任务开始前被清空，确保微任务优先于宏任务执行。



#### `readonly`和`disabled`的区别 

`readonly`和`disabled`都是`HTML`属性，用于控制表单元素，但它们之间有几个关键的区别：

1. **应用范围**：
   - `readonly`属性主要用于`<input>`和`<textarea>`元素。
   - `disabled`属性可以用于任何表单元素，包括`<input>`, `<textarea>`, `<select>`, 和`<button>`等。

2. **表单提交**：

   - 带有`readonly`属性的元素仍会将其值提交到表单。
   - 带有`disabled`属性的元素不会将其值提交到表单。

3. **用户交互**：

   - 用户不能修改`readonly`元素的值，但仍然可以与之交互，比如选中和复制文本。
   - `disabled`元素不仅用户不能修改，而且不能进行任何形式的交互，如点击或聚焦。

4. **样式**：

   - 默认情况下，`readonly`元素的样式与普通元素相同，除非通过CSS进行修改。
   - `disabled`元素通常会有不同的样式，以表明它们是不可用的，这也可以通过`CSS`修改。



#### `var`、`let`、`const`的区别

`var`, `let`, 和 `const` 是`JavaScript`中用于声明变量的关键字，它们之间有几个主要的区别：

1. **作用域（`Scope`）**：
   - `var`声明的变量拥有函数作用域或全局作用域。在函数外部使用`var`声明变量时，该变量属于全局作用域；在函数内部声明时，变量具有函数作用域。
   - `let`和`const`声明的变量具有块级作用域（`block scope`），即它们只在包含它们的代码块（例如：`if`语句、`for`循环）内有效。

2. **重复声明**：
   - 在同一作用域内，`var`允许重复声明同一个变量。
   - `let`和`const`不允许在同一作用域内重复声明同一个变量。

3. **变量提升（`Hoisting`）**：
   - `var`声明的变量会被提升到其作用域的顶部，但是初始化的值不会被提升。
   - `let`和`const`也存在变量提升，但它们被提升到了块的顶部，并且在声明之前访问这些变量会导致`ReferenceError`。这种状态被称为“暂时性死区（`Temporal Dead Zone`, `TDZ`）”。

4. **赋值**：
   - 使用`let`声明的变量可以被重新赋值。
   - 使用`const`声明的变量必须在声明时初始化，并且不能被重新赋值。但是，如果`const`声明的是一个对象或数组，对象的属性或数组的元素可以被修改。



#### 箭头函数和普通函数的区别

箭头函数（`Arrow functions`）和普通函数（`Function expressions` 或 `Function declarations`）在`JavaScript`中都用于定义函数，但它们之间存在几个关键的区别：

语法简洁：

- 箭头函数提供了更简洁的语法。
- 普通函数的语法相对较为冗长。

```javascript
// 箭头函数
const add = (a, b) => a + b;

// 普通函数
function add(a, b) {
  return a + b;
}
```

`this`关键字的绑定：

- 箭头函数不绑定自己的`this`，它会捕获其所在上下文的`this`值作为自己的`this`值，常用于回调函数中。
- 普通函数绑定自己的`this`，根据函数的调用方式不同（如直接调用、作为对象方法、构造函数等），`this`的值也会不同。

```javascript
const obj = {
  value: 1,
  increment: function() {
    setTimeout(function() {
      console.log(this.value); // undefined，因为`this`指向全局对象或undefined（在严格模式下）
    }, 1000);
  },
  // 在JavaScript中，函数的this值是在函数被调用时确定的，而不是在函数被定义时确定的。this的值取决于函数的调用方式。
  // 对于obj.increment方法中的setTimeout调用：
  // 这里的setTimeout中的匿名函数是一个普通函数调用，不是作为某个对象的方法调用。在非严格模式下，普通函数调用的this默认指向全局对象（在浏览器中是window对象，在Node.js中是global对象）。因此，this.value在这种情况下是undefined，因为全局对象上通常没有名为value的属性。
  // 在严格模式下（即文件或函数顶部有'use strict';声明），普通函数调用的this值不会被自动指向全局对象，而是保持为undefined。这是为了避免不小心在全局作用域中创建或修改变量，从而提高代码的安全性和可预测性。
  // 因此，在obj.increment方法中，由于setTimeout中的函数是作为普通函数调用执行的，它的this值不会指向obj对象，而是根据当前的运行模式（严格模式或非严格模式）指向undefined或全局对象。
  incrementArrow: function() {
    setTimeout(() => {
      console.log(this.value); // 1，因为箭头函数没有自己的`this`，所以`this`指向外层作用域的`this`
    }, 1000);
  }
};
```

构造函数：

- 箭头函数不能用作构造函数，尝试使用`new`关键字调用箭头函数会抛出错误。
- 普通函数可以用作构造函数。

```javascript
const Func = function() {
  this.a = 1;
};
const obj1 = new Func(); // 正常工作

const ArrowFunc = () => {
  this.a = 1;
};
// const obj2 = new ArrowFunc(); // TypeError: ArrowFunc is not a constructor
```

**`arguments`对象**：

- 箭头函数不绑定`arguments`对象，箭头函数内部如果引用`arguments`，它会取得外层函数的`arguments`。
- 普通函数有自己的`arguments`对象，表示在函数被调用时传给函数的参数列表

```javascript
function normalFunc() {
  console.log(arguments[0]); // 访问第一个参数
  console.log(arguments[1]); // 访问第二个参数
  const arrowFunc = () => {
    console.log(arguments[0]); // 在箭头函数中访问normalFunc的第一个参数
    console.log(arguments[1]); // 在箭头函数中访问normalFunc的第二个参数
  };
  arrowFunc();
}

normalFunc(1, 2); // 调用normalFunc，传入两个参数
```

适用场景：

- 箭头函数适合用在不需要使用自己`this`、`new`操作符和`arguments`对象的场景，如小型函数或回调函数。
- 普通函数适用于需要使用这些特性的更复杂的逻辑中。



#### `Promise`有哪些状态和方法 

`Promise`在`JavaScript`中是用于异步编程的一个重要概念。一个`Promise`有以下三种状态：

1. **`Pending`（等待态）**：初始状态，既不是成功，也不是失败状态。
2. **`Fulfilled`（成功态）**：意味着操作成功完成。
3. **`Rejected`（失败态）**：意味着操作失败。

`Promise`对象提供了几个用于处理异步操作的方法：

- **`Promise.prototype.then`(`onFulfilled`, `onRejected`)**：添加成功（`onFulfilled`）和/或失败（`onRejected`）回调到当前 Promise，并返回一个新的`Promise`，以便可以链式调用。
- `Promise.prototype.catch`**(`onRejected`)**：添加一个失败（`onRejected`）回调到当前`Promise`，并返回一个新的`Promise`，用于处理拒绝的情况或处理链中前面的错误。
- **`Promise.prototype.finally`(`onFinally`)**：添加一个事件处理回调（`onFinally`），并返回一个`Promise`。当`Promise`完成（无论成功还是失败）时，该回调函数会被执行。这用于执行清理操作。

此外，`Promise`对象还有几个静态方法：

- `Promise.resolve`**(`value`)**：返回一个以给定值解析后的`Promise`对象。如果该值是一个`Promise`，那么返回这个`Promise`；如果这个值是`thenable`（即带有"then"方法），返回的`Promise`会“跟随”这个`thenable`的对象，采用它的最终状态；否则返回的`Promise`将以此值完成。
- **`Promise.reject`(`reason`)**：返回一个以给定原因被拒绝的`Promise`。
- **`Promise.all`(`iterable`)**：此方法通常由多个`Promise`组成的数组作为输入。它返回一个单一的`Promise`，该`Promise`在所有输入的`Promise`都成功时被解析，或者任何一个输入的`Promise`被拒绝时被拒绝。
- **`Promise.allSettled`(`iterable`)**：接受一个`Promise`数组作为输入，返回一个`Promise`，该`Promise`在所有输入的`Promise`已经完成（无论成功还是失败）时解析，每个结果对象都包含一个状态（`fulfilled`或`rejected`）和一个对应的值或拒绝原因。
- **`Promise.race`(`iterable`)**：同样接受一个`Promise`数组作为输入，返回一个`Promise`，它将与第一个传入的`Promise`相同的解析或拒绝。
- **`Promise.any`(`iterable`)**：接受一个`Promise`数组作为输入，返回一个单一的`Promise`，该`Promise`在任何一个输入的`Promise`成功时解析，如果所有的`Promise`都失败了，则返回一个拒绝的`Promise`，并附带一个`AggregateError`类型的错误实例，包含所有`Promise`的错误。



#### 前端存储方案有哪些 

**`Cookie`**：

- 由服务器发送到用户浏览器并保存在本地的小数据片段。
- 主要用于会话管理（如用户登录状态）、个性化设置（如用户偏好设置）和跟踪用户行为（如广告目标）。
- 大小限制约为`4KB`，每个域名的`Cookie`数量也有限制。

**`LocalStorage`**：

- `HTML5`提供的一种在客户端存储数据的方式，无过期时间，数据在页面刷新后仍然可用。
- 用于存储不需要经常变动的数据，大小限制为`5MB`左右（不同浏览器可能略有差异）。

**`SessionStorage`**：

- 与`LocalStorage`类似，但其存储的数据在页面会话结束时被清除（即浏览器窗口关闭时）。
- 用于存储临时的会话数据，大小限制也为`5MB`左右。

**`IndexedDB`**：

- 一种在浏览器中存储大量结构化数据的低级`API`，允许创建、读取、导航和写入大型数据集。
- 支持事务、索引、游标等数据库特性，适用于需要在客户端存储大量数据的应用。
- 存储空间比`LocalStorage`和`SessionStorage`大得多，但具体限制取决于浏览器和用户的硬盘空间。

**`Cache API`**：

- 为网络请求和响应提供了缓存机制，主要用于支持离线`Web`应用程序，允许开发者精确控制何时添加、检索、删除缓存中的资源。
- 通常与  一起使用，以实现离线体验和性能优化。



#### `Cookie` `LocalStorage` `SessionStorage` `IndexedDB` `Cache API` 一般用于什么场景 

各种前端存储技术适用于不同的场景，以下是它们常见的使用场景：

`Cookie`

- **会话管理**：存储会话标识，如用户登录状态。
- **个性化**：存储用户偏好设置，如语言或主题。
- **跟踪**：用于用户行为跟踪，支持广告定位。

`LocalStorage`

- **长期数据存储**：存储不经常变动的数据，如用户界面的自定义设置。
- **无需服务器交互的数据**：存储可在客户端长期保持的信息，减少不必要的服务器请求。

`SessionStorage`

- **会话期间数据存储**：存储临时会话数据，如表单输入的临时保存。
- **单次会话的状态管理**：用于在页面刷新过程中保持用户界面状态。

`IndexedDB`

- **大量数据存储**：适用于需要在客户端存储大量数据的应用，如离线应用或游戏。
- **复杂查询**：支持更复杂的数据查询和存储需求，如存储文件或Blob对象。
- **离线应用**：存储应用程序的资产和数据，使其能够在离线时使用。

`Cache API`

- **资源缓存**：用于存储网络请求的资源，如`HTML`页面、`CSS`样式表、`JavaScript`文件和图片资源。
- **离线体验**：配合`Service Worker`使用，提供应用的离线访问能力，增强`PWA`（渐进式`Web`应用）的用户体验。
- **性能优化**：通过缓存重用先前请求的资源，减少网络延迟和带宽使用，提高应用加载速度。



#### `JSX`代码是如何摇身一变成为`DOM`的

`JSX`大多数情况下，都被认定为是一种模板语法，在`React`官网中对`JSX`的定义为是`JavaScript`的一种语法扩展，它和模板语言很接近，但是它充分具备`JavaScript`的能力，从`JSX`如何在`React`中生效分析来看，`JSX`经过`Babel`，会被编译为`React.createElement()`,`React.createElement()`会返回一个`React Element`的`JS`对象，所以相当于一次`JSX`，相当于一次`React.createElement()`的调用，`JSX`相当于是`React.createElement()`的语法糖，这样开发者就能使用最熟悉的类`HTML`标签语法来创建虚拟`DOM`，在降低学习成本的同时，也提高了研发效率与研发体验。

`JSX`是如何映射成为`DOM`的，首先我们要分析`React.createElement()`，它有三个入参，`type`用于标识节点的类型，可以是 类似`HTML`中`div`  `p`这种标准标签字符串，也可以是`React`组件类型和`React` `Fragment`类型，`config`是以对象形式传入，组件所有的属性都会以键值对的形式存储在`config`对象中，`children`以对象的形式传入，它主要记录的是组件标签之前的嵌套内容。

`React.createElement()`函数体拆解如下：首先二次处理`key` `ref` `self` `source`四个基础的属性值，然后遍历`config`，筛出可以提进`props`里的属性，再提取子元素，推入到`childArray`数组中，最后格式化`defaultProps`，发起`ReactElement`的调用。

整体的构建过程就是开发者发起一次`React.createElement()`调用，`React.createElement()`处理完数据后将数据打包发起一次`React Element`的调用，`React Element`将数据组装完成返回`React Element`对象，`React.createElement()`再将`React Element`对象原封不动的返回给开发者，这样就能生成一堆虚拟`DOM`。

虚拟`DOM`是如何变成真实`DOM`的呢，这就涉及到`ReatcDOM.render()`这个函数，它有三个入参，`element`是需要渲染的元素，它是一个`React Element`，`container`是一个元素挂载的目标容器，它是一个真实的`DOM`，最后是一个回调函数`callback`，用于处理渲染结束后的逻辑。



#### React 生命周期

说到生命周期，就要想到组件初始化、组件更新两种情况的思考， 组件初始化调用`render`方法，生成虚拟`DOM`，再通过`ReactDOM.render()`方法生成真实`DOM`，组件更新时调用`render`方法生成新的虚拟`DOM`，通过`diff`算法定位出两次虚拟`DOM`的差异。

组件化具有两种形态，它既是封闭的，也是开放的，对于封闭来说，在组件自身的渲染工作流中，每个组件都只处理它内部的渲染逻辑，对于开放来说，对于组件间通信来说的，`React`允许开发者基于单向数据流的原则完成组件间的通信，而组件之间的通信又将改变通信双方/某一方内部的数据，进而对渲染结果构成影响，对于`React`生命周期方法的本质就在于此。

从`React 15`开始分析。

`Mounting`阶段：组件的初始化渲染，组件挂载，初始化渲染，挂载阶段在组件生命周期中只会挂载一次，首先在`constructor()`中对`this.state`进行初始化，`compoentWillMount`触发，再触发`render`，它将需要渲染的内容返回出来，渲染结束后触发`compoentDidMount`，此时真实`DOM`已经挂载到页面上，可以在这个生命周期里执行真实`DOM`的相关操作。

`Updating`阶段：组件的更新，首先，组件更新由父组件触发，调用`componentWillReceiveProps`，再调用`shouldComponentUpdate`，如果组件更新是由自身触发的，则从`shouldComponentUpdate`开始执行，再触发`componentWillUpdate` `render` `componentDidUpdate`。

仔细分析下`componentWillReceiveProps`，从字面意思上看，是在组件的`props`内容发生变化时被触发，这种说法不严谨，在实际开发中，当父组件的`state`变化时，`componentWillReceiveProps`依旧会被执行，所以`React`官方的说法才是对的，即如果父组件导致组件重新渲染，即使`props`没有更改，也会触发`componentWillReceiveProps`的调用。

`shouldComponentUpdate(nextProps, nextState)`在`React`中，`React`会根据`shouldComponentUpdate`的返回值，来判断是否执行后面的生命周期，进而决定是否对组件进行重渲染的操作。

`Unmounting`阶段：组件的卸载，父组件被移除或者key不一致都会触发到组件的卸载。

以上就是`React 15`的生命周期。

下面是`React 16`的生命周期的理解，对于`16`版本的生命周期，大部分都保持了`15`版本的形式，但在初始化的时候，废除了`compoentWillMount` `compoentWillUpdate`，新增了`getDerivedStateFromProps` `getSnapshotBeforeUpdate`方法，在`render`中，`16`版本之前只支持返回单个元素，而`16`版本在此基础上允许我们返回元素数组和字符串。

`getDerivedStateFromProps`有且只有一个用途：使用`props`来派生/更新`state`。

`getDerivedStateFromProps`在更新和挂载两个阶段都会出现。

`getDerivedStateFromProps`是一个静态方法，它接收两个参数，`props和state`，初始化阶段，`props`为父组件传入的数据，`state`是组件自身的`state`对象，`getDerivedStateFromProps`需要一个对象格式的返回值，来更新组件的`state`，如果确实不存在`props`派生 `state`的时候，直接省略这个生命周期的编写，`getDerivedStateFromProps`方法并非是覆盖式的更新，而是针对某个属性的定向更新，在`16`版本中，只有父组件的更新才能触发`getDerivedStateFromProps`。

`getDerivedStateFromProps`可以代替`componentWillReceiveProps`实现基于`props`派生`state`，但它也只能做这一件事，`React 16`在强制推行“只用`getDerivedStateFromProps`来完成`props`到`state`的映射”，基于静态方法，无法从`getDerivedStateFromProps`中获取到执行上下文，也就无法通过`this.setState()`操作导致不可预见的副作用，为新的`fiber`架构铺路。

消失的`compoentWillUpdate`和新增的`getSnapshotBeforeUpdate`，`getSnapshotBeforeUpdate`的返回值会作为第三个参数给到`componentDidUpdate`，它的执行时机是在`render`方法之后，真实`DOM`之前，意思就是他能同时获取到更新前的真实`DOM`和更新前后的`state` `props`信息

`Fiber`是`React 16`对`React`核心算法的一次重写，`Fiber`会使同步的渲染过程变成异步的。

在`16`版本之前，每当开发者进行一次更新时，`React`都会构建一颗虚拟`DOM`树，通过与上一次得虚拟`DOM`树进行`diff`，实现对`DOM`的定向更新，这个过程是一个递归的过程，只要同步渲染不结束，主线程就一直无法释放资源，导致页面卡顿卡死的情况，而`Fiber`架构会将一个大的更新任务拆解成许多小任务，每当执行完一个小任务后，`Fiber`就会将资源释放出来，把主线程交回去，这就是渲染线程允许被打断，这就是所谓的异步渲染。

`Fiber`架构核~心在于，任务拆解、可打断。

`Fiber`架构的重要特性就是可以被打断的异步渲染模式，根据能否被打断这一标准，`React 16`的生命周期被划分为了`render`和`commit`两个阶段。

`render`阶段纯净且没有副作用，可能会被`React`暂停、终止或重新启动，`pre-commit`阶段可以读取`DOM`，`commit`阶段可以使用`DOM`，运行副作用，安排更新，总的来说，`render`阶段在执行过程中允许被打断，而`commit`阶段总是同步执行的。

为什么要这么设计，因为在`render`阶段的时候，对用户来说是无感知的，而`commit`阶段涉及到真实`DOM`的更新，会影响用户的操作。

从生命周期的角度上来看，`compoentWillMount` `componentWillUpdate`  `componentWillReceiveProps`都处于`render`阶段，且都有能被重复执行，而`getDerivedStateFromProps`避免开发者触碰`this`，就是在避免各种潜在的骚操作，比如`setState()`，发起异步请求，操作真实`DOM`等。

因此`React 16`改造生命周期的主要动机是为了配合`Fiber`架构带来的异步渲染机制，针对生命周期中长期被滥用的部分推行了具有强制性的最佳实践，确保了`Fiber`机制下数据和视图的安全性，同时也确保了生命周期方法的行为更加纯粹、可控、可预测。



#### 数据如何在`React`组件之间流动的

`React`是基于单向数据流进行组件通信的，意思是当前组件的`state`以`props`的形式流动时只能流向组件树中比自己层级更低的组件。

当存在多层层级嵌套结构的时候，组件通信就会存在大量的冗余代码，非常繁琐，不利于维护。

“发布-订阅”模式可谓是解决通信类问题的万金油，优点在于监听事件的位置和触发事件的位置是不受限的。 

```javascript
class myEventEmitter {
    constructor() {
    // eventMap 用来存储事件和监听函数之间的关系
    this.eventMap = {}
    }

    // type 这里就代表事件的名称
    on(type, handler) {
        // handler 必须是一个函数，如果不是直接报错
        if(!handler instanceof Function) {
            throw new Error("请传递一个函数")
        }
        // 判断type事件对应的队列是否存在
        if(!this.eventMap[type]) {
            // 若不存在，则新建该队列
            this.eventMap[type] = []
        }
        // 若存在，直接往队列里推入 handler
        this.eventMap[type].push(handler)
    }

    // 触发时params就是数据的载体
    emit(type, params) {
        // 假设该事件是有订阅的（对应的事件队列存在）
        if(this.eventMap[type]) {
            // 将事件队列里的handler依次执行出队
            this.eventMap[type].forEach((handler, index) => {
                // 传入params
                handler(params)
            })
        }
    }

    off(type, handler) {
        if(this.eventMap[type]) {
            this.eventMap[type].splice(this.eventMap[type].indexOf(handler)>>>0, 1)
        }
    }
}
```

下面介绍`Context API`：

`Context API`是`React`官方提供的一种组件全局通信的方式。

从编码的角度认识“三要素”：

```javascript
const { Provider , Consumer } = React.createContext(defaultValue)
```

首先分析过时的 Context 存在什么问题：

第一代码不够优雅，很难迅速判断出谁是`Provider`，谁是`Consumer`，第二无法保证数据在生产者和消费者之间的及时同步。

如果组件提供的一个`Context`发生了变化，而中间父组件的`shouldComponentUpdate`返回`false`，那么使用到该值的后代组件将不会进行更新，使用了`Content`的组件则完全失控，所以基本上没有办法能够可靠的更新`Context`。

新的`Context API`改进了这一点，即使组件的`shouldComponentUpdate`返回`false`， 它仍然可以穿透组件继续向后代组件进行传播，进而确保了数据生产者和数据消费者之间数据的一致性，也提供了新的语义化写法。

下面介绍`Redux`：

`Redux`是`JavaScript`状态容器，它提供可预测的状态管理。

`store`是一个单一的数据源，而且是只读的。

`action`是对变化的描述。

`reducer`负责对变化进行分发和处理。

在`Redux`的整个工作流程中，数据流是严格单向的，它通过提供一个统一的状态容器，使得数据能够自由而有序的在任意之间传输。

首先使用`createStore`创建`store`对象：

```javascript
import { createStore } from 'redux'

const store = createStore({
    reducer,
    initial_state,
    applyMiddleware(middleware1, middleware1...)
})
```

`reducer`的作用是将新的`state`返回给`store`：

```javascript
const reducer = (state, action) => {
    // 编写逻辑
    return new_state
}

// 更新规则全都写在 reducer 里
```

`action`的作用是通知`reducer`让改变发生：

```javascript
const action = {
    type: "ADD_ITEM",
    payload: "<li>text</li>"
}
```



#### `React-Hooks`设计动机与工作模式

它是`React`团队在`React`组件开发实践中逐渐认知到的一个改进点，背后涉及到对类组件和函数组件两种组件形式的思考和侧重。

类组件就是基于`ES6` `class`这种写法，通过继承`React.Component`的组件。

函数组件就是函数的形态存在的`React`组件，早期没有`React Hooks`的加持，函数组件并不能定义和维护`state`，又叫无状态组件。

函数组件与类组件的对比：无关“优劣”，只谈不同：

第一，类组件需要继承`class`, 函数组件不需要；

第二，类组件可以访问生命周期方法，函数组件不能；

第三，类组件中可以获取到实例化后的`this`，并基于这个`this`做各种各样的事情，而函数组件不可以；

第四，类组件中可以定义并维护`state`，而函数组件不可以。

在`React-Hooks`出现之前，类组件的能力明显强于函数组件。

类组件是面向对象编程思想的一种表征。

封装：将一类属性和方法，“聚拢”到一个`Class`里去。

继承：新的`Class`可以通过继承现有`Class`，实现对某一类属性和方法的复用。

开发者编写的逻辑在封装后是和组件粘在一起的，这使得类组件内部的逻辑难以实现拆分和复用。

在`React`的类组件中，状态和生命周期方法都是绑定在特定的类实例上的，这使得它们难以在组件之间共享和复用。例如，你不能在一个组件中复用另一个组件的状态更新逻辑或生命周期方法，除非通过继承或者高阶组件（`HOC`）等方式，但这些方式都有其局限性。

函数组件会捕获`render`内部的状态，这是两类组件最大的不同：

`React`函数组件在每次渲染时都会创建一个新的闭包，这个闭包会捕获当次渲染中的`props`、`state`和`context`。这是因为 JavaScript 的函数在每次调用时都会创建一个新的执行上下文和新的闭包。

当你在函数组件中使用`React Hooks`（如 `useState` 或 `useEffect`）时，这些`Hooks`会利用这个特性来保存和更新状态。例如，当你调用 `useState Hook`，`React`会在内部保存一个状态值，并在每次渲染时返回这个状态值。当你调用返回的`setter`函数时，`React`会更新这个状态值，并重新渲染组件。

这种模式使得函数组件可以在没有类和 `this` 的情况下保存状态，同时也使得你可以在自定义`Hooks`中复用状态逻辑。

`React`组件本身的定位就是函数，一个吃进数据、吐出`UI`的函数，把声明式的代码转换为命令式的`DOM`操作，把数据层面的描述映射到用户可见的`UI`变化中去。

以下通过两个示例解释函数组件会捕获`render`内部的状态：

```javascript
// 类组件
class ProfilePage extends React.Component {
    showMessage = () => {
		alert('Followed' + this.props.user)
    }
    
    handleClick = () => {
        setTimeout(this.showMessage, 3000)
    }
    
	render() {
        return <button onClick={this.handleClick}>Follow</button>
    }    
}

function ProfilePage(props) {
    showMessage = () => {
		alert('Followed' + props.user)
    }
    
    handleClick = () => {
        setTimeout(showMessage, 3000)
    }
}

// 场景为三秒内将 props.user 修改，函数组件和类组件打印的差异
// 类组件会打印最新的值，而函数组件会打印触发时的值
// 为什么？
// 在类组件中，虽然 props 本身是不可变的，但 this 却是可变的，this 上的数据是可以被修改的
// this.props 的调用每次都会获取最新的this
// 通过 setTimeout 将预期中的渲染推迟了3s，打破了 this.props 和渲染动作之间的这种时机上的关联
```

函数组件是一个更加匹配其设计理念，也更有利于逻辑拆分与重用的组件表达方式。

函数组件比起类组件少了很多东西，给函数组件的使用带来了局限性。

如果说函数组件是一台轻巧的快艇，那么`React-Hooks`就是一个内容丰富的零部件库，允许你自由的选择和使用你需要的那些能力。

以下为`useState`的分析：

早期的函数组件相比于类组件，其一大劣势是缺乏定义和维护`state`的能力，`useState`正是这样一个能够为函数组件引入状态的`API`。

`useState`返回的是一个数组，数组的第一个元素对应的`state`变量，第二个元素是对应的能够修改这个变量的`API`。

```javascript
const [state, setState] = useState(initialState)
```

状态和修改状态的`API`都是可以自定义的。

调用`React.useState`的时候，实际上是给这个组件关联了一个状态。

`React-Hooks`是如何帮助我们升级工作模式的：

第一，告别难以理解的`Class`；

第二，解决业务逻辑难以拆分的问题；

第三，使状态逻辑复用变得简单可行；

第四，函数组件从设计思想上来看更加契合`React`的理念。

此外，`Hoos`并不是万能的：

第一，`Hooks`暂时还不能完全的为函数组件补齐组件的能力，例如`getSnapshotBeforeUpdate` `compoentDidCatch`。

第二，“轻量”几乎是函数组件的基因，这可能会使它不能很好的消化复杂；

第三，`Hooks`在使用层面上有着严格的规则约束。



#### 深入`React-Hooks`工作机制：“原则”的背后，是“原理”

`React-Hooks`的使用原则：

第一，只在`React`函数中调用`Hook`;

第二，不要在循环、条件或嵌套函数中调用`Hook`。

要确保`Hooks`在每次渲染时都保持同样的执行顺序。

首次渲染构建链表并渲染，更新渲染依次遍历链表并渲染。

`Hooks`的渲染是通过依次遍历来定位每个`Hooks`内容的，如果前后两次读到的链表在顺序上出现差异，那么渲染的结果自然是不可控的。

所以`Hooks`的本质其实是链表。



#### 真正理解虚拟`DOM`：`React`选它，真的是为了性能吗

为什么我们需要虚拟`DOM`？

从本质上来说，`DOM`操作是很慢的，而`JS`却可以很快，直接操作`DOM`可能会导致频繁的回流与重绘，`JS`不存在这些问题，因此虚拟`DOM`比原生`DOM`更快。

以下分析为何`React`选择虚拟`DOM`。

虚拟`DOM`本质上是`JS`和`DOM`之间的一个映射缓存，在形态上表现为一个能够描述`DOM`结构及其属性信息的`JS`对象。

挂载阶段：`React`将结合`JSX`的描述，构建出虚拟`DOM`树，然后通过`ReactDOM.render`实现虚拟`DOM`到真实`DOM`的映射。

更新阶段：页面的变化会先作用于虚拟`DOM`，虚拟`DOM`将在`JS`层借助算法先对比出具体有哪些真实`DOM`需要被改变，然后再将这些改变作用于真实`DOM`。

历史长河中的`DOM`操作解决方案。

原生`JS`支配下的人肉`DOM`时期：前端页面“展示”的属性远远强于其交互的属性，这就导致`JS`的定位只能是辅助，前端会花费大量的时间去实现静态的`DOM`，再补充少量`JS`。

解放生产力的先导阶段，`JQuery`时期：大量`DOM`操作需求带来的前端开发工作量的激增，`JQuery`首先解决的就是“ `API` 不好使”这个问题，将`DOM API `封装为了相对简单和优雅的形式，同时一口气做掉了跨浏览器的兼容工作，并且提供了链式`API`调用，插件扩展等一系列能力用于进一步解放生产力。

早期模板引擎方案：`JS`和`HTML`结合在一起的规则，读取`HTML`模板并解析它，分离出其中的`JS`信息，将解析出的内容拼接成字符串，动态生成`JS`代码，运行动态生成的`JS`代码，吐出目标`HTML`，将目标`HTML`赋值给`innerHTML`，触发渲染流水线，完成真实`DOM`的渲染。

数据驱动视图：构建虚拟`DOM`树，通过`diff`对比出新旧两棵树的差异，差量更新`DOM`。

当数据内容变化非常大时，差量更新的结果和全量更新极为接近，这时模板引擎和虚拟`DOM`的效率大致一致，但每次`setState`的时候只修改少量的数据，虚拟`DOM`在性能上具备了绝对的优势。

虚拟`DOM`的价值：

第一，研发体验/研发效率的问题；

第二，跨平台的问题，一次编码，抽象成虚拟`DOM`，渲染成多个内核的代码。

第三，批量更新，在通用虚拟`DOM`库里是由`batch`函数来处理的， `batch`的作用是缓冲每次生成的补丁集，生成集中化的`DOM`更新。



#### React 中的“栈调和”（`Stack Reconciler`）过程是怎样的

调和（`Reconciliation`）过程和`Diff`算法。

`Vistual DOM`是一种编程理念，在这个概念中，`UI`以一种理想化的，或者说“虚拟的”表现形式被保存于内存中，并通过`ReactDOM`等类库使之与“真实的”`DOM`同步，这一过程叫做调和（协调）。意思就是虚拟`DOM`映射到真实`DOM`的过程。

调和过程并不能和`diff`划等号。

调和是“使一致”的过程，`Diff`是“找不同”的过程。

`Diff`策略的设计思想：

第一，若两个组件属于同一个类型，它们将拥有相同的`DOM`树形结构；

第二，处于同一层级的一组子节点，可用通过设置`key`作为唯一标识从而维持各个节点在不同渲染过程中的稳定性；

第三，`DOM`操作跨层级的节点不多，同层级操作是主流。

`Diff`逻辑的拆分与解读：

第一，`Diff`算法性能突破的关键点在于“分层对比”；

第二，类型一致的节点才有继续`Diff`的必要性；

第三，`key`属性的设置，可以帮我们尽可能重用同一层级内的节点。



#### `setState`到底是同步的，还是异步的

```javascript
import React from 'react';

export default class App extends React.Component {
    state={
        count: 0
    };
    increment = () => {
        console.log('increment setState前的count', this.state.count);
        this.setState({
            count: this.state.count + 1
        });
        console.log('increment setState后的count', this.state.count);
    }
    triple = () => {
        console.log('triple setState前的count', this.state.count);
        this.setState({
            count: this.state.count + 1
        });
        this.setState({
            count: this.state.count + 1
        });
        console.log('triple setState后的count', this.state.count);
    }
    reduce = () => {
        setTimeout(() => {
            console.log('reduce setState前的count', this.state.count);
            this.setState({
                count: this.state.count - 1
            });
            console.log('reduce setState后的count', this.state.count);
        }, 0);
    }
    render() {
        return (
            <div>
                <button onClick={this.increment}>点我增加</button>
                <button onClick={this.triple}>点我增加三倍</button>
                <button onClick={this.reduce}>点我减少</button>
            </div>
        )
    }
}
```

`increment`输出：0 0

`triple`输出：1 1

`reduce`输出：2 1

`increment`和`triple`是异步的，`reduce`是同步的，原因在那里？

异步的动机和原理——批量更新：每来一个`setState`，就把它塞进一个队列里“攒起来”，等时机成熟，再把“攒起来”的`state`结果做合并，最后只针对最新的`state`值走一次更新流程。

从源码上分析来看，在批量更新的时候，`React`都会开启一个事务锁，当这个事务锁关闭后，才会执行同步的操作，针对最新的`state`走更新流程，事务失败了就会全部不执行。

这个事务锁会在两个地方调用到，只有初始化和处理事件的时候才会设置，但当设置`setTimeout`这类异步事件时，事务锁开启后执行到异步任务`setTimeout`会放到异步队列中，事务锁马上就会关闭，这时在`setTimeout`中`setState`就会是一个同步的行为。

`setState`的表现会因调用场景的不同而不同:

第一，在`React`钩子函数及合成事件中,它表现为异步；

第二，在`setTimeout`、`setlnterval`等函数中，包括在`DOM`原生事件中,它都表现为同步。



#### 如何理解`Fiber`架构的迭代动机与设计思想

单线程的`JavaScript`与多线程的浏览器：多线程的浏览器除了要处理`JavaScript`线程以外，还需要处理各种各样的任务线程，其中也包括负责处理 DOM 的 UI 渲染线程，而 JavaScript 线程是可以操作 DOM 的。

如果渲染线程和`JavaScript`线程同时在工作，那么渲染结果必然是难以预测的。

所以`JavaScript`线程和渲染线程必须是互斥的：当其中一个线程执行时，另一个线程只能挂起等待。

与事件线程类似，当事件被触发时，对应的任务不会立刻被执行，而是由事件线程把它添加到任务队列的末尾，等待`JavaScript`的同步代码执行完毕后，在空闲时间里执行出队。

但渲染层面的更新长时间的等待，界面长时间不更新，带给用户的体验就是所谓的卡顿，当频繁操作时，事件线程也在等待`JavaScript`，这就导致触发的事件也难以被响应。

在`15`的版本中，`Stack Reconciler`是一个同步的递归过程。

这个过程的致命性在于它是同步的，不可以被打断，`Stack Reconciler`需要的调和时间会很长，这就意味着`JavaScript`线程将长时间的霸占主线程，导致渲染卡顿/卡死，交互长时间无响应等问题。

什么是`Fiber`?

`Fiber`就是比线程还要纤细的一个过程，也就是所谓的纤程，纤程的出现意在对渲染过程实现更加精细的控制。

从架构角度来看，`Fiber`是对`React`核心算法的重写，从编码角度来看，`Fiber`是`React`内部所定义的一种数据结构，它是`Fiber`树上的一个节点单位，也就是`React 16`下新的虚拟`DOM`，从工作流的角度来看，`Fiber` 节点保存了组件需要更新的状态和副作用。

`Fiber`架构的应用目的是实现“增量渲染”。

实现增量渲染的目的，是为了实现任务的可中断，可恢复，并给不同的任务赋予不同的优先级，最终达成更加顺滑的用户体验。

`React 15`的两层架构：`Reconciler`找不同 ——`Renderer`渲染不同。

`React 16`的三层架构：`Scheduler`调度更新的优先级 ——`Reconciler`找不同 ——`Renderer`渲染不同。

每个更新任务都会被赋予一个优先级，优先级高的会先进入`Reconciler`执行，如果新产生更高优先级的任务，那么处于`Reconciler`的正在执行的任务将会被中断。

从生命周期上分析：

`React 15 render`开始，马上会执行一个停不下来的递归计算（同步），最后`commit`提交渲染。

`React 16 render`开始，会将任务拆成一个个工作单元（异步），最后`commit`提交渲染。

`render`的工作单元有着不同的优先级，`React`可以根据优先级的高低去实现工作单元的打断和恢复。

因此取缔了会产生副作用影响的生命周期：`componentwillMount` `componentwillUpdate`  `shouldComponentUpdate` `componentwillReceiveProps` 。



#### `ReactDOM.render`是如何串联渲染链路的

以首次渲染为切入点，拆解 Fiber 架构下`ReactDOM.render`所触发的渲染链路，分析初始化、`render`和`commit`等过程。

初始化阶段；

`render`阶段：`performSyncWorkOnRoot`开启；

`commit`阶段：`commitRoot`开启。

初始化阶段：

调用`legacyCreateRootFromDOMContainer`，创建`container._reactRootContainer`对象，并赋值给`root`，将`root`上的`_internalRoot`属性赋值给`fiberRoot`，将`fiberRoot`与方法入参一起，传入`updateContainer`方法，形成回调，将`updateContainer`回调作为参数传入，调用`unbatchedUpdates`。

什么是`fiberRoot`？

`FiberRoot`是一个`FiberRootNode`对象，它是整棵`fiber`树的父节点，它有一个`current`属性，它是一个`FiberNode`实例，而`FiberNode`正是`Fiber`节点对应的对象类型。

`unbatchedUpdates`做了什么？

请求当前`Fiber`节点的`lane`（优先级），结合`lane`（优先级）创建当前`Fiber`节点的`update`对象，并将其入队，调度当前节点（`rootFiber`）。

当前分析来看，`ReactDOM.render`首次渲染是一个同步的过程，它并不支持对异步行为的处理。

因为`React 16`存在三种启动方式：

```javascript
// legacy 模式 触发的是同步的渲染链路
ReactDOM.render(<App />, rootNode)

// blocking 模式 过渡模式，渐进的迁移策略
ReactDOM.createBlockingRoot(rootNode).render(<App />)
                                             
// concurrent 模式 触发的是异步的渲染链路
ReactDOM.createRoot(rootNode).render(<App />)
```

`concurrent`模式下的触发逻辑：

 在`fiber`节点中设置`mode`属性，不同的渲染模式在挂载阶段的差异本质上是`mode`属性的差异，而`mode`属性决定工作流是否是同步的还是异步的。

`createWorkInProgress`将调用`createFiber`，`workInProgress`是`createFiber`方法的返回值，`workInProgress`的`alternate`将指向`current`，`current`的`alternate`将反过来指向`workInProgress`。

在`React`中，`fiberRoot`对象是整个应用的根，`fiberRoot`对象有一个`current`属性，这个属性指向`rootFiber`，也就是应用的根组件的`Fiber`节点。

`rootFiber`是一个特殊的`Fiber`节点，它没有对应的组件，但它的子节点是应用的根组件。`rootFiber`有一个`alternate`属性，这个属性指向它的替代节点，也就是`workInProgress`树的`rootFiber`。

在`React`的更新过程中，`fiberRoot`的`current`属性始终指向`current`树的`rootFiber`，而`rootFiber`和它的`alternate`之间始终保持双向链接。这样，`React`可以在`current`树和`workInProgress`树之间快速切换。

当有新的更新时，`React`会在 `workInProgress` 树上进行，这样如果更新过程被打断，`React`可以丢弃 `workInProgress` 树，然后在有空闲时再从中断的地方开始。当所有的更新都完成后，`React`会将`workInProgress`树变为新的 `current` 树，并将新的`UI`渲染到屏幕上。

这种方式使得`React`可以在不阻塞`UI`的情况下进行复杂的更新，提高了应用的响应性能。同时，通过`alternate`属性，`React`可以复用旧的`Fiber`节点，减少了内存的使用。

到目前为止，只是创建了`fiber`树的根节点，还没有创建子节点，下面分析子节点的创建。

`createFiber`返回的是`FiberNode`，通过赋值传递给`workInProgress`，`workInProgress`节点就是`current`节点的副本。

换句话说，当渲染开始时，`React`会创建一个新的`workInProgress`树，这个树的根节点是`current`树的根节点的一份拷贝，`React`会遍历根节点的子节点。对于每个子节点，`React`会创建一个`workInProgress`节点，这个节点是`current`节点的一份拷贝，最后遍历到没有子节点，完成整个`workInProgress`树的构建。

现在数据中存在了一颗`current`树，一颗`workInProgress`树，`workInProgress`树是`current`树的副本，为什么？下面分析：

`beginWork`的入参是一对用`alternate`连接起来的`workInProgress`和`current`节点；

`beginWork`的核心逻辑是根据`fiber`节点（`workInProgress`）的`tag`属性的不同，调用不同的节点创建函数。

通过判断`tag`调用不同的函数，但所有的函数内部共同的特性都是调用`reconcileChildren`方法，生成当前节点的子节点。

`reconcileChildren`作为逻辑分发，调用`mountChildFibers`和`reconcileChildFibers`。

```javascript
var reconcileChildFibers = ChildReconciler(true)
var mountChildFibers = ChildReconciler(false)
```

他们都是`ChildReconciler`的返回值，只是入参不同。

`reconcileChildFibers`和`mountChildFibers`的不同，在于对副作用的处理不同

`ChildReconciler`中定义了大量如`placeXXX ` `deleteXXX ` `updateXXX` `reconcileXXX`等这样的函数，这些函数覆盖了对`Fiber`节点的创建、增加、删除、修改等动作，将直接或间接地被`reconcilechildFibers`所调用。
`childReconciler`的返回值是一个名为`reconcileChildFibers`的函数，这个函数是一个逻辑分发器，它将根据入参的不同，执行不同的 `Fiber`节点操作，最终返回不同的目标`Fiber`节点。

对副作用不同的定义，在于`placeXXX` `deleteXXX` `updateXXX` `reconcileXXX`等这样的函数会打上`flags`的标记。

这个副作用用于告诉渲染器当前节点需要对`DOM`进行操作，`flags`记录的是副作用的类型，数据获取、订阅或者修改`DOM`等动作。

以上就是一个`fiber`节点的创建，通过`workLoopSync`循环调用，生成`fiber`节点，`fiber`节点之间通过`child` `return` `sibling`这三个属性建立关系，其中`child` `return`记录的是父子节点的关系，而`sibling`记录的是兄弟节点关系。

下面为`completeWork`函数的分析：

`completeWork`是在`React`的渲染过程中，当一个`Fiber`节点的子节点都已经处理完毕时被调用的。也就是说，`completeWork`是在一个 `Fiber`节点的子树已经完成渲染，即将开始处理兄弟节点或父节点时被调用的。

`completeWork`的主要任务是根据新的`props`和`state`，以及子节点的变化，来更新当前`Fiber`节点的`DOM`节点和其他副作用。

创建或更新`DOM`节点：对于`host`类型的`Fiber`节点（如 `div`、`span` 等），`completeWork`会创建或更新对应的`DOM`节点。

处理副作用：`completeWork`会根据`Fiber`节点的类型和新的`props`，来处理各种副作用。例如，对于`class`组件，`completeWork`会处理生命周期方法和`refs`；对于`function`组件，`completeWork`会处理`hooks`。

链接副作用列表：`completeWork`会将当前`Fiber`节点的副作用链接到它的父节点的副作用列表中，这样在后续的提交阶段，`React`可以按照正确的顺序执行所有的副作用。

返回下一个工作单元：最后，`completeWork`会返回下一个需要处理的工作单元。如果当前`Fiber`节点有兄弟节点，那么下一个工作单元就是兄弟节点；如果没有兄弟节点，那么下一个工作单元就是父节点。



#### 剖析`Fiber`架构下`Concurrent`模式的实现原理

在`React`中，`Fiber`架构是为了支持`Concurrent`模式（并发模式）而设计的。以下是`Fiber`架构下`Concurrent`模式的实现原理：

任务分解：在`Fiber`架构中，渲染工作被分解成许多小任务，每个任务对应一个`Fiber`节点的更新。这样，`React`可以在完成一个任务后，暂停工作，去执行更高优先级的任务。

双缓冲技术：`React`使用两棵`Fiber`树（`current`树和`workInProgress`树）来实现双缓冲技术。当有新的更新时，`React`会在`workInProgress`树上进行，这样如果更新过程被打断，`React`可以丢弃`workInProgress`树，然后在有空闲时再从中断的地方开始。当所有的更新都完成后，`React`会将`workInProgress`树变为新的`current`树，并将新的`UI`渲染到屏幕上。

优先级调度：在`Concurrent`模式中，不同类型的更新有不同的优先级。例如，用户交互（如点击事件）的优先级比数据获取（如网络请求）的优先级高。当有高优先级的更新时，`React`会暂停当前的工作，去执行高优先级的更新。这样，`React`可以确保重要的更新能够快速响应，提高了应用的用户体验。

时间切片：在`Concurrent`模式中，`React`使用浏览器的`requestIdleCallback API`来实现时间切片。当浏览器有空闲时间时，`React`会利用这个时间去执行低优先级的更新。这样，`React`可以在不阻塞`UI`的情况下进行复杂的更新，提高了应用的响应性能。

通过这些技术，`Concurrent`模式使得`React`可以在处理大量更新时，仍然保持流畅的用户体验。



#### 特别的事件系统：`React`事件与`DOM`事件有何不同

`React`事件系统与`DOM`事件系统在几个关键方面有所不同：

事件委托：`React`使用了事件委托的方式来处理事件。也就是说，`React`并不是将事件处理器直接绑定到真实的`DOM`节点上，而是在`document`层级上监听所有的事件，然后在需要的时候使用`JavaScript`来模拟事件的传播过程。这种方式可以减少事件监听器的数量，提高性能。

合成事件：`React`为了抽象掉浏览器的差异，实现了一套自己的事件系统，称为合成事件（`Synthetic Event`）。合成事件系统模拟了`W3C`规范，确保所有的事件在不同的浏览器中具有一致的行为。

事件池：`React`使用了事件池的机制来提高性能。当一个事件被触发并处理后，`React`会将这个事件对象进行回收，然后在下一个事件发生时再次使用。这意味着在事件回调函数异步执行时，你不能依赖事件对象的属性，因为它们可能已经被清除或改变。

函数组件中的事件处理：在`React`的函数组件中，事件处理函数每次渲染都会被重新创建，而在类组件中，事件处理函数通常只在挂载时创建一次。

事件处理的上下文：在`React`的类组件中，事件处理函数默认不会绑定`this`，你需要在构造函数中手动绑定，或者使用箭头函数。而在`DOM`事件处理中，事件处理函数的`this`默认指向触发事件的元素。

这些差异使得`React`的事件系统可以更好地与`React`的更新和渲染机制配合，提供一致的事件处理方式，同时提高性能。



#### 揭秘`Redux`设计思想与工作原理

`Flux`架构是`Redux`背后的架构思想，可以认为`Redux`是`Flux`的一种实现形式，`Flux`并不是一个具体的框架，它是一套由`Facebook`技术团队提出的应用架构，这套架构约束的是应用处理数据的模式。

`View`视图层：

用户界面，该用户界面可以是以任何形式实现出来的，`Flux`架构与`React`之间并不存在耦合关系。

`Action`动作：

可以理解为视图层发出的消息，它会触发应用状态的改变。

`Dispatcher`派发器：

它负责对`action`进行分发。

`Store`数据层：

它是存储应用状态的仓库，此外还会定义修改状态的逻辑。

`Flux`的核心特征是单向数据流，在单向数据流下，状态的变化是可预测的。

`MVC`模式是双向数据流，用户可以通过`View`触发数据更新，也能通过`Controller`触发更新，当业务复杂的时候，数据流会变得很混乱。

`Redux`是`JavaScript`状态容器，它提供可预测的状态管理。

任何组件都可以以约定的方式从`Store`读取到全局的状态，任何组件也都可以通过合理的派发`Action`来修改全局的状态。

通过`createStore`创建一个全局状态，在`Dispatch`中，触发数据后，`Redux`首先会将`isDispatching`变量置为`true`，待`reducer`执行完毕后，再将`isDispatching`变量置为`false`，通过上锁，避免用户在`reducer`中调用`Dispatch`，造成死循环，`reducer`必须是纯净的，只执行计算的操作。

触发订阅：

通过调用`store.subscribe`来注册监听函数，当`reducer`触发完毕后，就会将`listeners`数组中的监听函数逐个执行。

`listeners`数组有两个，一个是`currentListeners`，一个是`nextListeners`两个`listeners`数组。

`currentListeners`会将引用赋给`nextListeners`，最终被执行的`listeners`数组，实际上和当前的`nextListeners`指向同一个引用。 

注册监听和触发订阅都是操作`nextListeners` ，为何还需要`currentListeners`？

当存在三个监听函数`[listenerA, listenerB, listenerC]`，当一个监听函数执行完毕后，会执行解绑的操作，监听函数会从`nextListeners`弹出，但`for`循环不能感知到`nextListeners`的变化，当进入到下一下标的数据时，不存在监听函数，返回`undefined`，则会报错，因此当这个循环开始时，`currentListeners`会剥离自己的引用赋给`nextListeners`，`nextListeners`和`currentListeners`将不存在关联。

`currentListeners`数组用于确保监听函数执行过程的稳定性，`currentListeners`就是为了记录当前正在工作的`listeners`数组的引用，将它与可能发生变化的`nextListeners`区分开，确保监听函数在执行过程中的稳定性。



#### 从`Redux`中间件实现原理切入理解“面向切面编程”

面向切面编程（`Aspect-Oriented Programming，AOP`）是一种编程范式，其主要目标是提高模块化，通过将横切关注点（`cross-cutting concerns`）从业务逻辑中分离出来。横切关注点是那些散布在多个模块中，但并不属于模块核心功能的代码，例如日志、事务管理、安全等。

在`Redux`中，中间件就是一种实现`AOP`的方式。`Redux`中间件提供了一个在派发`action`到达`reducer`之前，插入自定义代码的机制。这使得我们可以在这个阶段添加各种副作用，如异步`API`调用、日志记录、错误报告等。

以下是`Redux`中间件的工作原理：

1. **中间件是一个函数**：每个`Redux`中间件都是一个函数，它接收`store`的`dispatch`和`getState`方法作为参数，并返回一个函数。
2. **中间件返回的函数接收 `next` 作为参数**：`next` 参数是一个指向`store` `dispatch` 方法的引用，或者是下一个中间件。返回的函数再次返回一个新的函数，这个新的函数接收`action`作为参数。
3. **中间件处理`action`**：在接收到`action`后，中间件可以选择在传递`action`之前和之后执行自定义代码，也可以选择是否传递`action`，或者传递不同的`action`。

通过这种方式，`Redux`中间件提供了一种模块化的方式来添加和管理横切关注点，这就是面向切面编程的思想。



#### 从`React-Router`切入系统学习前端路由解决方案

`React-Router`是如何实现路由跳转的？

1.**路由**：以`Router`为代表，负责定义路径与组件之间的映射关系；

2.**导航**：以`Link`为代表，负责触发路径的改变；

3.**路由器**：根据`Route`定义出来的映射关系，为新的路径匹配它对应的逻辑。

`BrowserRouter`是使用`HTML 5`的`history API`来控制路由跳转的。

`createHashHistory`是通过`URL`的`hash`属性来控制路由跳转的。

为什么需要前端路由？

`AJAX`的出现允许人们在不刷新页面的情况下发起请求，同时还有“不刷新页面即可更新页面内容”需求，在这样的背景下，出来了`SPA`（单页面应用）。

`SPA`允许页面在不刷新的情况下更新页面内容使内容切换的更加流畅。

关于`hash`，可以通过监听`hashChange`来处理。

关于`history`，通过`forward`前进一页、`back`后退一页、`go`前进后退指定页、`pushState`向浏览器历史追加记录、`replaceState`修改浏览器历史记录，可以通过监听`popstate`来处理。



#### 如何打造高性能的`React`应用

善用`shouldComponentUpdate`：

`React`组件会根据`shouldComponentUpdate`的返回值，来决定是否执行该方法之后的生命周期，进而决定是否对组件进行`re-render`（重渲染），`shouldComponentUpdate`的默认值是`true`，也就是无条件重渲染。

`shouldComponentUpdate`虽然在一定程度上帮我们解决了性能方面的问题，但会写很多的`shouldComponentUpdate`逻辑，这时候我们需要`PureComponent`。

`PureComponent`提前帮你安排好更新判定逻辑。

`PureComponent`将会在`shouldComponentUpdate`中对组件更新前后的`props`和`state`进行浅对比，并根据对比结果决定是否需要更新。类组件通过继承`React.PureComponent`实现。

但`PureComponent`也会存在问题：

1.若数据内容没变，但是引用变了，那么浅比较仍然认为数据发生了变化，进而触发一次不必要的更新，导致过度渲染。

2.若数据内容变了，但是引用没变，那么浅比较会认为数据没有发生变化，进而阻断一次更新，导致不渲染。

这时候需要`Immutable`，让前后变化的判断精准。

“持久性数据”，指的是这个数据只要被创建出来了，就不能被更改我们对当前数据的任何修改动作，都会导致一个新的对象的返回。

以上为类组件的优化，下面为函数组件的优化。

在函数组件中，`React.useMemo`就是类组件中`shouldComponentUpdate / PureComponent`的最佳实践。

如果希望复用的是组件中的某一个或几个部分这种更加“精细化”的管控，就需要`useMemo`。



#### `React`中数据不可变

不可变数据就是一旦被创建，就不能再被修改的数据，对数据的任何操作都会返回一个新的对象。

`Immutable`实现的原理是`Persistent Data Structure`（持久化数据结构），也就是使用旧数据创建新数据时，要保证旧数据同时可用且不变。同时为了避免`deepCopy`把所有节点都复制一遍带来的性能损耗，`Immutable`使用了（结构共享），即如果对象树中一个节点发生变化，只修改这个节点和受它影响的父节点，其它节点则进行共享。

1.数据可变会造成数据很难被回溯，无法保证数据的稳定和可预测；

2.节省内存；

3.更简单的编程和调试体验；

4.可以让复杂的变化检测机制得以简单快速的实现。从而确保代价高昂的DOM更新过程只在真正需要的时候进行；

5.避免副作用。



#### `useCallback`和`useMemo`

`useCallback()` 和 `useMemo()` 是React的两个钩子（Hooks），它们都用于优化性能，但用途和工作方式有所不同。

`useCallback()`

`useCallback()` 返回一个记忆化的回调函数。这个函数只会在其依赖项发生变化时才会更新。它主要用于优化那些传递给子组件的函数，以避免因父组件的重新渲染导致不必要的子组件渲染。

```
const memoizedCallback = useCallback(
  () => {
    // 执行的操作
  },
  [deps], // 依赖项数组
);
```

`useMemo()`

`useMemo()` 返回一个记忆化的值。这个值是通过运行一个“创建”函数得到的，并且只有当依赖项发生变化时，这个值才会重新计算。它主要用于避免在每次渲染时都进行高开销的计算。

```
const memoizedValue = useMemo(() => {
  // 计算并返回一个值
  return computeExpensiveValue(a, b);
}, [a, b]); // 依赖项数组
```

使用场景对比

- **`useCallback()`** 应用于那些你希望保持引用稳定的函数，这样可以避免因为函数引用变化而触发的子组件或效果的不必要渲染。
- **`useMemo()`** 适用于那些计算开销较大的值，你不希望在每次渲染时都重新计算这些值，而是希望在特定的依赖项改变时才重新计算。



#### `TypeScript`工具类

**`Partial<T>`**

`Partial<T>` 的作用是将某个类型 `T` 的所有属性变为可选。这对于在只需要对象的部分属性时非常有用，比如在更新对象时不需要传递所有属性。

```typescript
type User = {
  id: number;
  name: string;
  email: string;
};

// 使用 Partial<T> 使 User 类型的所有属性变为可选
function updateUser(id: number, changes: Partial<User>) {
  // 更新用户信息的逻辑...
}

// 现在我们可以只传递需要更新的属性
updateUser(1, { name: "John Doe" });
```

**`Readonly<Type>`**

`Readonly<Type>` 的作用是将类型 `Type` 的所有属性设置为只读。这意味着一旦对象被创建，其属性就不能被修改。

```typescript
type Person = {
  name: string;
  age: number;
};

// 使用 Readonly<Type> 使 Person 类型的所有属性变为只读
const person: Readonly<Person> = {
  name: "Alice",
  age: 30
};

// 尝试修改属性会导致 TypeScript 编译错误
person.name = "Bob"; // Error: Cannot assign to 'name' because it is a read-only property.
```

**`Record<Keys, Type>`**

`Record<Keys, Type>` 的作用是创建一个对象类型，其属性键为 `Keys`，属性值为 `Type`。这允许你定义一个对象，它的键来自于一个类型的集合（通常是字符串或数字的字面量类型），而所有属性的值都是同一类型。

```typescript
type Role = 'admin' | 'user' | 'guest';

// 使用 Record<Keys, Type> 创建一个对象类型
// 其中，键是 Role 类型，值是 number 类型
const accessLevels: Record<Role, number> = {
  admin: 3,
  user: 2,
  guest: 1
};

// 现在 accessLevels 对象的每个属性都严格对应 Role 类型的一个值，并且每个属性的值都是 number 类型
```

**`Pick<Type, Keys>`**

`Pick<Type, Keys>` 的作用是从类型 `Type` 中选取一组属性 `Keys`，创建一个新的类型。这允许你从现有的类型中构造一个只包含特定属性的子类型。

```typescript
type User = {
  id: number;
  name: string;
  email: string;
  age: number;
};

// 使用 Pick<Type, Keys> 从 User 类型中选取 name 和 email 属性
type UserContactInfo = Pick<User, 'name' | 'email'>;

// 现在 UserContactInfo 类型只包含 name 和 email 属性
const contactInfo: UserContactInfo = {
  name: "Alice",
  email: "alice@example.com"
};
```

**`Omit<Type, Keys>`**

`Omit<Type, Keys>` 的作用是从类型 `Type` 中排除一组属性 `Keys`，创建一个新的类型。这允许你从现有的类型中构造一个不包含特定属性的子类型。

```typescript
type User = {
  id: number;
  name: string;
  email: string;
  age: number;
};

// 使用 Omit<Type, Keys> 从 User 类型中排除 email 和 age 属性
type UserWithoutEmailAndAge = Omit<User, 'email' | 'age'>;

// 现在 UserWithoutEmailAndAge 类型不包含 email 和 age 属性
const user: UserWithoutEmailAndAge = {
  id: 1,
  name: "Alice"
};
```

**`Exclude<Type, ExcludedUnion>`**

`Exclude<Type, ExcludedUnion>` 的作用是从类型 `Type` 中排除那些可以赋值给 `ExcludedUnion` 的类型，创建一个新的类型。这允许你从现有的类型或联合类型中移除特定的成员。

```typescript
type EventTypes = "click" | "scroll" | "mousemove";

// 使用 Exclude<Type, ExcludedUnion> 从 EventTypes 中排除 "scroll"
type WithoutScroll = Exclude<EventTypes, "scroll">;

// 现在 WithoutScroll 类型包含 "click" 和 "mousemove"，但不包含 "scroll"
const event: WithoutScroll = "click"; // 正确
const anotherEvent: WithoutScroll = "scroll"; // 错误：类型 '"scroll"' 不能赋值给类型 '"click" | "mousemove"'
```

**`Extract<Type, Union>`**

`Extract<Type, Union>` 的作用是从类型 `Type` 中提取那些可以赋值给 `Union` 的类型，创建一个新的类型。这允许你从现有的类型或联合类型中选择特定的成员。

```typescript
type EventTypes = "click" | "scroll" | "mousemove";

// 使用 Extract<Type, Union> 从 EventTypes 中提取 "click" 和 "scroll"
type ClickOrScroll = Extract<EventTypes, "click" | "scroll">;

// 现在 ClickOrScroll 类型只包含 "click" 和 "scroll"
const event: ClickOrScroll = "click"; // 正确
const anotherEvent: ClickOrScroll = "mousemove"; // 错误：类型 '"mousemove"' 不能赋值给类型 '"click" | "scroll"'
```

**`NonNullable<Type>`**

`NonNullable<Type>` 的作用是从类型 `Type` 中排除 `null` 和 `undefined`，创建一个新的类型。这允许你确保某个类型的值不会是 `null` 或 `undefined`。

```typescript
type MaybeString = string | null | undefined;

// 使用 NonNullable<Type> 从 MaybeString 中排除 null 和 undefined
type DefinitelyString = NonNullable<MaybeString>;

// 现在 DefinitelyString 类型只包含 string，不包含 null 或 undefined
const myString: DefinitelyString = "Hello, TypeScript!";
const myNullString: DefinitelyString = null; // 错误：类型 'null' 不能赋值给类型 'string'
```

**`Parameters<Type>`**

`Parameters<Type>` 的作用是从函数类型 `Type` 中提取其参数类型作为一个元组类型。这允许你获取函数的参数类型，以便在其他地方重用。

```typescript
function greet(name: string, age: number): string {
  return `Hello, ${name}. You are ${age} years old.`;
}

// 使用 Parameters<Type> 获取 greet 函数的参数类型
type GreetParameters = Parameters<typeof greet>;

// 现在 GreetParameters 类型是 [string, number]，即一个元组类型，表示 greet 函数的参数
function forwardGreet(...args: GreetParameters): string {
  // 可以直接将 args 传递给 greet 函数
  return greet(...args);
}
```

**`ConstructorParameters<Type>`**

`ConstructorParameters<Type>` 的作用是从构造函数类型 `Type` 中提取其参数类型作为一个元组类型。这允许你获取类构造函数的参数类型，以便在其他地方重用。

```typescript
class Person {
  name: string;
  age: number;

  constructor(name: string, age: number) {
    this.name = name;
    this.age = age;
  }
}

// 使用 ConstructorParameters<Type> 获取 Person 构造函数的参数类型
type PersonConstructorParameters = ConstructorParameters<typeof Person>;

// 现在 PersonConstructorParameters 类型是 [string, number]，即一个元组类型，表示 Person 构造函数的参数
function createPerson(...args: PersonConstructorParameters): Person {
  // 可以直接将 args 传递给 Person 构造函数
  return new Person(...args);
}
```

**`ReturnType<Type>`**

`ReturnType<Type>` 的作用是从函数类型 `Type` 中提取其返回值类型。这允许你获取函数的返回类型，以便在其他地方重用。

```typescript
function calculateArea(radius: number): number {
  return Math.PI * radius * radius;
}

// 使用 ReturnType<Type> 获取 calculateArea 函数的返回类型
type Area = ReturnType<typeof calculateArea>;

// 现在 Area 类型是 number，即 calculateArea 函数的返回类型
const area: Area = calculateArea(10);
```

**`InstanceType<Type>`**

`InstanceType<Type>` 的作用是从构造函数类型 `Type` 中提取其实例类型。这允许你获取类的实例类型，以便在其他地方重用。

```typescript
class Person {
  name: string;
  age: number;

  constructor(name: string, age: number) {
    this.name = name;
    this.age = age;
  }

  greet() {
    return `Hello, my name is ${this.name} and I am ${this.age} years old.`;
  }
}

// 使用 InstanceType<Type> 获取 Person 类的实例类型
type PersonInstance = InstanceType<typeof Person>;

// 现在 PersonInstance 类型是 Person 的实例类型
const person: PersonInstance = new Person("Alice", 30);
console.log(person.greet());
```

**`Required<Type>`**

`Required<Type>` 的作用是将类型 `Type` 的所有属性设置为必须的，即从这个类型中移除所有属性的 `optional` 标记（`?`）。这允许你确保一个类型的所有属性都是必须存在的，没有任何一个属性是可选的。

```typescript
type UserProfile = {
  name: string;
  age?: number; // 可选属性
  email?: string; // 可选属性
};

// 使用 Required<Type> 使所有 UserProfile 的属性都变成必须的
type CompleteUserProfile = Required<UserProfile>;

// 现在 CompleteUserProfile 类型要求 name, age, 和 email 属性都必须存在
const user: CompleteUserProfile = {
  name: "Alice",
  // age 和 email 在 CompleteUserProfile 中不再是可选的，因此它们都必须提供
  age: 30,
  email: "alice@example.com"
};
```

**`ThisParameterType<Type>`**

`ThisParameterType<Type>` 的作用是从函数类型 `Type` 中提取其 `this` 参数的类型。如果函数没有显式指定 `this` 参数，`ThisParameterType` 将提取出 `unknown` 类型。这允许你获取函数中 `this` 的类型，以便在其他地方重用或进行类型检查。

```typescript
const user = {
  name: "Alice",
  greet(this: { name: string }) {
    return `Hello, my name is ${this.name}.`;
  }
};

// 使用 ThisParameterType<Type> 获取 greet 方法的 this 参数类型
type UserThisType = ThisParameterType<typeof user.greet>;

// 现在 UserThisType 类型是 { name: string }
function externalGreet(this: UserThisType) {
  // 这里可以安全地使用 this.name，因为我们知道 this 的类型
  console.log(`Hello, my name is ${this.name}.`);
}

// 使用 apply 方法来指定 externalGreet 函数中 this 的值
externalGreet.apply({ name: "Bob" });
```

**`OmitThisParameter<Type>`**

`OmitThisParameter<Type>` 的作用是从函数类型 `Type` 中移除 `this` 参数的类型。这允许你将一个明确包含 `this` 类型的函数转换为一个不需要显式 `this` 参数的函数类型，使其更容易在不同的上下文中重用。

```typescript
const user = {
  name: "Alice",
  greet(this: { name: string }) {
    return `Hello, my name is ${this.name}.`;
  }
};

// 使用 OmitThisParameter<Type> 移除 greet 方法的 this 参数
type GreetFunction = OmitThisParameter<typeof user.greet>;

// 现在 GreetFunction 类型是一个不需要 this 参数的函数类型
const greet: GreetFunction = () => `Hello, my name is Alice.`;

console.log(greet());
```

**`ThisType<Type>`**

`ThisType<T>` 是 TypeScript 中的一个工具类型，用于在对象字面量中显式指定 `this` 的类型。这允许你在对象的方法中使用一个预定义的类型作为 `this` 的类型，从而控制和约束方法内 `this` 的属性和方法。

```typescript
type Button = {
  label: string;
  onClick: (this: Button) => void;
};

const myButton: ThisType<Button> & Button = {
  label: "Click me",
  onClick() {
    // 在这里，TypeScript 知道 `this` 是 Button 类型
    console.log(`Button clicked: ${this.label}`);
  }
};

myButton.onClick();
```



#### 前端响应式解决方案

**媒体查询 (`Media Queries`)**

CSS 媒体查询是实现响应式设计的基础。通过使用媒体查询，可以根据不同的屏幕尺寸、分辨率等条件应用不同的样式规则。

```css
@media (max-width: 600px) {
  .sidebar {
    display: none;
  }
}
```

**百分比布局**

使用百分比而不是固定像素来定义元素的宽度，可以让布局的宽度自动适应父容器的宽度，从而实现响应式。

```css
.container {
  width: 80%;
}
```

**弹性盒子 (`Flexbox`)**

`Flexbox`布局提供了一种更加灵活的方式来设计响应式布局。它可以自动调整元素的大小和顺序，以最佳方式填充可用空间。

```css
.container {
  display: flex;
  flex-wrap: wrap;
}
```

**网格布局 (`CSS Grid`)**

`CSS Grid`是一种强大的布局系统，专为创建复杂的响应式布局而设计。它允许开发者在两个维度上对元素进行精确布局。

```css
.container {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
}
```

**框架和库**

使用响应式设计框架和库（如`Bootstrap` `Foundation` `Tailwind CSS`等）可以加速响应式网站的开发。这些框架提供了预定义的响应式组件和布局系统。



#### 实现二栏布局

浮动

```css
.container::after {
  content: "";
  display: table;
  clear: both;
}

.sidebar {
  float: left;
  width: 200px;
  background-color: lightblue;
}

.content {
  overflow: hidden;
  background-color: lightcoral;
}
```

```html
<div class="container">
  <div class="sidebar">Sidebar</div>
  <div class="content">Content</div>
</div>
```

**`Flexbox`**

```css
.container {
  display: flex;
}

.sidebar {
  width: 200px; /* 固定宽度 */
  background-color: lightblue;
}

.content {
  flex: 1; /* 剩余空间 */
  background-color: lightcoral;
}
```

```html
<div class="container">
  <div class="sidebar">Sidebar</div>
  <div class="content">Content</div>
</div>
```

**`Grid`布局**

```css
.container {
  display: grid;
  grid-template-columns: 200px 1fr; /* 一栏固定宽度，一栏自适应 */
}

.sidebar {
  background-color: lightblue;
}

.content {
  background-color: lightcoral;
}
```

```html
<div class="container">
  <div class="sidebar">Sidebar</div>
  <div class="content">Content</div>
</div>
```



#### 实现三栏布局

**浮动布局**

```css
.container {
  overflow: hidden;
}
.left, .right {
  width: 200px;
  float: left;
}
.main {
  float: left;
  width: calc(100% - 400px);
}
```

```html
<div class="container">
  <div class="left">Left Sidebar</div>
  <div class="main">Main Content</div>
  <div class="right">Right Sidebar</div>
</div>
```

**绝对定位**

```css
.container {
  position: relative;
}
.left, .main, .right {
  position: absolute;
}
.left {
  left: 0;
  width: 200px;
}
.main {
  left: 200px;
  right: 200px;
}
.right {
  right: 0;
  width: 200px;
}
```

```html
<div class="container">
  <div class="left">Left Sidebar</div>
  <div class="main">Main Content</div>
  <div class="right">Right Sidebar</div>
</div>
```

**`Flexbox`**

```css
.container {
  display: flex;
}
.left, .right {
  width: 200px;
}
.main {
  flex: 1;
}
```

```html
<div class="container">
  <div class="left">Left Sidebar</div>
  <div class="main">Main Content</div>
  <div class="right">Right Sidebar</div>
</div>
```

**表格布局**

```css
.container {
  display: table;
  width: 100%;
}
.left, .main, .right {
  display: table-cell;
}
.left, .right {
  width: 200px;
}
```

```html
<div class="container">
  <div class="left">Left Sidebar</div>
  <div class="main">Main Content</div>
  <div class="right">Right Sidebar</div>
</div>
```

**网格布局**

```css
.container {
  display: grid;
  grid-template-columns: 200px auto 200px;
}
```

```html
<div class="container">
  <div class="left">Left Sidebar</div>
  <div class="main">Main Content</div>
  <div class="right">Right Sidebar</div>
</div>
```



#### 双飞翼布局

```css
/* 容器样式 */
.container {
  min-height: 100px; /* 仅为了演示，可以根据需要调整 */
}

/* 中间栏样式 */
.middle {
  float: left;
  width: 100%;
  background-color: #f2f2f2; /* 仅为了演示，可以根据需要调整 */
  position: relative;
  box-sizing: border-box;
  padding-left: 200px; /* 左侧栏宽度 */
  padding-right: 150px; /* 右侧栏宽度 */
}

/* 左侧栏样式 */
.left {
  float: left;
  width: 200px; /* 左侧栏宽度 */
  margin-left: -100%;
  background-color: #d9ead3; /* 仅为了演示，可以根据需要调整 */
  position: relative;
  left: -200px; /* 向左移动，确保与容器左边界对齐 */
}

/* 右侧栏样式 */
.right {
  float: left;
  width: 150px; /* 右侧栏宽度 */
  margin-left: -150px; /* 向左移动，确保与中间栏右边界对齐 */
  background-color: #c9daf8; /* 仅为了演示，可以根据需要调整 */
  position: relative;
  left: 150px; /* 向右移动，确保与容器右边界对齐 */
}

/* 清除浮动 */
.container::after {
  content: "";
  display: table;
  clear: both;
}
```

```html
<div class="container">
  <div class="middle">中间栏</div>
  <div class="left">左侧栏</div>
  <div class="right">右侧栏</div>
</div>
```



#### 圣杯布局

```css
/* 容器样式 */
.container {
  padding-left: 200px; /* 左侧栏宽度 */
  padding-right: 150px; /* 右侧栏宽度 */
}

/* 中间栏样式 */
.main {
  float: left;
  width: 100%;
  background-color: #f2f2f2; /* 仅为了演示，可以根据需要调整 */
}

/* 左侧栏样式 */
.left {
  float: left;
  width: 200px; /* 左侧栏宽度 */
  margin-left: -100%;
  position: relative;
  left: -200px; /* 向左移动，确保与容器左边界对齐 */
  background-color: #d9ead3; /* 仅为了演示，可以根据需要调整 */
}

/* 右侧栏样式 */
.right {
  float: left;
  width: 150px; /* 右侧栏宽度 */
  margin-left: -150px; /* 向左移动，确保与中间栏右边界对齐 */
  position: relative;
  right: -150px; /* 向右移动，确保与容器右边界对齐 */
  background-color: #c9daf8; /* 仅为了演示，可以根据需要调整 */
}

/* 清除浮动 */
.container::after {
  content: "";
  display: table;
  clear: both;
}
```

```html
<div class="container">
  <div class="main">中间栏</div>
  <div class="left">左侧栏</div>
  <div class="right">右侧栏</div>
</div>
```



#### `CSS`画一个三角形

```css
.triangle {
  width: 0;
  height: 0;
  border-left: 50px solid transparent;
  border-right: 50px solid transparent;
  border-top: 100px solid black;
}
```



#### 实现多列等高布局

```css
.container {
  display: flex;
}

.column {
  flex: 1; /* 使所有列具有相同的宽度 */
  margin: 10px; /* 可选，为列添加一些间隔 */
  padding: 20px; /* 可选，为列内容添加一些内边距 */
  background-color: lightgray; /* 可选，更清晰地看到列布局 */
}
```

```html
<div class="container">
  <div class="column">Column 1<br>More content here.</div>
  <div class="column">Column 2<br>This column has more content than the others.<br>Another line.</div>
  <div class="column">Column 3</div>
</div>
```



#### 实现水平垂直居中

**`Flexbox`**

```css
.container {
  display: flex;
  justify-content: center; /* 水平居中 */
  align-items: center; /* 垂直居中 */
  height: 100vh; /* 容器高度 */
}
```

**`Grid`布局**

```css
.container {
  display: grid;
  place-items: center; /* 水平和垂直居中 */
  height: 100vh;
}
```

**使用绝对定位和`Transform`**

```css
.container {
  position: relative;
  height: 100vh;
}

.centered {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%); /* 偏移自身宽高的一半 */
}
```

**使用绝对定位和负`Margin`**

```css
.container {
  position: relative;
  height: 100vh;
}

.centered {
  position: absolute;
  width: 200px; /* 元素宽度 */
  height: 100px; /* 元素高度 */
  top: 50%;
  left: 50%;
  margin-left: -100px; /* 宽度的一半 */
  margin-top: -50px; /* 高度的一半 */
}
```

**使用`Table-cell`和`Inline-block`**

```css
.container {
  display: table;
  width: 100%;
  height: 100vh;
}

.centered {
  display: table-cell;
  text-align: center;
  vertical-align: middle;
}

.inner {
  display: inline-block;
}
```

**使用`Margin Auto`**

```css
.centered {
  width: 50%; /* 或任意宽度 */
  margin: 0 auto; /* 水平居中 */
}
```



#### `JavaScript`有哪些数据类型，它们的区别？

`JavaScript`共有八种数据类型，分别是`Undefined` `Null` `Boolean` `Number` `String` `Object` `Symbol` `BigInt` 其中`Symbol`和`BigInt`是`ES6`中新增的数据类型：

`Symbol`代表创建后独一无二且不可变的数据类型，它主要是为了解决可能出现的全局变量冲突的问题。

`BigInt`是一种数字类型的数据，它可以表示任意精度格式的整数，使用`BigInt`可以安全地存储和操作大整数，即使这个数已经超出了`Number`能够表示的安全整数范围。

这些数据可以分为原始数据类型和引用数据类型：

栈：原始数据类型（`Undefined` `Null` `Boolean` `Number` `String`）

堆：引用数据类型（对象、数组和函数）

两种类型的区别在于存储位置的不同：

原始数据类型直接存储在栈（`stack`）中的简单数据段，占据空间小、大小固定，属于被频繁使用数据，所以放入栈中存储；

引用数据类型存储在堆（`heap`）中的对象，占据空间大、大小不固定。如果存储在栈中，将会影响程序运行的性能；引用数据类型在栈中存储了指针，该指针指向堆中该实体的起始地址。当解释器寻找引用值时，会首先检索其在栈中的地址，取得地址后从堆中获得实体。堆和栈的概念存在于数据结构和操作系统内存中，在数据结构中：

在数据结构中，栈中数据的存取方式为先进后出。

堆是一个优先队列，是按优先级来进行排序的，优先级可以按照大小来规定。

在操作系统中，内存被分为栈区和堆区：

栈区内存由编译器自动分配释放，存放函数的参数值，局部变量的值等。其操作方式类似于数据结构中的栈。

堆区内存一般由开发着分配释放，若开发者不释放，程序结束时可能由垃圾回收机制回收。



#### 数据类型检测的方式有哪些

**`typeof`**

```javascript
console.log(typeof 2)                // number
console.log(typeof true)             // boolean
console.log(typeof 'str')            // string
console.log(typeof [])               // object
console.log(typeof function(){})     // function
console.log(typeof {})               // object
console.log(typeof undefined)        // undefined
console.log(typeof null)             // object
```

其中数组、对象、null 都会被判断为 object，其他判断都正确。

**`instanceof`**

`instanceof`可以正确判断对象的类型，其内部运行机制是判断在其原型链中能否找到该类型的原型。

```javascript
console.log(2 instanceof Number)                // false
console.log(true instanceof Boolean)            // false
console.log('str' instanceof String)            // false

console.log([] instanceof Array)                // true
console.log(function(){} instanceof Function)   // true
console.log({} instanceof Object)               // true
```

可以看到，instanceof 只能正确判断引用数据类型，而不能判断基本数据类型。instanceof 运算符可以用来测试一个对象在其原型链中是否存在一个构造函数的 prototype 属性。

**`constructor`**

```javascript
console.log((2).constructor === Number)                  // true
console.log((true).constructor === Boolean)              // true
console.log(('str').constructor === String)              // true
console.log(([]).constructor === Array)                  // true
console.log((function(){}).constructor === Function)     // true
console.log(({}).constructor === Object)                 // true
```

`constructor`有两个作用，一是判断数据的类型，二是对象实例通过`constrcutor`对象访问它的构造函数。需要注意，如果创建一个对象来改变它的原型，`constructor`就不能用来判断数据类型了：

```javascript
function Fn(){};

Fn.prototype = new Array();

var f = new Fn();

console.log(f.constructor === Fn)      // false
console.log(f.constructor === Array)   // true
```

**`Object.prototype.toString.call()`**

`Object.prototype.toString.call()`使用Object 对象的原型方法`toString`来判断数据类型：

```javascript
var a = Object.prototype.toString;

console.log(a.call(2));
console.log(a.call(true));
console.log(a.call('str'));
console.log(a.call([]));
console.log(a.call(function(){}));
console.log(a.call({}));
console.log(a.call(undefined));
console.log(a.call(null));
```

同样是检测对象`obj`调用`toString`方法，`obj.toString()`的结果和`Object.prototype.toString.call(obj)`的结果不一样，这是为什么？ 这是因为`toString`是`Object`的原型方法，而`Array` `function`等类型作为`Object`的实例，都重写了`toString`方法。不同的对象类型调用`toString`方法时，根据原型链的知识，调用的是对应的重写之后的`toString`方法（`function`类型返回内容为函数体的字符串，`Array`类型返回元素组成的字符串…），而不会去调用`Object`上原型`toString`方法（返回对象的具体类型），所以采用`obj.toString()`不能得到其对象类型，只能将`obj`转换为字符串类型；因此，在想要得到对象的具体类型时，应该调用`Object`原型上的`toString`方法。



#### `null`和`undefined`区别

首先`Undefined`和`Null`都是基本数据类型，这两个基本数据类型分别都只有一个值，就是`undefined`和`null`。

`undefined`代表的含义是未定义，`null`代表的含义是空对象。一般变量声明了但还没有定义的时候会返回`undefined` `null`主要用于赋值给一些可能会返回对象的变量，作为初始化。

`undefined`在`JavaScript`中不是一个保留字，这意味着可以使用`undefined`来作为一个变量名，但是这样的做法是非常危险的，它会影响对`undefined`值的判断。我们可以通过一些方法获得安全的`undefined`值，比如说`void 0`。 

当对这两种类型使用`typeof`进行判断时，`Null`类型化会返回`“object”`，这是一个历史遗留的问题。当使用双等号对两种类型的值进行比较时会返回`true`，使用三个等号时会返回`false`。



#### `intanceof`操作符的实现原理及实现

`instanceof`运算符用于判断构造函数的`prototype`属性是否出现在对象的原型链中的任何位置。

```javascript
function myInstanceof(left, right) {
	// 获取对象的原型
	let proto = Object.getPrototypeOf(left);
	let prototype = right.prototype;
	
	while(true) {
		if(!proto) return false;
		if(proto === prototype) return true;
		proto = Object.getPrototypeOf(proto);
	}
}
```



#### 如何获取安全的`undefined`值

因为`undefined`是一个标识符，所以可以被当作变量来使用和赋值，但是这样会影响`undefined`的正常判断。表达式`void ___ `没有返回值，因此返回结果是`undefined`。`void`并不改变表达式的结果，只是让表达式不返回值。因此可以用`void 0`来获得`undefined`。



#### `Object.is()`与比较操作符`===` `==`的区别

使用双等号`==`进行相等判断时，如果两边的类型不一致，则会进行强制类型转化后再进行比较。 

使用三等号`===`进行相等判断时，如果两边的类型不一致时，不会做强制类型准换，直接返回`false`。 

使用`Object.is`来进行相等判断时，一般情况下和三等号的判断相同，它处理了一些特殊的情况，比如`-0`和`+0`不再相等，两个`NaN`是相等的。



#### 什么是`JavaScript`中的包装类型

在`JavaScript`中，基本类型是没有属性和方法的，但是为了便于操作基本类型的值，在调用基本类型的属性或方法时`JavaScript`会在后台隐式地将基本类型的值转换为对象，如：

```javascript
const a = 'abc';
a.length; // 3
a.toUpperCase(); // 'ABC'
```

在 访 问`'abc'.length`时 ，`JavaScript`将`'abc'`在后台转换成`String('abc')`，然后再访问其`length`属性。`JavaScript`也可以使用`Object`函数显式地将基本类型转换为包装类型：

```javascript
var a = 'abc';
Object(a); // String {'abc'}
```

也可以使用`valueOf`方法将包装类型倒转成基本类型：

```javascript
var a = 'abc';
var b = Object(a);
var c = b.valueOf(b); // 'abc'
```

看看如下代码会打印出什么：

```javascript
var a = new Boolean(false);
if(!a) {
	console.log('Oops') // never runs
}
```

答案是什么都不会打印，因为虽然包裹的基本类型是`false`，但是`false`被包裹成包装类型后就成了对象，所以其非值为`false`，所以循环体中的内容不会运行。



#### 为什么会有`BigInt`的提案

`JavaScript`中`Number.MAX_SAFE_INTEGER`表示最⼤安全数字，计算结果是`9007199254740991`，即在这个数范围内不会出现精度丢失（⼩数除外）。但是⼀旦超过这个范围，`js`就会出现计算不准确的情况，这在⼤数计算的时候不得不依靠⼀些第三⽅库进⾏解决，因此官⽅提出了`BigInt`来解决此问题。



#### 如何判断一个对象是空对象

使用`JSON`自带的`.stringify`方法来判断：

```javascript
if(JSON.stringify(obj) === '{}') {
	console.log('空对象');
}
```

使用`ES6`新增的方法`Object.keys()`来判断：

```javascript
if(Object.keys(obj).length < 0) {
    console.log('空对象')
}
```



#### `const`对象的属性可以修改吗

`const`保证的并不是变量的值不能改动，而是变量指向的那个内存地址不能改动。对于基本类型的数据（数值、字符串、布尔值），其值就保存在变量指向的那个内存地址，因此等同于常量。

但对于引用类型的数据（主要是对象和数组）来说，变量指向数据的内存地址，保存的只是一个指针，`const`只能保证这个指针是固定不变的，至于它指向的数据结构是不是可变的，就完全不能控制了。



#### 如果`new`一个箭头函数的会怎么样

箭头函数是`ES6`中的提出来的，它没有`prototype`，也没有自己的`this`指向，更不可以使用`arguments`参数，所以不能`New`一个箭头函数。

`new`操作符的实现步骤如下：

1. 创建一个对象；
2. 将构造函数的作用域赋给新对象（也就是将对象的`__proto__`属性指向构造函数的`prototype`属性）；
3. 指向构造函数中的代码，构造函数中的`this`指向该对象（也就是为这个对象添加属性和方法）；
4. 返回新的对象。

所以，上面的第二、三步，箭头函数都是没有办法执行的。



#### 箭头函数的`this`指向哪⾥

箭头函数不同于传统`JavaScript`中的函数，箭头函数并没有属于⾃⼰的`this`，它所谓的`this`是捕获其所在上下⽂的`this`值，作为⾃⼰的`this`值，并且由于没有属于⾃⼰的`this`，所以是不会被`new`调⽤的，这个所谓的`this`也不会被改变。

可以⽤`Babel`理解⼀下箭头函数：

```javascript
// ES6
const obj = {
	getArrow() {
		return () => {
			console.log(this === obj);
		}
	}
}
```

转化后：

```javascript
var obj = {
    getArrow: function getArrow() {
        var _this = this;
        return function() {
            console.log(_this === obj);
        }
    }
}
```



#### 扩展运算符的作用及使用场景

**对象扩展运算符**

对象的扩展运算符(`...`)用于取出参数对象中的所有可遍历属性，拷贝到当前对象之中。

```javascript
let bar = { a: 1, b: 2 };
let baz = { ...bar }; // { a: 1, b: 2 }
```

上述方法实际上等价于：

```javascript
let bar = { a: 1, b: 2 };
let baz = Object.assign({}, bar); // { a: 1, b: 2 }
```

`Object.assign`方法用于对象的合并，将源对象（`source`）的所有可枚举属性，复制到目标对象（`target`）。`Object.assign`方法的第一个参数是目标对象，后面的参数都是源对象。(如果目标对象与源对象有同名属性，或多个源对象有同名属性，则后面的属性会覆盖前面的属性)。 

同样，如果用户自定义的属性，放在扩展运算符后面，则扩展运算符内部的同名属性会被覆盖掉。

```javascript
let bar = { a: 1, b: 2 };
let baz = { ...bar, ...{ a: 2, b: 4 } }; // { a: 2, b: 4 }
```

利用上述特性就可以很方便的修改对象的部分属性。

在`redux`中的`reducer`函数规定必须是一个纯函数，`reducer`中的`state`对象要求不能直接修改，可以通过扩展运算符把修改路径的对象都复制一遍，然后产生一个新的对象返回。 

需要注意：扩展运算符对对象实例的拷贝属于浅拷贝。

**数组扩展运算符**

数组的扩展运算符可以将一个数组转为用逗号分隔的参数序列，且每次只能展开一层数组。

```javascript
console.log(...[1, 2, 3]); // 1 2 3
console.log(...[1, [2, 3, 4], 5]); // 1 [2, 3, 4] 5
```

下面是数组的扩展运算符的应用：

**将数组转换为参数序列**

```javascript
function add(x, y) {
	return x + y;
}
const numbers = [1, 2];
add(...numbers); // 3
```

**复制数组**

```javascript
const arr1 = [1, 2];
const arr2 = [...arr1];
```

要记住：扩展运算符(`…`)用于取出参数对象中的所有可遍历属性，拷贝到当前对象之中，这里参数对象是个数组，数组里面的所有对象都是基础数据类型，将所有基础数据类型重新拷贝到新的数组中。

**合并数组**

如果想在数组内合并数组，可以这样：

```javascript
const arr1 = ['two', 'three'];
const arr2 = ['one', ...arr1, 'four', 'five'];
// 'one' 'two' 'three' 'four' 'five'
```

**扩展运算符与解构赋值结合起来，用于生成数组**

```javascript
const [first, ...rest] = [1, 2, 3, 4, 5];
first // 1
rest // [2, 3, 4, 5]
```

需要注意：如果将扩展运算符用于数组赋值，只能放在参数的最后一位，否则会报错。

```javascript
const [...rest, last] = [1, 2, 3, 4, 5]; // 报错
const [first, ...rest, last] = [1, 2, 3, 4, 5]; // 报错
```

**将字符串转为真正的数组**

```javascript
[...'hello'] // ['h', 'e', 'l', 'l', 'o']
```

**任何 Iterator 接口的对象，都可以用扩展运算符转为真正的数组**

比较常见的应用是可以将某些数据结构转为数组：

```javascript
// arguments 对象
function foo() {
	const args = [...arguments];
}
```

**使用`Math`函数获取数组中特定的值**

```javascript
const numbers = [9, 4, 7, 1];
Math.min(...numbers);
Math.max(...numbers);
```



#### 对`JSON`的理解

`JSON`是一种基于文本的轻量级的数据交换格式。它可以被任何的编程语言读取和作为数据格式来传递。 

在项目开发中，使用`JSON`作为前后端数据交换的方式。

在前端通过将一个符合`JSON`格式的数据结构序列化为`JSON`字符串，然后将它传递到后端，后端通过`JSON`格式的字符串解析后生成对应的数据结构，以此来实现前后端数据的一个传递。

因为`JSON`的语法是基于`js`的，因此很容易将`JSON`和`js`中的对象弄混，但是应该注意的是`JSON`和`js`中的对象不是一回事，`JSON`中对象格式更加严格，比如说在`JSON`中属性值不能为函数，不能出现`NaN`这样的属性值等，因此大多数的`js`对象是不符合`JSON`对象的格式的。

在`js`中提供了两个函数来实现`js`数据结构和`JSON`格式的转换处理，`JSON.stringify`函数，通过传入一个符合`JSON`格式的数据结构，将其转换为一个`JSON`字符串。如果传入的数据结构不符合`JSON`格式，那么在序列化的时候会对这些值进行对应的特殊处理，使其符合规范。在前端向后端发送数据时，可以调用这个函数将数据对象转化为`JSON`格式的字符串。 

`JSON.parse()`函数，这个函数用来将`JSON`格式的字符串转换为一个`js`数据结构，如果传入的字符串不是标准的`JSON`格式的字符串的话，将会抛出错误。当从后端接收到`JSON`格式的字符串时，可以通过这个方法来将其解析为一个`js`数据结构，以此来进行数据的访问。



#### `JavaScript`脚本延迟加载的方式有哪些

延迟加载就是等页面加载完成之后再加载`JavaScript`文件，`js`延迟加载有助于提高页面加载速度。 

一般有以下几种方式：

`defer`属性

给`js`脚本添加`defer`属性，这个属性会让脚本的加载与文档的解析同步解析，然后在文档解析完成后再执行这个脚本文件，这样的话就能使页面的渲染不被阻塞。多个设置了`defer`属性的脚本按规范来说最后是顺序执行的，但是在一些浏览器中可能不是这样。

`async`属性

给`js`脚本添加`async`属性，这个属性会使脚本异步加载，不会阻塞页面的解析过程，但是当脚本加载完成后立即执行`js`脚本，这个时候如果文档没有解析完成的话同样会阻塞。多个`async`属性的脚本的执行顺序是不可预测的，一般不会按照代码的顺序依次执行。

动态创建`DOM`方式

动态创建`DOM`标签的方式，可以对文档的加载事件进行监听，当文档加载完成后再动态的创建`script`标签来引入`js`脚本。

使用`setTimeout`延迟方法

设置一个定时器来延迟加载`js`脚本文件。

让`JS`最后加载

将`js`脚本放在文档的底部，来使`js`脚本尽可能的在最后来加载执行。



#### 什么是`DOM`和`BOM`

`DOM`指的是文档对象模型，它指的是把文档当做一个对象，这个对象主要定义了处理网页内容的方法和接口。

`BOM`指的是浏览器对象模型，它指的是把浏览器当做一个对象来对待，这个对象主要定义了与浏览器进行交互的法和接口。`BOM`的核心是`window`，而`window`对象具有双重角色，它既是通过`js`访问浏览器窗口的一个接口，又是一个`Global`（全局）对象。这意味着在网页中定义的任何对象，变量和函数，都作为全局对象的一个属性或者方法存在。`window`对象含有`location`对象、`navigator`对象、`screen`对象等子对象，并且`DOM`的最根本的对象`document`对象也是`BOM`的`window`对象的子对象。



#### `escape` `encodeURI` `encodeURIComponent`的区别

`encodeURI`是对整个`URI`进行转义，将`URI`中的非法字符转换为合法字符，所以对于一些在`URI` 中有特殊意义的字符不会进行转义。`encodeURIComponent`是对`URI`的组成部分进行转义，所以一些特殊字符也会得到转义。

`escape`和`encodeURI`的作用相同，不过它们对于`unicode`编码为`0xff`之外字符的时候会有区别，`escape`是直接在字符的`unicode`编码前加上`%u`，而`encodeURI`首先会将字符转换为`UTF-8`的格式，再在每个字节前加上`%`。



#### 对`AJAX`的理解，实现一个`AJAX`请求

`AJAX`是`Asynchronous JavaScript and XML`的缩写，指的是通过`JavaScript`的异步通信，从服务器获取`XML`文档从中提取数据，再更新当前网页的对应部分，而不用刷新整个网页。

创建`AJAX`请求的步骤： 创建一个`XMLHttpRequest`对象。 

在这个对象上使用`open`方法创建一个`HTTP`请求，`open`方法所需要的参数是请求的方法、请求的地址、是否异步和用户的认证信息。在发起请求前，可以为这个对象添加一些信息和监听函数。比如说可以通过`setRequestHeader`方法来为请求添加头信息。还可以为这个对象添加一个状态监听函数。一个`XMLHttpRequest`对象一共有5个状态，当它的状态变化时会触发 onreadystatechange 事件，可以通过设置监听函数，来处理请求成功后的结果。当对象的`readyState`变为 4 的时候，代表服务器返回的数据接收完成，这个时候可以通过判断请求的状态，如果状态是 2xx 或者304 的话则代表返回正常。这个时候就可以通过 response 中的数据来对页面进行更新了。当对象的属性和监听函数设置完成后，最后调用`send`方法来向服务器发起请求，可以传入参数作为发送的数据体。

```javascript
// 创建一个新的XMLHttpRequest对象
var xhr = new XMLHttpRequest();

// 配置请求类型、URL以及是否异步执行
xhr.open("POST", "https://example.com/api/data", true);

// 设置请求头，例如发送JSON数据时的Content-Type
xhr.setRequestHeader("Content-Type", "application/json");

// 定义当请求完成时执行的回调函数
xhr.onreadystatechange = function () {
    // 检查请求是否完成
    if (xhr.readyState === 4 && xhr.status === 200) {
        // 处理响应内容
        console.log(xhr.responseText);
    }
};

// 准备发送的数据
var data = JSON.stringify({
    key: "value",
    anotherKey: "anotherValue"
});

// 发送请求，附带数据
xhr.send(data);
```

使用`promise`封装`AJAX`：

```javascript
function sendPostRequest(url, data) {
  // 返回一个新的Promise
  return new Promise((resolve, reject) => {
    // 创建一个新的XMLHttpRequest对象
    var xhr = new XMLHttpRequest();

    // 配置请求类型、URL以及是否异步执行
    xhr.open("POST", url, true);

    // 设置请求头，例如发送JSON数据时的Content-Type
    xhr.setRequestHeader("Content-Type", "application/json");

    // 定义当请求完成时执行的回调函数
    xhr.onreadystatechange = function () {
      // 检查请求是否完成
      if (xhr.readyState === 4) {
        // 检查响应状态码
        if (xhr.status === 200) {
          // 请求成功，解析响应内容并解决Promise
          resolve(JSON.parse(xhr.responseText));
        } else {
          // 请求失败，拒绝Promise
          reject(xhr.statusText);
        }
      }
    };

    // 处理网络错误
    xhr.onerror = function () {
      reject("Network error");
    };

    // 发送请求，附带数据
    xhr.send(JSON.stringify(data));
  });
}

// 使用封装的函数
sendPostRequest("https://example.com/api/data", { key: "value", anotherKey: "anotherValue" })
  .then(response => {
    console.log("Success:", response);
  })
  .catch(error => {
    console.error("Error:", error);
  });
```



#### 什么是尾调用，使用尾调用有什么好处

尾调用指的是函数的最后一步调用另一个函数。代码执行是基于执行栈的，所以当在一个函数里调用另一个函数时，会保留当前的执行上下文，然后再新建另外一个执行上下文加入栈中。使用尾调用的话，因为已经是函数的最后一步，所以这时可以不必再保留当前的执行上下文，从而节省了内存，这就是尾调用优化。但是ES6 的尾调用优化只在严格模式下开启，正常模式是无效的。



#### `ES6`模块与`CommonJS`模块有什么异同

`ES6 Module`和`CommonJS`模块的区别： 

`CommonJS`是对模块的浅拷⻉，`ES6 Module`是对模块的引⽤，即`ES6Module`只存只读，不能改变其值，也就是指针指向不能变，类似`const`；`import`的接⼝是`read-only`（只读状态），不能修改其变量值。即不能修改其变量的指针指向，但可以改变变量内部指针指向，可以对`commonJS`对重新赋值（改变指针指向），但是对`ES6 Module`赋值会编译报错。

`ES6 Module`和`CommonJS`模块的共同点：

`CommonJS`和`ES6 Module`都可以对引⼊的对象进⾏赋值，即对对象内部属性的值进⾏改变。



#### `for...in`和`for...of`的区别

`for…of`是`ES6`新增的遍历方式，允许遍历一个含有`iterator`接口的数据结构（数组、对象等）并且返回各项的值，和`ES3`中的`for…in`的区别如下：

`for…of`遍历获取的是对象的键值，`for…in`获取的是对象的键名；`for… in`会遍历对象的整个原型链，性能非常差不推荐使用，而`for … of`只遍历当前对象不会遍历原型链；对于数组的遍历，`for…in`会返回数组中所有可枚举的属性(包括原型链上可枚举的属性)，`for…of`只返回数组的下标对应的属性值；总结：`for...in`循环主要是为了遍历对象而生，不适用于遍历数组；`for...of`循环可以用来遍历数组、类数组对象，字符串、`Set`、`Map`以及`Generator`对象。



#### `ajax` `axios` `fetch`的区别

**`AJAX`**

`Ajax`即`AsynchronousJavascriptAndXML`（异步JavaScript和XML），是指一种创建交互式网页应用的网页开发技术。它是一种在无需重新加载整个网页的情况下，能够更新部分网页的技术。通过在后台与服务器进行少量数据交换，`Ajax`可以使网页实现异步更新。这意味着可以在不重新加载整个网页的情况下，对网页的某部分进行更新。传统的网页（不使用`Ajax`）如果需要更新内容，必须重载整个网页页面。其缺点如下： 

本身是针对`MVC`编程，不符合前端`MVVM`的浪潮

基于原生`XHR`开发，`XHR`本身的架构不清晰

不符合关注分离（`Separation of Concerns`）的原则

配置和调用方式非常混乱，而且基于事件的异步模型不友好。

**`Fetch`**

`fetch`号称是`AJAX`的替代品，是在`ES6`出现的，使用了`ES6`中的`promise`对象。`Fetch`是基于`promise`设计的。`Fetch`的代码结构比起`ajax`简单多。`fetch`不是`ajax`的进一步封装，而是原生`js`，没有使用`XMLHttpRequest`对象。

`fetch`的优点：

语法简洁，更加语义化

基于标准`Promise`实现，支持`async/await`

更加底层，提供的`API`丰富（`request` `response`）

脱离了`XHR`，是`ES`规范里新的实现方式

`fetch`的缺点：

`fetch`只对网络请求报错，对 400，500 都当做成功的请求，服务器返回 400，500 错误码时并不会`reject`，只有网络错误这些导致请求不能完成时，`fetch`才会被`reject`。

`fetch`默认不会带`cookie`， 需要添加配置项：`fetch(url,{credentials: 'include'})` `fetch`不 支 持`abort`，不支持超时控制，使用`setTimeout`及`Promise.reject`的实现的超时控制并不能阻止请求过程继续在后台运行，造成了流量的浪费

`fetch`没有办法原生监测请求的进度，而`XHR`可以

**`Axios`**

`Axios`是一种基于`Promise`封装的`HTTP`客户端，其特点如下：

浏览器端发起`XMLHttpRequests`请求

`node`端发起`http`请求

支持`Promise API`

监听请求和返回

对请求和返回进行转化

取消请求

自动转换`json`数据

客户端支持抵御`XSRF`攻击



#### `MVVM` `MVC` `MVP`的区别

`MVC` `MVP` ` MVVM`是三种常见的软件架构设计模式，主要通过分离关注点的方式来组织代码结构，优化开发效率。

在开发单页面应用时，往往一个路由页面对应了一个脚本文件，所有的页面逻辑都在一个脚本文件里。页面的渲染、数据的获取，对用户事件的响应所有的应用逻辑都混合在一起，这样在开发简单项目时，可能看不出什么问题，如果项目变得复杂，那么整个文件就会变得冗长、混乱，这样对项目开发和后期的项目维护是非常不利的。

**`MVC`**

`MVC`通过分离`Model` `View` `Controller`的方式来组织代码结构。其中`View`负责页面的显示逻辑，`Model`负责存储页面的业务数据，以及对相应数据的操作。并且`View`和`Model`应用了观察者模式，当`Model`层发生改变的时候它会通知有关`View`层更新页面。

`Controller`层是`View`层和`Model`层的纽带，它主要负责用户与应用的响应操作，当用户与页面产生交互的时候，`Controller`中的事件触发器就开始工作了，通过调用`Model`层，来完成对`Model`的修改，然后`Model`层再去通知`View`层更新。

**`MVVM`**

`MVVM`分为`Model` `View` `ViewModel`：

`Model`代表数据模型，数据和业务逻辑都在`Model`层中定义；

`View`代表`UI`视图，负责数据的展示；

`ViewModel`负责监听`Model`中数据的改变并且控制视图的更新，处理用户交互操作；

`Model`和`View`并无直接关联，而是通过`ViewModel`来进行联系的，`Model`和`ViewModel`之间有着双向数据绑定的联系。因此当`Model`中的数据改变时会触发`View`层的刷新，`View`中由于用户交互操作而改变的数据也会在`Model`中同步。

这种模式实现了`Model`和`View`的数据自动同步，因此开发者只需要专注于数据的维护操作即可，而不需要自己操作`DOM`。

**`MVP`**

`MVP`模式与`MVC`唯一不同的在于`Presenter`和`Controller`。在`MVC`模式中使用观察者模式，来实现当`Model`层数据发生变化的时候，通知`View`层的更新。这样`View`层和`Model`层耦合在一起，当项目逻辑变得复杂的时候，可能会造成代码的混乱，并且可能会对代码的复用性造成一些问题。`MVP`的模式通过使用`Presenter`来实现对`View`层和`Model`层的解耦。`MVC`中的`Controller`只知道`Model`的接口，因此它没有办法控制`View`层的更新，`MVP`模式中，`View`层的接口暴露给了`Presenter`因此可以在`Presenter`中将`Model`的变化和`View`的变化绑定在一起，以此来实现`View`和`Model`的同步更新。这样就实现了对`View`和`Model`的解耦，`Presenter`还包含了其他的响应逻辑。



#### 对原型、原型链的理解

在`JavaScript`中是使用构造函数来新建一个对象的，每一个构造函数的内部都有一个`prototype`属性，它的属性值是一个对象，这个对象包含了可以由该构造函数的所有实例共享的属性和方法。当使用构造函数新建一个对象后，在这个对象的内部将包含一个指针，这个指针指向构造函数的`prototype`属性对应的值，在`ES5`中这个指针被称为对象的原型。一般来说不应该能够获取到这个值的，但是现在浏览器中都实现了`__proto__`属性来访问这个属性，但是最好不要使用这个属性，因为它不是规范中规定的。`ES5`中新增了一个`Object.getPrototypeOf()`方法，可以通过这个方法来获取对象的原型。

当访问一个对象的属性时，如果这个对象内部不存在这个属性，那么它就会去它的原型对象里找这个属性，这个原型对象又会有自己的原型，于是就这样一直找下去，也就是原型链的概念。原型链的尽头一般来说都是`Object.prototype`所以这就是新建的对象为什么能够使用`toString()`等方法的原因。

特点：`JavaScript`对象是通过引用来传递的，创建的每个新对象实体中并没有一份属于自己的原型副本。当修改原型时，与之相关的对象也会继承这一改变。



#### 原型链的终点是什么，如何打印出原型链的终点

由于`Object`是构造函数，原型链终点`Object.prototype.__proto__`，而`Object.prototype.__proto__=== null // true`，所以，原型链的终点是`null`。原型链上的所有原型都是对象，所有的对象最终都是由`Object`构造的，而`Object.prototype`的下一级是`Object.prototype.__proto__`。

```javascript
console.log(Object.prototype.__proto__) // null
```



#### 对作用域、作用域链的理解

作用域分为全局作用域和函数作用域

**全局作用域**

最外层函数和最外层函数外面定义的变量拥有全局作用域

所有未定义直接赋值的变量自动声明为全局作用域

所有 window 对象的属性拥有全局作用域全局

作用域有很大的弊端，过多的全局作用域变量会污染全局命名空间，容易引起命名冲突。

**函数作用域**

函数作用域声明在函数内部的变零，一般只有固定的代码片段可以访问到

作用域是分层的，内层作用域可以访问外层作用域，反之不行

**块级作用域**

使用`ES6`中新增的`let`和`const`指令可以声明块级作用域，块级作用域可以在函数中创建也可以在一个代码块中的创建（由`{}`包裹的代码片段）

`let`和`const`声明的变量不会有变量提升，也不可以重复声明在循环中比较适合绑定块级作用域，这样就可以把声明的计数器变量限制在循环内部。

**作用域链**

在当前作用域中查找所需变量，但是该作用域没有这个变量，那这个变量就是自由变量。如果在自己作用域找不到该变量就去父级作用域查找，依次向上级作用域查找，直到访问到window 对象就被终止，这一层层的关系就是作用域链。

作用域链的作用是保证对执行环境有权访问的所有变量和函数的有序访问，通过作用域链，可以访问到外层环境的变量和函数。

作用域链的本质上是一个指向变量对象的指针列表。变量对象是一个包含了执行环境中所有变量和函数的对象。作用域链的前端始终都是当前执行上下文的变量对象。全局执行上下文的变量对象（也就是全局对象）始终是作用域链的最后一个对象。

当查找一个变量时，如果当前执行环境中没有找到，可以沿着作用域链向后查找。



#### 对`this`对象的理解

`this`是执行上下文中的一个属性，它指向最后一次调用这个方法的对象。在实际开发中，`this`的指向可以通过四种调用模式来判断。

第一种是函数调用模式，当一个函数不是一个对象的属性时，直接作为函数来调用时，`this`指向全局对象。

第二种是方法调用模式，如果一个函数作为一个对象的方法来调用时，`this`指向这个对象。

第三种是构造器调用模式，如果一个函数用`new`调用时，函数执行前会新创建一个对象，`this`指向这个新创建的对象。

第四种是`apply ` `call ` `bind`调用模式，这三个方法都可以显示的指定调用函数的`this`指向。其中`apply`方法接收两个参数：一个是`this`绑定的对象，一个是参数数组。`call`方法接收的参数，第一个是`this`绑定的对象，后面的其余参数是传入函数执行的参数。也就是说，在使用`call()`方法时，传递给函数的参数必须逐个列举出来。`bind`方法通过传入一个对象，返回一个`this`绑定了传入对象的新函数。这个函数的`this`指向除了使用`new`时会被改变，其他情况下都不会改变。

这四种方式，使用构造器调用模式的优先级最高，然后是`apply` `call` `bind`调用模式，然后是方法调用模式，然后是函数调用模式。



#### `call()`和`apply()`的区别

它们的作用一模一样，区别仅在于传入参数的形式的不同。

`apply`接受两个参数，第一个参数指定了函数体内`this`对象的指向，第二个参数为一个带下标的集合，这个集合可以为数组，也可以为类数组，`apply`方法把这个集合中的元素作为参数传递给被调用的函数。`call`传入的参数数量不固定，跟`apply`相同的是，第一个参数也是代表函数体内的`this`指向，从第二个参数开始往后，每个参数被依次传入函数。



#### 异步编程的实现方式

`JavaScript`中的异步机制可以分为以下几种：

回调函数的方式，使用回调函数的方式有一个缺点是，多个回调函数嵌套的时候会造成回调函数地狱，上下两层的回调函数间的代码耦合度太高，不利于代码的可维护。

`Promise`的方式，使用`Promise`的方式可以将嵌套的回调函数作为链式调用。但是使用这种方法，有时会造成多个`then`的链式调用，可能会造成代码的语义不够明确。

`generator`的方式，它可以在函数的执行过程中，将函数的执行权转移出去，在函数外部还可以将执行权转移回来。当遇到异步函数执行的时候，将函数执行权转移出去，当异步函数执行完毕时再将执行权给转移回来。因此在`generator`内部对于异步操作的方式，可以以同步的顺序来书写。使用这种方式需要考虑的问题是何时将函数的控制权转移回来，因此需要有一个自动执行`generator`的机制，比如说 `co`模块等方式来实现`generato`的自动执行。

`async`函数 的方式，`async`函数是`generator`和`promise`实现的一个自动执行的语法糖，它内部自带执行器，当函数内部执行到一个`await`语句的时候，如果语句返回一个`promise`对象，那么函数将会等待`promise`对象的状态变为`resolve`后再继续向下执行。因此可以将异步逻辑，转化为同步的顺序来书写，并且这个函数可以自动执行。



#### 对`Promise`的理解

`Promise`是异步编程的一种解决方案，它是一个对象，可以获取异步操作的消息，他的出现大大改善了异步编程的困境，避免了地狱回调，它比传统的解决方案回调函数和事件更合理和更强大。

所谓`Promise`，简单说就是一个容器，里面保存着某个未来才会结束的事件（通常是一个异步操作）的结果。从语法上说，`Promise`是一个对象，从它可以获取异步操作的消息。`Promise`提供统一的`API`，各种异步操作都可以用同样的方法进行处理。

`Promise`的实例有三个状态：

- `Pending`（进行中）
- `Resolved`（已完成）
- `Rejected`（已拒绝）

当把一件事情交给`promise`时，它的状态就是`Pending`，任务完成了状态就变成了`Resolved`、没有完成失败了就变成了`Rejected`。

`Promise`的实例有两个过程：

`pending` ->  `fulfilled` : `Resolved`（已完成）

`pending` -> `rejected` ：`Rejected`（已拒绝）

注意：一旦从进行状态变成为其他状态就永远不能更改状态了。

`Promise`的特点：

对象的状态不受外界影响。`promise`对象代表一个异步操作，有三种状态，`pending`（进行中）`fulfilled`（已成功） `rejected`（已失败）。只有异步操作的结果，可以决定当前是哪一种状态，任何其他操作都无法改变这个状态，这也是`promise`这个名字的由来——“承诺”。

一旦状态改变就不会再变，任何时候都可以得到这个结果。`promise`对象的状态改变，只有两种可能：从`pending`变为`fulfilled`，从`pending`变为`rejected`。这时就称为`resolved`（已定型）。如果改变已经发生了，你再对`promise`对象添加回调函数，也会立即得到这个结果。这与事件（`event`）完全不同，事件的特点是：如果你错过了它，再去监听是得不到结果的。

`Promise`的缺点：

无法取消`Promise`，一旦新建它就会立即执行，无法中途取消。如果不设置回调函数，`Promise`内部抛出的错误，不会反应到外部。当处于`pending`状态时，无法得知目前进展到哪一个阶段（刚刚开始还是即将完成）。

总结：

`Promise`对象是异步编程的一种解决方案，最早由社区提出。`Promise`是一个构造函数，接收一个函数作为参数，返回一个`Promise`实例。一个`Promise`实例有三种状态，分别是`pending` `resolved` `rejected`，分别代表了进行中、已成功和已失败。实例的状态只能由`pending`转变`resolved`或者`rejected`状态，并且状态一经改变，就凝固了，无法再被改变了。

状态的改变是通过`resolve()`和`reject()`函数来实现的，可以在异步操作结束后调用这两个函数改变`Promise`实例的状态，它的原型上定义了一个`then`方法，使用这个`then`方法可以为两个状态的改变注册回调函数。这个回调函数属于微任务，会在本轮事件循环的末尾执行。

注意：在构造`Promise`的时候，构造函数内部的代码是立即执行的



#### `Promise`解决了什么问题

在工作中经常会碰到这样一个需求，比如我使用`ajax`发一个A请求后，成功后拿到数据，需要把数据传给`B`请求；那么需要如下编写代码：

```javascript
let fs = require('fs');
fs.readFile('./a.txt', 'utf-8', function(err, data1) {
    fs.readFile(data1, 'utf-8', function(err, data2) {
        fs.readFile(data2, 'utf-8', function(err, data3) {
            console.log(data)
        })
    })
})
```

上面的代码有如下缺点： 后一个请求需要依赖于前一个请求成功后，将数据往下传递，会导致多个`ajax`请求嵌套的情况，代码不够直观。

如果前后两个请求不需要传递参数的情况下，那么后一个请求也需要前一个请求成功后再执行下一步操作，这种情况下，那么也需要如上编写代码，导致代码不够直观。

Promise 出现之后，代码变成这样：

```javascript
let fs = require('fs');
function read(url) {
    return new Promise((resolve, reject) => {
        fs.readFile('./a.txt', 'utf-8', function(err, data) {
            err && reject(err);
            resolve(data);
        }
    })
}
                       
read('./a.txt').then((data) => {
  return read(data);     
}).then((data) => {
  return read(data);
}).then((data) => {
  console.log(data);
})
```

这样代码看起了就简洁了很多，解决了地狱回调的问题。



#### 对`async/await`的理解

`async/await`其实是`Generator`的语法糖，它能实现的效果都能用`then`链来实现，它是为优化`then`链而开发出来的。从字面上来看，`async`是“异步”的简写，`await`则为等待，所以很好理解`async`用于申明一个`function`是异步的，而`await`用于等待一个异步方法执行完成。当然语法上强制规定`await`只能出现在`asnyc`函数中。

所以，`async`函数返回的是一个`Promise`对象。`async`函数（包含函数语句、函数表达式、`Lambda`表达式）会返回一个`Promise`对象，如果在函数中`return`一个直接量，`async`会把这个直接量通过`Promise.resolve()`封装成`Promise`对象。

`async`函数返回的是一个`Promise`对象，所以在最外层不能用`await`获取其返回值的情况下，当然应该用原来的方式：`then()`链来处理这个`Promise`对象。

那如果`async`函数没有返回值，又该如何？很容易想到，它会返回`Promise.resolve(undefined)`。

联想一下`Promise`的特点——无等待，所以在没有`await`的情况下执行`async`函数，它会立即执行，返回一个`Promise`对象，并且，绝不会阻塞后面的语句。这和普通返回`Promise`对象的函数并无二致。

注意：`Promise.resolve(x)`可以看作是`new Promise(resolve=>resolve(x))`的简写，可以用于快速封装字面量对象或其他对象，将其封装成`Promise`实例。



#### `async/await`的优势

单一的`Promise`链并不能发现`async/await`的优势，但是，如果需要处理由多个`Promise`组成的`then`链的时候，优势就能体现出来了（很有意思，`Promise`通过`then`链来解决多层回调的问题，现在又用`async/await`来进一步优化它）。

假设一个业务，分多个步骤完成，每个步骤都是异步的，而且依赖于上一个步骤的结果。仍然用`setTimeout`来模拟异步操作：

```javascript
function takeLongTime(n) {
    return Promise((resolve) => {
        setTimeout(() => resolve(n + 200), n);
    })
}
function step1(n) {
    console.log(`step1 with ${n}`);
    return takeLongTime(n);
}
function step2(n) {
    console.log(`step2 with ${n}`);
    return takeLongTime(n);
}
function step3(n) {
    console.log(`step3 with ${n}`);
    return takeLongTime(n);
}
```

现在用`Promise`方式来实现这三个步骤的处理：

```javascript
function doIt() {
    console.time('doIt');
    const time1 = 300;
    step1(time1)
        .then((time2) => step2(time2))
    	.then((time3) => step2(time3))
    	.then((result) => {
        	console.log(`result is ${result}`);
    	})
}
```

如果用`async/await`来实现呢，会是这样：

```javascript
async function doIt() {
    console.time('doIt');
    const time1 = 300;
    const time2 = await step1(time1);
    const time3 = await step1(time2);
    const result = await step1(time3);
    console.log(`result is ${result}`);
}
doIt();
```

结果和之前的`Promise`实现是一样的，但是这个代码看起来是不是清晰得多，几乎跟同步代码一样。



#### `async/await`对比`Promise`的优势

代码读起来更加同步，`Promise`虽然摆脱了回调地狱，但是`then`的链式调⽤也会带来额外的阅读负担。

`Promise`传递中间值⾮常麻烦，⽽`async/await`⼏乎是同步的写法，⾮常优雅。

错误处理友好，`async/await`可以⽤成熟的`try/catch`，`Promise`的错误捕获⾮常冗余。

调试友好，`Promise`的调试很差，由于没有代码块，你不能在⼀个返回表达式的箭头函数中设置断点，如果你在⼀个`.then`代码块中使⽤调试器的步进(`step-over`)功能，调试器并不会进⼊后续的`.then`代码块，因为调试器只能跟踪同步代码的每⼀步。



#### `React`的事件和普通的`HTML`事件有什么不同

区别：

对于事件名称命名方式，原生事件为全小写，`react`事件采用小驼峰；

对于事件函数处理语法，原生事件为字符串，`react`事件为函数；

`react`事件不能采用`return false`的方式来阻止浏览器的默认行为，而必须要地明确地调用`preventDefault()`来阻止默认行为。合成事件是`react`模拟原生`DOM`事件所有能力的一个事件对象，其优点如下：

兼容所有浏览器，更好的跨平台；

将事件统一存放在一个数组，避免频繁的新增与删除（垃圾回收）。

方便`react`统一管理和事务机制。

事件的执行顺序为原生事件先执行，合成事件后执行，合成事件会冒泡绑定到`document`上，所以尽量避免原生事件与合成事件混用，如果原生事件阻止冒泡，可能会导致合成事件不执行，因为需要冒泡到`document`上合成事件才会执行。



#### `React`如何判断什么时候重新渲染组件

组件状态的改变可以因为`props`的改变，或者直接通过`setState`方法改变。组件获得新的状态，然后`React`决定是否应该重新渲染组件。只要组件的`state`发生变化，`React`就会对组件进行重新渲染。这是因为`React`中的`shouldComponentUpdate`方法默认返回`true`，这就是导致每次更新都重新渲染的原因。

当`React`将要渲染组件时会执行`shouldComponentUpdate`方法来看它是否返回`true`（组件应该更新，也就是重新渲染）。所以需要重写`shouldComponentUpdate`方法让它根据情况返回`true`或者`false`来告诉`React`什么时候重新渲染什么时候跳过重新渲染。



#### `React`中的`props`为什么是只读的

`this.props`是组件之间沟通的一个接口，原则上来讲，它只能从父组件流向子组件。`React`具有浓重的函数式编程的思想。

提到函数式编程就要提一个概念：纯函数。它有几个特点：

给定相同的输入，总是返回相同的输出。

过程没有副作用。

不依赖外部状态。

`this.props`就是汲取了纯函数的思想。`props`的不可以变性就保证的相同的输入，页面显示的内容是一样的，并且不会产生副作用。



#### `React 16.X`中`props`改变后在哪个生命周期中处理

在`getDerivedStateFromProps`中进行处理。

这个生命周期函数是为了替代`componentWillReceiveProps`存在的，所以在需要使用`componentWillReceiveProps`时，就可以考虑使用`getDerivedStateFromProps`来进行替代。

两者的参数是不相同的，而`getDerivedStateFromProps`是一个静态函数，也就是这个函数不能通过`this`访问到`class`的属性，也并不推荐直接访问属性。而是应该通过参数提供的`nextProps`以及`prevState`来进行判断，根据新传入的`props`来映射到`state`。

需要注意的是，如果`props`传入的内容不需要影响到你的`state`，那么就需要返回一个`null`。



#### `Redux`中间件是怎么拿到`store`和`action`，然后怎么处理

`redux`中间件本质就是一个函数柯里化。`redux applyMiddlewareApi`源码中每个`middleware`接受 2 个参数，`Store`的`getState`函数和`dispatch`函数，分别获得`store`和`action`，最终返回一个函数。该函数会被传入`next`的下一个`middleware`的`dispatch`方法，并返回一个接收`action`的新函数，这个函数可以直接调用`next`（`action`），或者在其他需要的时刻调用，甚至根本不去调用它。调用链中最后一个`middleware`会接受真实的`store`的`dispatch`方法作为`next`参数，并借此结束调用链。所以，`middleware`的函数签名是`（{ getState，dispatch })=> next => action`。



#### 协商缓存和强缓存的区别

协商缓存和强缓存都是HTTP缓存机制的一部分，用于减少网络带宽的使用，提高网页加载速度，减轻服务器压力。它们在处理缓存的方式上有本质的区别。

**强缓存**

强缓存不会向服务器发送请求，直接从缓存中读取资源。浏览器根据本地缓存资源的HTTP头信息来判断是否需要向服务器请求。强缓存主要通过以下两个HTTP响应头实现：

`Expires`：HTTP/1.0的产物，值为资源到期的时间，是一个绝对时间。如果客户端的时间与服务器时间不同步，可能会导致缓存失效。

`Cache-Control`：HTTP/1.1引入，优先级高于`Expires`。它可以通过设置`max-age`（资源最大有效时间）来控制资源的缓存时间。

**协商缓存**

当强缓存失效时，浏览器会向服务器发送请求，由服务器决定是否使用缓存。这一过程称为协商缓存。协商缓存通过以下两对HTTP头实现：

`Last-Modified` / `If-Modified-Since`：服务器响应请求时，通过`Last-Modified`头告诉浏览器资源的最后修改时间。浏览器再次请求该资源时，通过`If-Modified-Since`头将上次获取的`Last-Modified`值发送给服务器。服务器比较这个时间与资源当前的最后修改时间，如果时间相同，则返回304状态码，告诉浏览器使用本地缓存。

`ETag` / `If-None-Match`：`ETag`是资源的唯一标识，更精确地控制缓存。服务器通过`ETag`响应头返回资源的标识，浏览器将此标识与资源一起缓存。再次请求资源时，浏览器通过`If-None-Match`头将`ETag`发送给服务器。如果服务器上的资源未改变（`ETag`相同），服务器返回304状态码，否则返回新的资源和200状态码。

**区别**

**请求发送**：强缓存不会发送请求到服务器，直接从缓存中读取资源；协商缓存会发送请求到服务器，由服务器决定是否使用缓存。

**控制头**：强缓存通过`Expires`和`Cache-Control`控制；协商缓存通过`Last-Modified` / `If-Modified-Since`和`ETag` / `If-None-Match`控制。

**优先级**：当`Cache-Control`和`Expires`同时存在时，`Cache-Control`的优先级更高。对于协商缓存，`ETag` / `If-None-Match`的优先级通常高于`Last-Modified` / `If-Modified-Since`。



#### 浏览器的渲染过程

浏览器渲染主要有以下步骤：

首先解析收到的文档，根据文档定义构建一棵`DOM`树，`DOM`树是由`DOM`元素及属性节点组成的。

然后对`CSS`进行解析，生成`CSS`规则树。

根据`DOM`树和`CSS`规则树构建渲染树。渲染树的节点被称为渲染对象，渲染对象是一个包含有颜色和大小等属性的矩形，渲染对象和`DOM`元素相对应，但这种对应关系不是一对一的，不可见的`DOM`元素不会被插入渲染树。还有一些`DOM`元素对应几个可见对象，它们一般是一些具有复杂结构的元素，无法用一个矩形来描述。

当渲染对象被创建并添加到树中，它们并没有位置和大小，所以当浏览器生成渲染树以后，就会根据渲染树来进行布局（也可以叫做回流）。这一阶段浏览器要做的事情是要弄清楚各个节点在页面中的确切位置和大小。通常这一行为也被称为“自动重排”。

布局阶段结束后是绘制阶段，遍历渲染树并调用渲染对象的paint方法将它们的内容显示在屏幕上，绘制使用`UI`基础组件。

这个过程是逐步完成的，为了更好的用户体验，渲染引擎将会尽可能早的将内容呈现到屏幕上，并不会等到所有的`html`都解析完成之后再去构建和布局`render`树。它是解析完一部分内容就显示一部分内容，同时，可能还在通过网络下载其余内容。



#### 渲染过程中遇到`JS`文件如何处理

`JavaScript`的加载、解析与执行会阻塞文档的解析，也就是说，在构建`DOM`时，HTML 解析器若遇到了`JavaScript`，那么它会暂停文档的解析，将控制权移交给`JavaScript`引擎，等`JavaScript`引擎运行完毕，浏览器再从中断的地方恢复继续解析文档。也就是说，如果想要首屏渲染的越快，就越不应该在首屏就加载`JS`文件，这也是都建议将`script `标签放在`body`标签底部的原因。当然在当下，并不是说`script`标签必须放在底部，因为你可以给`script`标签添加`defer`或者`async`属性。



#### 事件是什么，事件模型

事件是用户操作网页时发生的交互动作，比如`click/move`，事件除了用户触发的动作外，还可以是文档加载，窗口滚动和大小调整。事件被封装成一个`event`对象，包含了该事件发生时的所有相关信息（`event`的属性）以及可以对事件进行的操作（`event`的方法）。事件是用户操作网页时发生的交互动作或者网页本身的一些操作，现代浏览器一共有三种事件模型：

`DOM`0 级事件模型，这种模型不会传播，所以没有事件流的概念，但是现在有的浏览器支持以冒泡的方式实现，它可以在网页中直接定义监听函数，也可以通过`js`属性来指定监听函数。所有浏览器都兼容这种方式。直接在`dom`对象上注册事件名称，就是`DOM`0 写法。

`IE`事件模型，在该事件模型中，一次事件共有两个过程，事件处理阶段和事件冒泡阶段。事件处理阶段会首先执行目标元素绑定的监听事件。然后是事件冒泡阶段，冒泡指的是事件从目标元素冒泡到`document`，依次检查经过的节点是否绑定了事件监听函数，如果有则执行。这种模型通过`attachEvent`来添加监听函数，可以添加多个监听函数，会按顺序依次执行。

`DOM`2 级事件模型，在该事件模型中，一次事件共有三个过程，第一个过程是事件捕获阶段。捕获指的是事件从`document`一直向下传播到目标元素，依次检查经过的节点是否绑定了事件监听函数，如果有则执行。后面两个阶段和`IE`事件模型的两个阶段相同。这种事件模型，事件绑定的函数是`addEventListener`，其中第三个参数可以指定事件是否在捕获阶段执行。



#### 对事件循环的理解

因为`js`是单线程运行的，在代码执行时，通过将不同函数的执行上下文压入执行栈中来保证代码的有序执行。在执行同步代码时，如果遇到异步事件，`js`引擎并不会一直等待其返回结果，而是会将这个事件挂起，继续执行执行栈中的其他任务。当异步事件执行完毕后，再将异步事件对应的回调加入到一个任务队列中等待执行。任务队列可以分为宏任务队列和微任务队列，当当前执行栈中的事件执行完毕后，`js`引擎首先会判断微任务队列中是否有任务可以执行，如果有就将微任务队首的事件压入栈中执行。当微任务队列中的任务都执行完成后再去执行宏任务队列中的任务。

`Event Loop`执行顺序如下所示：

首先执行同步代码，这属于宏任务。

当执行完所有同步代码后，执行栈为空，查询是否有异步代码需要执行。

执行所有微任务。

当执行完所有微任务后，如有必要会渲染页面。

然后开始下一轮`Event Loop`，执行宏任务中的异步代码。



`GET`和`POST`的请求的区别

`Post`和`Get`是`HTTP`请求的两种方法，其区别如下：

应用场景：`GET`请求是一个幂等的请求，一般`Get`请求用于对服务器资源不会产生影响的场景，比如说请求一个网页的资源。而`Post`不是一个幂等的请求，一般用于对服务器资源会产生影响的情景，比如注册用户这一类的操作。

是否缓存：因为两者应用场景不同，浏览器一般会对`Get`请求缓存，但很少对`Post`请求缓存。

发送的报文格式：`Get`请求的报文中实体部分为空，`Post`请求的报文中实体部分一般为向服务器发送的数据。

安全性：`Get`请求可以将请求的参数放入`url`中向服务器发送，这样的做法相对于`Post`请求来说是不太安全的，因为请求的`url`会被保留在历史记录中。

请求长度：浏览器由于对`url`长度的限制，所以会影响`get`请求发送数据时的长度。这个限制是浏览器规定的，并不是`RFC`规定的。

参数类型：`post`的参数传递支持更多的数据类型。



#### `POST`和`PUT`请求的区别

`PUT`请求是向服务器端发送数据，从而修改数据的内容，但是不会增加数据的种类等，也就是说无论进行多少次`PUT`操作，其结果并没有不同。（可以理解为时更新数据）

`POST`请求是向服务器端发送数据，该请求会改变数据的种类等资源，它会创建新的内容。（可以理解为是创建数据）



#### 常见的`HTTP`请求头和响应头

`HTTP Request Header`常见的请求头：

`Accept`:浏览器能够处理的内容类型

`Accept-Charset`:浏览器能够显示的字符集

`Accept-Encoding`：浏览器能够处理的压缩编码

`Accept-Language`：浏览器当前设置的语言

`Connection`：浏览器与服务器之间连接的类型

`Cookie`：当前页面设置的任何`Cookie`

`Host`：发出请求的页面所在的域

`Referer`：发出请求的页面的`URL`

`User-Agent`：浏览器的用户代理字符串

`HTTP Responses Header`常见的响应头：

`Date`：表示消息发送的时间，时间的描述格式由`rfc822`定义

`server`:服务器名称

`Connection`：浏览器与服务器之间连接的类型

`Cache-Control`：控制`HTTP`缓存

`ontent-type`:表示后面的文档属于什么`MIME`类型

常见的`Content-Type`属性值有以下四种：

`application/x-www-form-urlencoded`：浏览器的原生`form`表单 ， 如 果不设置`enctype`属 性，那么最终就会以`application/x-www-form-urlencoded`方式提交数据。该种方式提交的数据放在`body`里面，数据按照`key1=val1&key2=val2`的方式进行编码，`key`和`val`都进行了`URL`转码。

`multipart/form-data`：该种方式也是一个常见的`POST`提交方式，通常表单上传文件时使用该种方式。

`application/json`：服务器消息主体是序列化后的`JSON`字符串。

`text/xml`：该种方式主要用来提交`XML`格式的数据。



#### 常见的`HTTP`请求方法

`GET`: 向服务器获取数据；

`POST`：将实体提交到指定的资源，通常会造成服务器资源的修改；

`PUT`：上传文件，更新数据；

`DELETE`：删除服务器上的对象；

`HEAD`：获取报文首部，与`GET`相比，不返回报文主体部分；

`OPTIONS`：询问支持的请求方法，用来跨域请求；

`CONNECT`：要求在与代理服务器通信时建立隧道，使用隧道进行`TCP`通信；

`TRACE`: 回显服务器收到的请求，主要⽤于测试或诊断。



#### `HTTP 1.1`和`HTTP 2.0`的区别

二进制协议：`HTTP/2`是一个二进制协议。在`HTTP/1.1`版中，报文的头信息必须是文本（`ASCII`编码），数据体可以是文本，也可以是二进制。`HTTP/2`则是一个彻底的二进制协议，头信息和数据体都是二进制，并且统称为"帧"，可以分为头信息帧和数据帧。帧的概念是它实现多路复用的基础。

多路复用：`HTTP/2`实现了多路复用，`HTTP/2`仍然复用`TCP`连接，但是在一个连接里，客户端和服务器都可以同时发送多个请求或回应，而且不用按照顺序一一发送，这样就避免了"队头堵塞"的问题。

数据流：`HTTP/2`使用了数据流的概念，因为`HTTP/2`的数据包是不按顺序发送的，同一个连接里面连续的数据包，可能属于不同的请求。因此，必须要对数据包做标记，指出它属于哪个请求。`HTTP/2`将每个请求或回应的所有数据包，称为一个数据流。每个数据流都有一个独一无二的编号。数据包发送时，都必须标记数据流`ID`，用来区分它属于哪个数据流。

头信息压缩：`HTTP/2`实现了头信息压缩，由于`HTTP 1.1`协议不带状态，每次请求都必须附上所有信息。所以，请求的很多字段都是重复的，比如`Cookie`和`User Agent`，一模一样的内容，每次请求都必须附带，这会浪费很多带宽，也影响速度。`HTTP/2`对这一点做了优化，引入了头信息压缩机制。一方面，头信息使用`gzip`或`compress`压缩后再发送；另一方面，客户端和服务器同时维护一张头信息表，所有字段都会存入这个表，生成一个索引号，以后就不发送同样字段了，只发送索引号，这样就能提高速度了。

服务器推送：`HTTP/2`允许服务器未经请求，主动向客户端发送资源，这叫做服务器推送。使用服务器推送提前给客户端推送必要的资源，这样就可以相对减少一些延迟时间。这里需要注意的是`http2`下服务器主动推送的是静态资源，和`WebSocket`以及使用`SSE`等方式向客户端发送即时数据的推送是不同的。



#### `HTTP`和`HTTPS`协议的区别

`HTTP`和`HTTPS`协议的主要区别如下：

`HTTPS`协议需要`CA`证书，费用较高；而`HTTP`协议不需要；

`HTTP`协议是超文本传输协议，信息是明文传输的，`HTTPS`则是具有安全性的`SSL`加密传输协议；

使用不同的连接方式，端口也不同，`HTTP`协议端口是`80`，`HTTPS`协议端口是`443`；

`HTTP`协议连接很简单，是无状态的；`HTTPS`协议是有`SSL`和`HTTP`协议构建的可进行加密传输、身份认证的网络协议，比`HTTP`更加安全。



说一下`HTTP 3.0`

`HTTP/3`基于`UDP`协议实现了类似于`TCP`的多路复用数据流、传输可靠性等功能，这套功能被称为`QUIC`协议。

流量控制、传输可靠性功能：`QUIC`在`UDP`的基础上增加了一层来保证数据传输可靠性，它提供了数据包重传、拥塞控制、以及其他一些`TCP`中的特性。

集成`TLS`加密功能：目前`QUIC`使用`TLS1.3`，减少了握手所花费的`RTT`数。

多路复用：同一物理连接上可以有多个独立的逻辑数据流，实现了数据流的单独传输，解决了`TCP`的队头阻塞问题。

快速握手：由于基于`UDP`，可以实现使用0 ~ 1 个`RTT`来建立连接。



#### 什么是`HTTPS`协议

超文本传输安全协议（`Hypertext Transfer Protocol Secure`，简称：`HTTPS`）是一种通过计算机网络进行安全通信的传输协议。`HTTPS`经由`HTTP`进行通信，利用`SSL/TLS`来加密数据包。`HTTPS`的主要目的是提供对网站服务器的身份认证，保护交换数据的隐私与完整性。

`HTTP`协议采用明文传输信息，存在信息窃听、信息篡改和信息劫持的风险，而协议`TLS/SSL`具有身份验证、信息加密和完整性校验的功能，可以避免此类问题发生。

安全层的主要职责就是对发起的`HTTP`请求的数据进行加密操作和对接收到的`HTTP`的内容进行解密操作。



#### `HTTPS`通信（握手）过程

`HTTPS`的通信过程如下：

客户端向服务器发起请求，请求中包含使用的协议版本号、生成的一个随机数、以及客户端支持的加密方法。

服务器端接收到请求后，确认双方使用的加密方法、并给出服务器的证书、以及一个服务器生成的随机数。

客户端确认服务器证书有效后，生成一个新的随机数，并使用数字证书中的公钥，加密这个随机数，然后发给服务器。并且还会提供一个前面所有内容的`hash`的值，用来供服务器检验。

服务器使用自己的私钥，来解密客户端发送过来的随机数。并提供前面所有内容的`hash`值来供客户端检验。

客户端和服务器端根据约定的加密方法使用前面的三个随机数，生成对话秘钥，以后的对话过程都使用这个秘钥来加密信息。



#### `DNS`完整的查询过程

`DNS`服务器解析域名的过程：

首先会在浏览器的缓存中查找对应的`IP`地址，如果查找到直接返回，若找不到继续下一步

将请求发送给本地`DNS`服务器，在本地域名服务器缓存中查询，如果查找到，就直接将查找结果返回，若找不到继续下一步

本地`DNS`服务器向根域名服务器发送请求，根域名服务器会返回一个所查询域的顶级域名服务器地址

本地`DNS`服务器向顶级域名服务器发送请求，接受请求的服务器查询自己的缓存，如果有记录，就返回查询结果，如果没有就返回相关的下一级的权威域名服务器的地址

本地`DNS`服务器向权威域名服务器发送请求，域名服务器返回对应的结果

本地`DNS`服务器将返回结果保存在缓存中，便于下次使用本地`DNS`服务器将返回结果返回给浏览器

比如要查询`www.baidu.com`的`IP`地址，首先会在浏览器的缓存中查找是否有该域名的缓存，如果不存在就将请求发送到本地的`DNS`服务器中，本地`DNS`服务器会判断是否存在该域名的缓存，如果不存在，则向根域名服务器发送一个请求，根域名服务器返回负责`.com`的顶级域名服务器的`IP`地址的列表。然后本地`DNS`服务器再向其中一个负责`.com`的顶级域名服务器发送一个请求，负责`.com`的顶级域名服务器返回负责`.baidu`的权威域名服务器的`IP`地址列表。然后本地`DNS`服务器再向其中一个权威域名服务器发送一个请求，最后权威域名服务器返回一个对应的主机名的`IP`地址列表。



#### `OSI`七层模型

`OSI`（`Open Systems Interconnection`）模型是一个参考模型，用于描述和标准化网络中不同计算机系统间通信的功能，以实现开放式系统间的通信。`OSI`模型将网络通信的工作分为七个抽象层，每一层都有其特定的功能和协议。从上到下，这七层分别是：

1. **应用层（`Application Layer`）**：为应用软件提供网络服务，如`HTTP` `FTP` `SMTP`等协议，处理与用户直接交互的部分。
2. **表示层（`Presentation Layer`）**：确保一个系统的应用层发送的信息可以被另一个系统的应用层读取，即数据的表示、安全、压缩。例如，加密、字符编码、数据压缩等。
3. **会话层（`Session Layer`）**：建立、管理和终止会话。会话层在两个应用进程之间建立、管理和终止会话（数据交换的逻辑连接）。
4. **传输层（`Transport Layer`）**：提供可靠的数据传输服务，确保数据的完整性。主要协议有`TCP`（传输控制协议）和`UDP`（用户数据报协议）。
5. **网络层（`Network Layer`）**：负责数据包从源到宿的传递和路由选择，例如`IP`协议。
6. **数据链路层（`Data Link Layer`）**：在物理介质上提供错误检测和纠正，以及适当的流量控制。例如，以太网（`Ethernet`）、`PPP`（点对点协议）等。
7. **物理层（`Physical Layer`）**：处理与电气或物理介质的接口细节，包括比特流的传输，如电缆、光纤、无线电频率等。

每一层都依赖于其下一层提供的服务，并为其上一层提供服务。数据在发送时，从应用层向下通过每一层，每一层都会添加该层相关的控制信息（如头部信息），直到物理层，然后在接收端，数据从物理层向上通过每一层，每一层都会去除相应的控制信息，直到应用层。这个过程被称为封装和解封装。



#### `TCP`的三次握手和四次挥手

**三次握手**

三次握手（`Three-way Handshake`）其实就是指建立一个`TCP`连接时，需要客户端和服务器总共发送 3 个包。进行三次握手的主要作用就是为了确认双方的接收能力和发送能力是否正常、指定自己的初始化序列号为后面的可靠性传送做准备。实质上其实就是连接服务器指定端口，建立`TCP`连接，并同步连接双方的序列号和确认号，交换`TCP`窗口大小信息。

刚开始客户端处于`Closed`的状态，服务端处于`Listen`状态。

第一次握手：客户端给服务端发一个`SYN`报文，并指明客户端的初始化序列号`ISN`，此时客户端处于`SYN_SEND`状态。

首部的同步位`SYN=1`，初始序号`seq=x`，`SYN=1`的报文段不能携带数据，但要消耗掉一个序号。

第二次握手：服务器收到客户端的`SYN`报文之后，会以自己的`SYN`报文作为应答，并且也是指定了自己的初始化序列号`ISN`。同时会把客户端的`ISN + 1`作为`ACK`的值，表示自己已经收到了客户端的`SYN`，此时服务器处于`SYN_REVD`的状态。

在确认报文段中`SYN=1`，`ACK=1`，确认号`ack=x+1`，初始序号`seq=y`第三次握手：客户端收到`SYN`报文之后，会发送一个`ACK`报文，当然，也是一样把服务器的`ISN + 1`作为`ACK`的值，表示已经收到了服务端的`SYN`报文，此时客户端处于`ESTABLISHED` 状态。服务器收到`ACK`报文之后，也处于`ESTABLISHED`状态，此时，双方已建立起了连接。

确认报文段`ACK=1`，确认号`ack=y+1`，序号`seq=x+1`（初始为`seq=x`，第二个报文段所以要+1），`ACK`报文段可以携带数据，不携带数据则不消耗序号。

简单来说就是以下三步：

第一次握手：客户端向服务端发送连接请求报文段。该报文段中包含自身的数据通讯初始序号。请求发送后，客户端便进入`SYN-SENT`状态。

第二次握手：服务端收到连接请求报文段后，如果同意连接，则会发送一个应答，该应答中也会包含自身的数据通讯初始序号，发送完成后便进入`SYN-RECEIVED`状态。

第三次握手：当客户端收到连接同意的应答后，还要向服务端发送一个确认报文。客户端发完这个报文段后便进入`ESTABLISHED`状态，服务端收到这个应答后也进入`ESTABLISHED`状态，此时连接建立成功。

`TCP`三次握手的建立连接的过程就是相互确认初始序号的过程，告诉对方，什么样序号的报文段能够被正确接收。第三次握手的作用是客户端对服务器端的初始序号的确认。如果只使用两次握手，那么服务器就没有办法知道自己的序号是否 已被确认。同时这样也是为了防止失效的请求报文段被服务器接收，而出现错误的情况。

**四次挥手**

刚开始双方都处于`ESTABLISHED`状态，假如是客户端先发起关闭请求。四次挥手的过程如下：

第一次挥手： 客户端会发送一个`FIN`报文，报文中会指定一个序列号。此时客户端处于`FIN_WAIT1`状态。

即发出连接释放报文段（`FIN=1`，序号`seq=u`），并停止再发送数据，主动关闭`TCP`连接，进入`FIN_WAIT1`（终止等待1）状态，等待服务端的确认。

第二次挥手：服务端收到`FIN`之后，会发送`ACK`报文，且把客户端的序列号值 +1 作为`ACK`报文的序列号值，表明已经收到客户端的报文了，此时服务端处于`CLOSE_WAIT`状态。

即服务端收到连接释放报文段后即发出确认报文段（`ACK=1`，确认号`ack=u+1`，序号`seq=v`），服务端进入`CLOSE_WAIT`（关闭等待）状态，此时的`TCP`处于半关闭状态，客户端到服务端的连接释放。客户端收到服务端的确认后，进入`FIN_WAIT2`（终止等待2）状态，等待服务端发出的连接释放报文段。

第三次挥手：如果服务端也想断开连接了，和客户端的第一次挥手一样，发给`FIN`报文，且指定一个序列号。此时服务端处于`LAST_ACK`的状态。

即服务端没有要向客户端发出的数据，服务端发出连接释放报文段（`FIN=1`，`ACK=1`，序号`seq=w`，确认号`ack=u+1`），服务端进入`LAST_ACK`（最后确认）状态，等待客户端的确认。

第四次挥手：客户端收到`FIN`之后，一样发送一个`ACK`报文作为应答，且把服务端的序列号值 +1 作为自己`ACK`报文的序列号值，此时客户端处于`TIME_WAIT`状态。需要过一阵子以确保服务端收到自己的 ACK 报文之后才会进入`CLOSED`状态，服务端收到`ACK`报文之后，就处于关闭连接了，处于`CLOSED`状态。

即客户端收到服务端的连接释放报文段后，对此发出确认报文段（`ACK=1`，`seq=u+1`，`ack=w+1`），客户端进入`TIME_WAIT`（时间等待）状态。此时`TCP`未释放掉，需要经过时间等待计时器设置的时间`2MSL`后，客户端才进入`CLOSED`状态。

简单来说就是以下四步：

第一次挥手：若客户端认为数据发送完成，则它需要向服务端发送连接释放请求。

第二次挥手：服务端收到连接释放请求后，会告诉应用层要释放`TCP`链接。然后会发送`ACK`包，并进入`CLOSE_WAIT`状态，此时表明客户端到服务端的连接已经释放，不再接收客户端发的数据了。但是因为`TCP`连接是双向的，所以服务端仍旧可以发送数据给客户端。

第三次挥手：服务端如果此时还有没发完的数据会继续发送，完毕后会向客户端发送连接释放请求，然后服务端便进入`LAST-ACK`状态。

第四次挥手：客户端收到释放请求后，向服务端发送确认应答，此时客户端进入`TIME-WAIT`状态。该状态会持续`2MSL`（最大段生存期，指报文段在网络中生存的时间，超时会被抛弃）时间，若该时间段内没有服务端的重发请求的话，就进入`CLOSED`状态。当服务端收到确认应答后，也便进入`CLOSED`状态。

`TCP`使用四次挥手的原因是因为`TCP`的连接是全双工的，所以需要双方分别释放到对方的连接，单独一方的连接释放，只代表不能再向对方发送数据，连接处于的是半释放的状态。

最后一次挥手中，客户端会等待一段时间再关闭的原因，是为了防止发送给服务器的确认报文段丢失或者出错，从而导致服务器端不能正常关闭。



#### 对`HTML`语义化的理解

语义化是指根据内容的结构化（内容语义化），选择合适的标签（代码语义化）。

语义化的优点如下：

对机器友好，带有语义的文字表现力丰富，更适合搜索引擎的爬虫爬取有效信息，有利于`SEO`。除此之外，语义类还支持读屏软件，根据文章可以自动生成目录；

对开发者友好，使用语义类标签增强了可读性，结构更加清晰，开发者能清晰的看出网页的结构，便于团队的开发与维护。



#### `display`的`block` `inline` `inline-block`的区别

`block`：会独占一行，多个元素会另起一行，可以设置`width` `height` `margin` `padding`属性；

`inline`：元素不会独占一行，设置`width` `height`属性无效。但可以设置水平方向的`margin` `padding`属性，不能设置垂直方向的`padding` `margin`；

`inline-block`：将对象设置为`inline`对象，但对象的内容作为`block`对象呈现，之后的内联对象会被排列在同一行内。



#### 什么是`WebSocket`

`WebSocket`是一种在单个`TCP`连接上进行全双工通信的协议。它允许服务器主动发送信息给客户端，与`HTTP`不同，`WebSocket`提供了持久连接，适用于需要实时数据交换的应用。



#### `WebSocket`和`HTTP`的区别是什么

**连接持续性**：`HTTP`是无状态的，通常用于请求-响应模式的通信，每次请求都需要建立一个新的连接；而`WebSocket`则是在客户端和服务器之间建立一个持久的连接，这个连接会保持打开状态，直到客户端或服务器决定关闭。

**数据传输效率**：`WebSocket`提供了更高效的数据传输，因为它减少了`HTTP`的头部信息和重复建立连接的开销。

**用途**：`HTTP`适用于传统的网页请求和数据获取，而`WebSocket`适用于需要实时数据交换的场景。



#### `WebSocket`如何握手

`WebSocket`握手使用的是`HTTP`协议。客户端发送一个`HTTP`请求到服务器，这个请求包含一个`Upgrade`头部，请求将连接升级到`WebSocket`。如果服务器支持`WebSocket`，它会返回一个状态码`101 Switching Protocols`的响应，表示同意切换协议。之后，连接就从`HTTP`升级到`WebSocket`协议。



#### `WebSocket`是否支持跨域

`WebSocket`支持跨域通信。`WebSocket`握手请求是一个标准的`HTTP`请求，可以包含`Origin`头部，服务器可以检查这个头部来决定是否接受连接。因此，`WebSocket`协议本身支持跨域，但是服务器需要配置允许特定来源的连接。



#### 如何在`WebSocket`中实现心跳机制

心跳机制用于保持连接的活性，防止连接因为长时间的空闲而被网络设备关闭。在`WebSocket`中，客户端和服务器可以通过定期发送非数据帧，如`ping/pong`帧来实现心跳。客户端发送`ping`帧，服务器必须回复`pong`帧，这样可以确保连接的活性。



#### `WebSocket`如何保证安全性

`WebSocket`安全性通过使用`wss://`（`WebSocket Secure`）协议来实现，它是`WebSocket`的加密版本，类似于`HTTPS`。`wss://`在`WebSocket`连接上实现了`TLS`（传输层安全性协议），确保数据的加密和完整性，防止数据被窃听和篡改。



#### 为什么需要打包工具

开发时，我们会使用框架 (`React` `Vue`) ，`ES6`模块化语法，`Less/Sass`等`CSS`预处理器等语法进行开发，这样的代码要想在浏览器运行必须经过编译成浏览器能识别的`JS` `CSS`语法才能运行。所以我们需要打包工具帮我们做完这些事。除此之外，打包还能压缩代码、做兼容性处理、提升代码性能等。



#### 说说你对`webpack`的理解

`webpack`是一个静态模块的打包工具。它会在内部从一个或多个入口点构建一个依赖图，然后将项目中所需的每一个模块组合成一个或多个`bundles`进行输出，它们均为静态资源。输出的文件已经编译好了，可以在浏览器运行。`webpack`具有打包压缩、编译兼容、能力扩展等功能。其最初的目标是实现前端项目的模块化，也就是如何更高效地管理和维护项目中的每一个资源。

`webpack`有五大核心概念：

- 入口(`entry`)
- 输出(`output`)
- 解析器（`loader`）
- 插件(`plugin`)
- 模式(`mode`)



#### `Loader`是什么

其作用是让 Webpack 能够去处理那些非 JavaScript 文件。由于 Webpack 自身只理解 JavaScript、JSON ，其他类型/后缀的文件都需要经过 loader 处理，并将它们转换为有效模块。loader 可以是同步的，也可以是异步的；而且支持链式调用，链中的每个 loader 会处理之前已处理过的资源。

当 `webpack` 碰到不识别的模块时， 就会在配置中查找该文件的解析规则

在`webpack`的配置中，`loader` 有两个属性：

`test`：识别出哪些文件会被转换

`use`：定义在进行转换时，应该使用哪个`loader`



#### 常见的`Loader`

```javascript
babel-loader：使用Babel加载ES2015+ 代码并将其转换为ES5

ts-loader: 将TypeScript转换成JavaScript

sass-loader：将SCSS/SASS代码转换成CSS

style-loader：将模块导出的内容作为样式添加到DOM中

css-loader：加载CSS文件并解析import的CSS文件

less-loader：将Less编译为CSS

node-loader：处理Node.js插件

source-map-loader：加载额外的Source Map文件，以方便断点调试
```



#### `Plugin`是什么

`loader`用于转换某些类型的模块，而插件则可以用于执行范围更广的任务：打包优化，资源管理，注入环境变量。`plugin`会运行在 `webpack`的不同阶段，贯穿整个编译周期，目的在于解决`loader`无法实现的其他事。



#### 有哪些常见的`Plugin`

```javascript
clean-webpack-plugin: 用于在打包前清理上一次项目生成的bundle文件

mini-css-extract-plugin: 分离样式文件，CSS提取为独立文件

webpack-bundle-analyzer: 可视化Webpack输出文件的体积 

speed-measure-webpack-plugin: 可以看到每个Loader和Plugin执行耗时

optimize-css-assets-webpack-plugin：压缩css文件

css-minimizer-webpack-plugin：压缩css文件（用于 webpack 5）

uglifyjs-webpack-plugin：压缩js文件

compression-webpack-plugin：启用gzip压缩

html-webpack-plugin：自动生成一个html文件，并且引用bundle.js文件

terser-webpack-plugin: 可以压缩和去重 js 代码(Webpack4)
```



#### `Loader`和`Plugin`的区别

`loader`能够让`Webpack`能够去处理那些非`JavaScript`文件，因为`Webpack`自身只能理解`JavaScript` `JSON `其他类型/后缀的文件都需要经过`loader`处理，并将它们转换为有效模块。

`plugin`赋予了`webpack`各种灵活的功能例如打包优化、资源管理、环境变量注入等，目的是解决`loader`无法实现的其他事。

**区别一：**

`loader`运行在打包文件之前

`plugins`在整个编译周期都起作用

**区别二：**

`loader`在`module.rules`中配置，类型为数组。每一项都是`Object`，包含了`test` `use`等属性

`Plugin`在`plugins`中单独配置，类型为数组，每一项是一个`Plugin`实例，参数都通过构造函数传入

**区别三：**

在`Webpack`运行的生命周期中会广播出许多事件，`Plugin`可以监听这些事件，在合适的时机通过`Webpack`提供的`API`改变输出结果

对于`loader`，实质是一个转换器，将`A`文件进行编译形成B文件，操作的是文件，比如将`A.scss`转变为`B.css`，单纯的文件转换过程



#### `webpack`的构建流程

初始化参数：从配置文件和`Shell`语句中读取与合并参数，得出最终的参数

开始编译：用上一步得到的参数初始化`Compiler`对象，加载所有配置的插件，执行对象的`run`方法开始执行编译

确定入口：根据配置中的`entry`找出所有的入口文件

编译模块：从入口文件出发，调用所有配置的`Loader`对模块进行翻译，再找出该模块依赖的模块，再递归本步骤直到所有入口依赖的文件都经过了本步骤的处理

完成模块编译：在经过第4步使用`Loader`翻译完所有模块后，得到了每个模块被翻译后的最终内容以及它们之间的依赖关系

输出资源：根据入口和模块之间的依赖关系，组装成一个个包含多个模块的`Chunk`，再把每个`Chunk`转换成一个单独的文件加入到输出列表

输出完成：在确定好输出内容后，根据配置确定输出的路径和文件名，把文件内容写入到文件系统



#### `webpack`如何分包

在`Webpack`中实现代码分割（分包）主要有两种方式：静态导入和动态导入。静态导入通常通过配置来实现，而动态导入则通过代码中的特定语法来实现。以下是实现代码分割的步骤：

**配置多入口（可选）**

如果你的应用有多个独立的入口点（例如，多页应用），你可以配置多个入口。每个入口都会生成自己的代码包。

```javascript
module.exports = {
  entry: {
    main: './src/index.js',
    admin: './src/admin.js'
  },
  output: {
    filename: '[name].bundle.js',
    path: __dirname + '/dist'
  }
};
```

**使用`SplitChunksPlugin`进行代码分割**

`Webpack`内置了`SplitChunksPlugin`插件，用于将公共的依赖模块分割到单独的包中，以避免重复打包。

```javascript
module.exports = {
  optimization: {
    splitChunks: {
      chunks: 'all'
    }
  }
};
```

**动态导入（代码级分割）**

动态导入是通过在代码中使用import()`语法来实现的，它允许你按需加载模块。

```javascript
// 假设我们有一个很大的模块 `largeModule.js`
button.onclick = function() {
  import('./largeModule.js').then(largeModule => {
    largeModule.doSomething();
  });
};
```

这段代码会在用户点击按钮时才加载`largeModule.js`，并且`largeModule.js`会被打包成一个独立的`chunk`。

**配置 `output` 以自定义分包文件名**

你可以在 `output` 配置中使用 `[name]`、`[id]` 和 `[chunkhash]` 等占位符来自定义生成的包文件名。

```javascript
module.exports = {
  output: {
    filename: '[name].bundle.js',
    chunkFilename: '[name].chunk.js',
    path: __dirname + '/dist'
  }
};
```



#### 为什么说`vite`比`webpack`要快

`vite`不需要做全量的打包

`vite`在解析模块依赖关系时，利用了`esbuild`，更快（`esbuild`使用`Go`编写，并且比以`JavaScript`编写的打包器预构建依赖快 10-100 倍）

按需加载：在`HMR`（热更新）方面，当改动了一个模块后，`vite`仅需让浏览器重新请求该模块即可，不像`webpack`那样需要把该模块的相关依赖模块全部编译一次，效率更高。

由于现代浏览器本身就支持 `ES Module`，会自动向依赖的`Module`发出请求。`vite`充分利用这一点，将开发环境下的模块文件，就作为浏览器要执行的文件，而不是像`webpack`那样进行打包合并。

按需编译：当浏览器请求某个模块时，再根据需要对模块内容进行编译，这种按需动态编译的方式，极大的缩减了编译时间。

`webpack`是先打包再启动开发服务器，`vite`是直接启动开发服务器，然后按需编译依赖文件。由于`vite`在启动的时候不需要打包，也就意味着不需要分析模块的依赖、不需要编译，因此启动速度非常快。



#### 两数之和

给定一个整数数组 `nums` 和一个整数目标值 `target`，请你在该数组中找出 **和为目标值** *`target`* 的那 **两个** 整数，并返回它们的数组下标。

你可以假设每种输入只会对应一个答案。但是，数组中同一个元素在答案里不能重复出现。

你可以按任意顺序返回答案。

**示例 1：**

```
输入：nums = [2,7,11,15], target = 9
输出：[0,1]
解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1] 。
```

**示例 2：**

```
输入：nums = [3,2,4], target = 6
输出：[1,2]
```

**示例 3：**

```
输入：nums = [3,3], target = 6
输出：[0,1]
```

解法：

```javascript
var twoSum = function(nums, target) {
    const map = new Map()

    for(let i = 0, len = nums.length; i < len; i++) {
        if(map.has(target - nums[i])) {
            return [map.get(target - nums[i]), i]
        }
        map.set(nums[i], i)
    }
    return [-1, -1]
}
```



#### 整数反转

给你一个 32 位的有符号整数 `x` ，返回将 `x` 中的数字部分反转后的结果。

如果反转后整数超过 32 位的有符号整数的范围 `[−231, 231 − 1]` ，就返回 0。

**假设环境不允许存储 64 位整数（有符号或无符号）。**

**示例 1：**

```
输入：x = 123
输出：321
```

**示例 2：**

```
输入：x = -123
输出：-321
```

**示例 3：**

```
输入：x = 120
输出：21
```

**示例 4：**

```
输入：x = 0
输出：0
```

解法：

```javascript
var reverse = function(x) {
    let y = parseInt(x.toString().split("").reverse().join(""))
    if(x < 0) {
        y = -y
    }
    return y > Math.pow(2, 31) - 1 || y < -Math.pow(2, 31) ? 0 : y
};
```



#### 盛最多的水

给定一个长度为 `n` 的整数数组 `height` 。有 `n` 条垂线，第 `i` 条线的两个端点是 `(i, 0)` 和 `(i, height[i])` 。

找出其中的两条线，使得它们与 `x` 轴共同构成的容器可以容纳最多的水。

返回容器可以储存的最大水量。

**说明：**你不能倾斜容器。

**示例 1：**

![img](https://aliyun-lc-upload.oss-cn-hangzhou.aliyuncs.com/aliyun-lc-upload/uploads/2018/07/25/question_11.jpg)

```
输入：[1,8,6,2,5,4,8,3,7]
输出：49 
解释：图中垂直线代表输入数组 [1,8,6,2,5,4,8,3,7]。在此情况下，容器能够容纳水（表示为蓝色部分）的最大值为 49。
```

**示例 2：**

```
输入：height = [1,1]
输出：1
```

解答：

```javascript
 // 双指针
var maxArea = function(height) {
    let start = 0
    let end = height.length - 1
    let area = 0
    while(start < end) {
        // 计算出当前的容积
        const currentArea = (end - start) * Math.min(height[start], height[end])
        area = Math.max(area, currentArea)
        // start 向前移动
        if(height[start] < height[end]) {
            start++
        } else {
            end--
        }
    }
    return area
};
```



#### 搜索旋转排序数组

整数数组 `nums` 按升序排列，数组中的值 **互不相同** 。

在传递给函数之前，`nums` 在预先未知的某个下标 `k`（`0 <= k < nums.length`）上进行了 **旋转**，使数组变为 `[nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]`（下标 **从 0 开始** 计数）。例如， `[0,1,2,4,5,6,7]` 在下标 `3` 处经旋转后可能变为 `[4,5,6,7,0,1,2]` 。

给你 **旋转后** 的数组 `nums` 和一个整数 `target` ，如果 `nums` 中存在这个目标值 `target` ，则返回它的下标，否则返回 `-1` 。

你必须设计一个时间复杂度为 `O(log n)` 的算法解决此问题。

 

**示例 1：**

```
输入：nums = [4,5,6,7,0,1,2], target = 0
输出：4
```

**示例 2：**

```
输入：nums = [4,5,6,7,0,1,2], target = 3
输出：-1
```

**示例 3：**

```
输入：nums = [1], target = 0
输出：-1
```

解答：

```javascript
// var search = function(nums, target) {
//     for(const [index, k] of nums.entries()) {
//         if(k === target) {
//             return index
//         }
//     }
//     return -1
// };

var search = function(nums, target) {
    return nums.indexOf(target)
};
```



#### 全排列

给定一个不含重复数字的数组 `nums` ，返回其 *所有可能的全排列* 。你可以 **按任意顺序** 返回答案。

 

**示例 1：**

```
输入：nums = [1,2,3]
输出：[[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
```

**示例 2：**

```
输入：nums = [0,1]
输出：[[0,1],[1,0]]
```

**示例 3：**

```
输入：nums = [1]
输出：[[1]]
```

解答：

```javascript
var permute = function (nums) {
    const n = nums.length;
    let res = [];
    let path = [];
    let used = new Array(n).fill(false);
    dfs(0, n, nums, used, path, res);
    return res;
};

function dfs(u, n, nums, used, path, res) {
    if (u == n) {
        res.push(path.slice());
        return;
    }
    for (let i = 0; i < n; i++) {
        if (!used[i]) {
            path.push(nums[i]);
            used[i] = true;
            dfs(u + 1, n, nums, used, path, res);
            used[i] = false;
            path.pop();
        }
    }
}
```



#### 子集

给你一个整数数组 `nums` ，数组中的元素 **互不相同** 。返回该数组所有可能的

子集

（幂集）。

解集 **不能** 包含重复的子集。你可以按 **任意顺序** 返回解集。

**示例 1：**

```
输入：nums = [1,2,3]
输出：[[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]
```

**示例 2：**

```
输入：nums = [0]
输出：[[],[0]]
```

解答：

```javascript
var subsets = function(nums) {
    let result = []
    let path = []
    function backtracking(startIndex) {
        result.push([...path])
        for(let i = startIndex; i < nums.length; i++) {
            path.push(nums[i])
            backtracking(i + 1)
            path.pop()
        }
    }
    backtracking(0)
    return result
};
```



#### 买卖股票的最佳时期

给定一个数组 `prices` ，它的第 `i` 个元素 `prices[i]` 表示一支给定股票第 `i` 天的价格。

你只能选择 **某一天** 买入这只股票，并选择在 **未来的某一个不同的日子** 卖出该股票。设计一个算法来计算你所能获取的最大利润。

返回你可以从这笔交易中获取的最大利润。如果你不能获取任何利润，返回 `0` 。

**示例 1：**

```
输入：[7,1,5,3,6,4]
输出：5
解释：在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。
     注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格；同时，你不能在买入前卖出股票。
```

**示例 2：**

```
输入：prices = [7,6,4,3,1]
输出：0
解释：在这种情况下, 没有交易完成, 所以最大利润为 0。
```

解答：

```javascript
var maxProfit = function(prices) {
    let min = prices[0]
    let max = 0
    for(const k of prices) {
        max = Math.max(max, k - min)
        min = Math.min(min, k)
    }
    return max
}
```



#### 最小栈

设计一个支持 `push` ，`pop` ，`top` 操作，并能在常数时间内检索到最小元素的栈。

实现 `MinStack` 类:

- `MinStack()` 初始化堆栈对象。
- `void push(int val)` 将元素val推入堆栈。
- `void pop()` 删除堆栈顶部的元素。
- `int top()` 获取堆栈顶部的元素。
- `int getMin()` 获取堆栈中的最小元素。

**示例 1:**

```
输入：
["MinStack","push","push","push","getMin","pop","top","getMin"]
[[],[-2],[0],[-3],[],[],[],[]]

输出：
[null,null,null,null,-3,null,0,-2]

解释：
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin();   --> 返回 -3.
minStack.pop();
minStack.top();      --> 返回 0.
minStack.getMin();   --> 返回 -2.
```

解答：

```javascript
var MinStack = function() {
    this.x_stack = [];
    this.min_stack = [Infinity];
};

MinStack.prototype.push = function(x) {
    this.x_stack.push(x);
    this.min_stack.push(Math.min(this.min_stack[this.min_stack.length - 1], x));
};

MinStack.prototype.pop = function() {
    this.x_stack.pop();
    this.min_stack.pop();
};

MinStack.prototype.top = function() {
    return this.x_stack[this.x_stack.length - 1];
};

MinStack.prototype.getMin = function() {
    return this.min_stack[this.min_stack.length - 1];
};
```



#### 整数拆分

给定一个正整数 `n` ，将其拆分为 `k` 个 **正整数** 的和（ `k >= 2` ），并使这些整数的乘积最大化。

返回 *你可以获得的最大乘积* 。

**示例 1:**

```
输入: n = 2
输出: 1
解释: 2 = 1 + 1, 1 × 1 = 1。
```

**示例 2:**

```
输入: n = 10
输出: 36
解释: 10 = 3 + 3 + 4, 3 × 3 × 4 = 36。
```

解答：

```javascript
var integerBreak = function(n) {
    let dp = new Array(n + 1).fill(0)
    dp[2] = 1

    for(let i = 3; i <= n; i++) {
        for(let j = 0; j <= i / 2; j++) {
            dp[i] = Math.max(dp[i], j * (i - j), (j * dp[i - j]))
        }
    }

    return dp[n];
};
```



#### 排序数组

给你一个整数数组 `nums`，请你将该数组升序排列。

**示例 1：**

```
输入：nums = [5,2,3,1]
输出：[1,2,3,5]
```

**示例 2：**

```
输入：nums = [5,1,1,2,0,0]
输出：[0,0,1,1,2,5]
```

解答：

```javascript
// var sortArray = function(nums) {
//     return nums.sort((a, b) => a - b)
// };

// 选择排序
// var sortArray = function(nums) {
//     let n = nums.length
//     for(let i = 0; i < n; i++) {
//         let minIndex = i;
//         for(let j = i + 1; j < n; j++) {
//             if(nums[j] < nums[minIndex]) {
//                 minIndex = j
//             }
//         }
//         [nums[i], nums[minIndex]] = [nums[minIndex], nums[i]]
//     }
//     return nums
// }

// 插入排序
// var sortArray = function(nums) {
//     const n = nums.length;
//     for(let i = 1; i < n; i++) {
//         const key = nums[i];
//         j = i - 1;
//         while(j >= 0 && nums[j] > key) {
//             nums[j + 1] = nums[j]
//             j--
//         }
//         nums[j + 1] = key
//     }
//     return nums;
// }

// 冒泡排序
// var sortArray = function(nums) {
//     let n = nums.length;
//     for(let i = 0; i < n - 1; i++) {
//         for(let j = 0; j < n - i - 1; j++) {
//             if(nums[j] > nums[j + 1]) {
//                 [nums[j], nums[j + 1]] = [nums[j + 1], nums[j]]
//             }
//         }
//     }
//     return nums
// }

// 快速排序
// var sortArray = function(nums) {
//   if (nums.length <= 1) return nums;
//   let pivotIndex = Math.floor(nums.length / 2);
//   let pivot = nums.splice(pivotIndex, 1)[0];
//   let left = [];
//   let right = [];
//   for (let i = 0; i < nums.length; i++) {
//     if (nums[i] < pivot) {
//       left.push(nums[i]);
//     } else {
//       right.push(nums[i]);
//     }
//   }
//   return sortArray(left).concat([pivot], sortArray(right));
// }

// 归并排序
function sortArray(nums) {
    if(nums.length <= 1) return nums;
    const pivotIndex = Math.floor(nums.length / 2);
    const left = nums.slice(0, pivotIndex);
    const right = nums.slice(pivotIndex);
    return merge(sortArray(left), sortArray(right));
}

function merge(left, right) {
    let result = []
    while(left.length && right.length) {
        if(left[0] < right[0]) {
            result.push(left.shift());
        } else {
            result.push(right.shift());
        }
    }
    return result.concat(left, right);
}

```



#### 文物朝代判断

展览馆展出来自 13 个朝代的文物，每排展柜展出 5 个文物。某排文物的摆放情况记录于数组 `places`，其中 `places[i]` 表示处于第 `i` 位文物的所属朝代编号。其中，编号为 0 的朝代表示未知朝代。请判断并返回这排文物的所属朝代编号是否连续（如遇未知朝代可算作连续情况）。

**示例 1：**

```
输入: places = [0, 6, 9, 0, 7]
输出: True
```

**示例 2：**

```
输入: places = [7, 8, 9, 10, 11]
输出: True
```

解答：

```javascript
var checkDynasty = function(places) {
    let arr = places.filter((place) => place !== 0);
    if(arr.length !== (new Set(arr)).size) {
        return false;
    }
    let max = 0;
    let min = 14;
    for (const k of arr) {
        max=Math.max(max, k);
        min=Math.min(min, k);
    }

    return max - min <= 4;
};
```



#### 最长回文子串

给你一个字符串 `s`，找到 `s` 中最长的 回文子串。

**示例 1：**

```
输入：s = "babad"
输出："bab"
解释："aba" 同样是符合题意的答案。
```

**示例 2：**

```
输入：s = "cbbd"
输出："bb"
```

解答：

```javascript
var longestPalindrome = function(s) {
    const len = s.length;
    // 布尔类型的dp[i][j]：表示区间范围[i,j] （注意是左闭右闭）的子串是否是回文子串，如果是dp[i][j]为true，否则为false
    let dp = new Array(len).fill(false).map(() => new Array(len).fill(false));
    // left起始位置  maxlenth回文串长度
    let left = 0, maxlenth = 0;
    for(let i = len - 1; i >= 0; i--){
        for(let j = i; j < len; j++){
            // 情况一：下标i 与 j相同，同一个字符例如a，当然是回文子串 j - i == 0
            // 情况二：下标i 与 j相差为1，例如aa，也是文子串 j - i == 1
            // 情况一和情况二 可以合并为 j - i <= 1
            // 情况三：下标：i 与 j相差大于1的时候，例如cabac，此时s[i]与s[j]已经相同了，我们看i到j区间是不是回文子串就看aba是不是回文就可以了，那么aba的区间就是 i+1 与 j-1区间，这个区间是不是回文就看dp[i + 1][j - 1]===true
            if(s[i] === s[j] && (j - i <= 1 || dp[i + 1][j - 1])){
                dp[i][j] = true;
            }
            // 只要 dp[i][j] == true 成立，就表示子串 s[i..j] 是回文，此时记录回文长度和起始位置
            if(dp[i][j] && j - i + 1 > maxlenth) {
                maxlenth = j - i + 1; // 回文串长度
                left = i; // 起始位置
            }
        }
    }
    return s.substr(left, maxlenth); // 找到子串
};
```

