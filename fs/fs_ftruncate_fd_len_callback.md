<!-- YAML
added: v0.8.6
changes:
  - version: v10.0.0
    pr-url: https://github.com/nodejs/node/pull/12562
    description: The `callback` parameter is no longer optional. Not passing
                 it will throw a `TypeError` at runtime.
  - version: v7.0.0
    pr-url: https://github.com/nodejs/node/pull/7897
    description: The `callback` parameter is no longer optional. Not passing
                 it will emit a deprecation warning with id DEP0013.
-->

* `fd` {integer}
* `len` {integer} **默认值:** `0`。
* `callback` {Function}
  * `err` {Error}

异步的 ftruncate(2)。
除了可能的异常，完成回调没有其他参数。

如果文件描述符指向的文件大于 `len` 个字节，则只有前面 `len` 个字节会保留在文件中。

例如，以下程序只保留文件的前 4 个字节：

```js
console.log(fs.readFileSync('temp.txt', 'utf8'));
// 打印: Node.js

// 获取要截断的文件的文件描述符。
const fd = fs.openSync('temp.txt', 'r+');

// 将文件截断为前 4 个字节。
fs.ftruncate(fd, 4, (err) => {
  assert.ifError(err);
  console.log(fs.readFileSync('temp.txt', 'utf8'));
});
// 打印: Node
```

如果文件小于 `len` 个字节，则会对其进行扩展，并且扩展部分将填充空字节（`'\0'`）：

```js
console.log(fs.readFileSync('temp.txt', 'utf8'));
// 打印: Node.js

// 获取要截断的文件的文件描述符。
const fd = fs.openSync('temp.txt', 'r+');

// 将文件截断为前 10 个字节，但实际大小为 7 个字节。
fs.ftruncate(fd, 10, (err) => {
  assert.ifError(err);
  console.log(fs.readFileSync('temp.txt'));
});
// 打印: <Buffer 4e 6f 64 65 2e 6a 73 00 00 00>
// (UTF8 的值为 'Node.js\0\0\0')
```

最后 3 个字节是空字节（`'\0'`），以补充超出的截断。

