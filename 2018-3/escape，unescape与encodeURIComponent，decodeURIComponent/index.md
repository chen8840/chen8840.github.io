### escape，unescape与encodeURIComponent，decodeURIComponent
escape：将string转成unicode字符串
```JAVASCRIPT
escape('𠮷 a')
//输出"%uD842%uDFB7%20a" 等同于 “%uD842%uDFB7%u0020%u0061”
```
unescape：使用类似String.fromCharCode的机制将unicode字符串转成string
```JAVASCRIPT
unescape('%uD842%uDFB7%20a')
//输出"𠮷 a"
String.fromCharCode(0xD842,0xDFB7, 0x20, 0x61);
//输出"𠮷 a"
```
encodeURIComponent：将string转成utf-8字符串
```JAVASCRIPT
encodeURIComponent('𠮷 a')
//输出"%F0%A0%AE%B7%20a" 等同于 “%F0%A0%AE%B7%20%61”
```
decodeURIComponent：将utf-8字符串转成string
```JAVASCRIPT
decodeURIComponent('%F0%A0%AE%B7%20a')
//输出"𠮷 a"
```
应用：

1.将string转成utf-8 格式存储的UInt8Array
```JAVASCRIPT
function stringBufToUInt8Array(str) {
    var bytes = new Uint8Array(new ArrayBuffer(str.length));
    for(var i = 0; i < str.length; ++i) {
        bytes[i] = str.charCodeAt(i) & 0xff;
    }
    return bytes;
}

function printUInt8Array(bytes) {
    console.log([...bytes].map(byte => byte.toString(16).toUpperCase()).join(' '));
}

function stringToUTF8FormatUInt8Array(str) {
    var stringBuf = unescape(encodeURIComponent(str));
    return stringBufToUInt8Array(stringBuf);
}

printUInt8Array(stringToUTF8FormatUInt8Array('𠮷 a'));
//输出 F0 A0 AE B7 20 61
```
2.将utf-8格式存储的UInt8Array转成string
```JAVASCRIPT
function UTF8FormatUInt8ArrayToString(bytes) {
    if(bytes.length > 0) {
        var convertStr = '%' + [...bytes].map(byte => byte.toString(16).toUpperCase()).join('%');
        return decodeURIComponent(convertStr);
    } else {
    return '';
    }
}

var bytesArray = [0xF0, 0xA0, 0xAE, 0xB7, 0x20, 0x61];
var bytes = new Uint8Array(new ArrayBuffer(bytesArray.length));
for(var i = 0; i < bytesArray.length; ++i) {
    bytes[i] = bytesArray[i];
}

console.log(UTF8FormatUInt8ArrayToString(bytes));
//输出 “𠮷 a”
```
