for...in和for...of可以用于循环遍历JavaScript对象和数组等可迭代对象。

for...in是用于遍历对象的属性，循环变量是对象的属性名（字符串类型）。它遍历对象自身所有可枚举的属性，包括从原型链中继承的属性，但是遍历的顺序是不确定的。例如：

```javascript
复制let obj = { a: 1, b: 2, c: 3 };
for (let key in obj) {
  console.log(key); // 输出 a, b, c
}
```

for...of是用于遍历可迭代对象的元素，循环变量是元素的值。它遍历对象自身定义的属性，而且只能遍历实现了遍历器接口的对象（例如数组、Set、Map等）。例如：

```javascript
复制let arr = [1, 2, 3];
for (let value of arr) {
  console.log(value); // 输出 1, 2, 3
}
```

另外需要注意的是，for...of循环可以被用在一些类数组对象上（例如arguments和NodeList），但是不能对普通对象进行遍历。如果要遍历对象中的属性，请使用for...in循环。

总之，for...in用于遍历对象的属性，for...of用于遍历可迭代对象的元素，它们在不同的场合有各自的应用。

for in如果遍历迭代对象的话，将获取到的是下标索引