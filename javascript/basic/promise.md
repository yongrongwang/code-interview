### promise 构造器

Promise 对象的构造器语法：`let promise = new Promise(function(resolve, reject) {}`，传递给
`new Promise` 的函数被称为 executor。当 `new Promise` 被创建，executor 会自动运行。它的参数
`resolve` 和 `reject` 是由 JavaScript 自身提供的回调。我们的代码仅在 executor 的内部。当
executor 获得了结果，它应该调用以下回调之一：
- `resolve(value)` —— 如果任务成功完成并带有结果 `value`。
- `reject(error)` —— 如果出现了 error，`error` 即为 error 对象。
由 `new Promise` 构造器返回的 `promise` 对象具有以下内部属性：
- `state` —— 最初是 `"pending"`，然后在 `resolve` 被调用时变为 `"fulfilled"`，或者在 `reject` 被
  调用时变为 `"rejected"`。
- `result` —— 最初是 `undefined`，然后在 `resolve(value)` 被调用时变为 `value`，或者在
  `reject(error)` 被调用时变为 `error`。
一个 resolved 或 rejected 的 promise 都会被称为 `"settled"`。

### promise 方法

Promise 对象的 then 函数语法：`promise.then(function(result) {}, function(error) {})`，
- `.then` 的第一个参数是一个函数，该函数将在 promise resolved 且接收到结果后执行。
- `.then` 的第二个参数也是一个函数，该函数将在 promise rejected 且接收到 error 信息后执行。
如果我们只对 `error` 感兴趣，那么我们可以使用 `null` 作为第一个参数，`.catch(f)` 调用是
`.then(null, f)` 的完全的模拟，它只是一个简写形式。
调用 `.finally(f)` 类似于 `.then(f, f)`，因为当 promise settled 时 `f` 就会执行：无论 promise 被
resolve 还是 reject。`finally` 处理程序没有参数，也不应该返回任何内容。

Promise 静态方法：
- `Promise.resolve(value)` —— 使用给定 `value` 创建一个 resolved 的 promise。
- `Promise.reject(error)` —— 使用给定 `error` 创建一个 rejected 的 promise。
- `Promise.all(promises)` —— 等待所有 promise 都 resolve 时，返回存放它们结果的数组。如果给定
  的任意一个 promise 为 reject，那么它就会变成 `Promise.all` 的 `error`，所有其他 promise 的结果
  都会被忽略。
- `Promise.race(promises)` —— 等待第一个 settle 的 promise，并将其 `result`/`error` 作为结果返回。

### promise 链

Promise 链：如果 `.then`（或 `catch`/`finally` 都可以）处理程序返回一个 promise，那么链的其余
部分将会等待，直到它状态变为 settled。当它被 settled 后，其 `result`（或 `error`）将被进一步
传递下去。

### promise 解决的问题

我们确定了通过回调表达程序异步和管理并发的两个主要缺陷：缺乏顺序性和可信任性。
Promise 解决了我们因只用回调的代码而备受困扰的控制反转问题。它们并没有摈弃回调，只是
把回调的安排转交给了一个位于我们和其他工具之间的可信任的中介机制。Promise 链也开始
提供（尽管并不完美）以顺序的方式表达异步流的一个更好的方法。

### 生成器

yield / next(..) 这一对不只是一种控制机制，实际上也是一种双向消息传递机制。 yield .. 表达式
本质上是暂停下来等待某个值，接下来的 next(..) 调用会向被暂停的 yield 表达式传回一个值
（或者是隐式的 undefined）。
