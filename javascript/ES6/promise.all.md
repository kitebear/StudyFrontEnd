Promise.all 是一个静态方法，用于处理多个 Promise 对象。它会接收一个 Promise 对象数组作为参数，返回一个新的 Promise 对象，该 Promise 对象在所有 Promise 对象已完成时才会完成，并携带着所有 Promise 对象完成的结果（按照 Promise 对象数组顺序）。

以下是实现一个简单的 Promise.all 的核心代码：

```js
function PromiseAll(promises) {
  return new Promise((resolve, reject) => {
    const result = [];
    let count = 0;
    for (let i = 0; i < promises.length; i++) {
      promises[i]
        .then(res => {
          result[i] = res; // 将 Promise 结果存入数组
          count++; // 记录已完成的 Promise 数量
          if (count === promises.length) {
            resolve(result); // 当所有 Promise 都完成时，返回结果
          }
        })
        .catch(err => {
          reject(err); // 有一个 Promise 出错即返回错误信息
        });
    }
  });
}
```

这个 `PromiseAll` 函数实现的流程是：

1. 接收 Promise 数组作为参数
2. 创建一个数组 result，用于存储所有 Promise 完成的结果
3. 创建一个计数器 count，用于记录已完成的 Promise 数量
4. 循环遍历 Promise 数组，对每个 Promise 进行操作
5. 当 Promise 完成后，将结果存入 result 数组，并将计数器加 1，同时判断是否所有 Promise 都完成
6. 当所有 Promise 都完成时，调用 resolve 函数，将 result 数组作为参数返回，否则不做任何操作
7. 出现错误时，调用 reject 函数，返回错误信息。

可以使用以下代码测试 `PromiseAll` 函数是否实现正确：

```javascript
const promise1 = Promise.resolve(1);
const promise2 = Promise.resolve('hello');
const promise3 = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve('world');
  }, 1000);
});

PromiseAll([promise1, promise2, promise3])
  .then(res => console.log(res)) // [1, 'hello', 'world']
  .catch(err => console.log(err));
```