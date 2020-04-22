<!-- YAML
added: v0.11.15
changes:
  - version: v10.0.0
    pr-url: https://github.com/nodejs/node/pull/18395
    description: Removed `noAssert` and no implicit coercion of the offset
                 to `uint32` anymore.
-->

* `value` {number} 要写入 `buf` 的数值。
* `offset` {integer} 开始写入之前要跳过的字节数。必须满足：`0 <= offset <= buf.length - 8`。**默认值:** `0`。
* 返回: {integer} `offset` 加上已写入的字节数。

用指定的[字节序][endianness]（`writeDoubleBE()` 写入为大端序，`writeDoubleLE()` 写入为小端序）将 `value` 写入到 `buf` 中指定的 `offset` 位置。
`value` 必须是 JavaScript 数值。
当 `value` 不是 JavaScript 数值时，行为是未定义的。

```js
const buf = Buffer.allocUnsafe(8);

buf.writeDoubleBE(123.456, 0);

console.log(buf);
// 打印: <Buffer 40 5e dd 2f 1a 9f be 77>

buf.writeDoubleLE(123.456, 0);

console.log(buf);
// 打印: <Buffer 77 be 9f 1a 2f dd 5e 40>
```

