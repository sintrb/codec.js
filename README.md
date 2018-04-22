# codec.js
ASCII/GBK/UTF-8/CODE128A/CODE128B/CODE128C encode or decode with Javascript.

Javascript实现的ASCII/GBK/UTF-8/CODE128A/CODE128B/CODE128C编码和解码封装，支持浏览器端和微信小程序。

例子:
```js
var bytes = codes.gbk.encode("你好"); // gbk编码
console.log(bytes); // 无符号8位数组: [196, 227, 186, 195]
console.log(codes.gbk.decode(bytes)); // gbk解码: 你好

bytes = codes.utf8.encode("你好"); // utf-8编码
console.log(bytes); // 无符号8位数组: [228, 189, 160, 229, 165, 189]
console.log(codes.utf8.decode(bytes)); // utf-8解码: 你好

bytes = codes.ascii.encode("abc"); // ascii编码
console.log(bytes); // 无符号8位数组: [97, 98, 99]
console.log(codes.ascii.decode(bytes)); // ascii解码: abc

```

测试:
```js
['ABC123', 'Hello World', '你好 世界!', '这是一个测试文本👌', 'StartA12541HI', '123456'].map(text => {
    console.log("test for", text);
    Object.keys(codec).map(k => {
        let cd = codec[k];
        let en = cd.encode(text);
        let de = cd.decode(en);
        console.log("\t", k, text == de ? "ok" : "fail", de);
    });
});

```

输出:
```text
test for ABC123
	 gbk ok ABC123
	 utf8 ok ABC123
	 ascii ok ABC123
	 code128a ok ABC123
	 code128b ok ABC123
	 code128c fail 23
test for Hello World
	 gbk ok Hello World
	 utf8 ok Hello World
	 ascii ok Hello World
	 code128a fail H W
	 code128b fail Heo Word
	 code128c fail 
test for 你好 世界!
	 gbk ok 你好 世界!
	 utf8 ok 你好 世界!
	 ascii fail `} L!
	 code128a fail  !
	 code128b fail  !
	 code128c fail 
test for 这是一个测试文本👌
	 gbk fail 这是一个测试文本?0xD83D?0xDC4C
	 utf8 ok 这是一个测试文本👌
	 ascii fail Ù/
```

备注: 
> gbk原始代码出自 https://blog.csdn.net/vnvlyp/article/details/25827999 在此表示感谢。原来的代码在做"ac 你好"的解码时会出错，是因为%20和"你"的第一个编码合并解码导致失败，已做调整调整。

> 对于utf-8中的某些字符无法映射到GBK中，代码亦未经过完整测试，请开发者慎重使用。

> CODE128仅做了编码解码，没有处理校验，需要的开发者自行增加。
