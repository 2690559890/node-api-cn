<!-- YAML
changes:
  - version: v6.4.0
    pr-url: https://github.com/nodejs/node/pull/7111
    description: Introduced `latin1` as an alias for `binary`.
  - version: v5.0.0
    pr-url: https://github.com/nodejs/node/pull/2859
    description: Removed the deprecated `raw` and `raws` encodings.
-->

当字符串数据被存储入 `Buffer` 实例或从 `Buffer` 实例中被提取时，可以指定一个字符编码。

```js
const buf = Buffer.from('hello world', 'ascii');

console.log(buf.toString('hex'));
// 打印: 68656c6c6f20776f726c64
console.log(buf.toString('base64'));
// 打印: aGVsbG8gd29ybGQ=

console.log(Buffer.from('fhqwhgads', 'ascii'));
// 打印: <Buffer 66 68 71 77 68 67 61 64 73>
console.log(Buffer.from('fhqwhgads', 'utf16le'));
// 打印: <Buffer 66 00 68 00 71 00 77 00 68 00 67 00 61 00 64 00 73 00>
```

Node.js 当前支持的字符编码有：

* `'ascii'`: 仅适用于 7 位 ASCII 数据。此编码速度很快，如果设置则会剥离高位。

* `'utf8'`: 多字节编码的 Unicode 字符。许多网页和其他文档格式都使用 UTF-8。

* `'utf16le'`: 2 或 4 个字节，小端序编码的 Unicode 字符。支持代理对（U+10000 至 U+10FFFF）。

* `'ucs2'`: `'utf16le'` 的别名。

* `'base64'`: Base64 编码。当从字符串创建 `Buffer` 时，此编码也会正确地接受 [RFC 4648 第 5 节][RFC 4648, Section 5]中指定的 “URL 和文件名安全字母”。

* `'latin1'`: 一种将 `Buffer` 编码成单字节编码字符串的方法（由 [RFC 1345] 中的 IANA 定义，第 63 页，作为 Latin-1 的补充块和 C0/C1 控制码）。

* `'binary'`: `'latin1'` 的别名。

* `'hex'`: 将每个字节编码成两个十六进制的字符。

```js
Buffer.from('1ag', 'hex');
// 打印 <Buffer 1a>，当遇到第一个非十六进制的值（'g'）时，则数据会被截断。

Buffer.from('1a7g', 'hex');
// 打印 <Buffer 1a>，当数据以一个数字（'7'）结尾时，则数据会被截断。

Buffer.from('1634', 'hex');
// 打印 <Buffer 16 34>，所有数据均可用。
```

现代的 Web 浏览器遵循 [WHATWG 编码标准][WHATWG Encoding Standard]，将 `'latin1'` 和 `'ISO-8859-1'` 别名为 `'win-1252'`。
这意味着当执行 `http.get()` 之类的操作时，如果返回的字符集是 WHATWG 规范中列出的字符集之一，则服务器可能实际返回 `'win-1252'` 编码的数据，而使用 `'latin1'` 编码可能错误地解码字符。

