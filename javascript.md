# Node.js

## ArrayBuffer, Buffer, Uint8Array, DataView

Buffer, Uint8Array, DataViewはバイト列を読み書きするためのViewであり、内部的に持つArrayBufferが実際にデータが格納される実体である。
なお、ArrayBuffer自体には読み書きするインターフェースは用意されていないため、Viewを利用する必要がある。

`Buffer.from(b64str, 'base64')`で作成したBufferの内部で保持しているArrayBufferは8192bytesのようなブロック単位で確保しているため、`Buffer.from(b64str, 'base64').buffer`で得られるArrayBufferのサイズはデータに対して適切なサイズではない。

```js
const b64str = 'aG9nZQ=='; // "hoge"

console.log(Buffer.from(b64str, 'base64').buffer);
// ArrayBuffer {
//   [Uint8Contents]: <2f 00 ... 8190 more bytes>,
//   byteLength: 8192
// }

console.log(new Uint8Array(Buffer.from(b64str, 'base64')).buffer);
// ArrayBuffer { 
//   [Uint8Contents]: <68 6f 67 65>,
//   byteLength: 4
// }
```
