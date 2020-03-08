# Buffer
앞서 본 [Global 모듈](../01. Global 모듈/global module.md)에 이어 Buffer에 대해 알아보겠습니다.

**Buffer**는 binary 데이터를 처리할때 사용되며, **alloc()**, **allocUnsafe()**, **from()** 함수를 통해 객체를 생성할 수 있습니다.
**alloc()** 과 **allocUnsafe()** 의 차이는 **alloc()** 는 초기값을 0으로 초기화하며. **allocUnsafe()** 는 초기값을 초기화 하지않기때문에 데이터를 바로 넣을 시 객체 생성에 유리합니다.
```javascript
const buff1 = Buffer.alloc(10);         // 초기값을 0으로 초기화
const buff2 = Buffer.allocUnsafe(10);   // 초기화 X
const buff3 = Buffer.from("buffer");

console.log("buff1: ", buff1)   // buff1: Buffer(10) [0, 0, 0, 0, 0, 0, 0, 0, …]
console.log("buff2: ", buff2)   // buff2: Buffer(10) [2, 0, 0, 0, 10, 12, 144, 58, …]
console.log("buff3: ", buff3)   // buff3: Buffer(6) [98, 117, 102, 102, 101, 114]

// Buffer.byteLength(), length로 buffer의 크기를 확인할 수 있습니다.
console.log("buff1 size: ", Buffer.byteLength(buff1));  // buff1 size: 10
console.log("buff2 size: ", buff2.length);              // buff2 size: 10
```

## Buffer 내장함수
Buffer에는 여러 내장함수들이 존재합니다.

#### compare
**compare**는 두 버퍼객체를 비교합니다.
* 두 버퍼 값이 같으면 0 반환
* 첫번째 버퍼 값이 크면 1 반환
* 첫번째 버퍼 값이 작으면 -1 반환
```javascript
const buff1 = Buffer.from("bbb");
const buff2 = Buffer.from("aaa");
const buff3 = Buffer.from("bbb");
const buff4 = Buffer.from("ccc");

console.log("bbb=aaa : ", Buffer.compare(buff1, buff2));    // 1
console.log("bbb=bbb : ", Buffer.compare(buff1, buff3));    // 0
console.log("bbb=ccc : ", Buffer.compare(buff1, buff4));    // -1
```

#### equals
**equals**는 두 버퍼의 내용이 같은지 비교합니다.
```javascript
const buff1 = Buffer.from("aaa");
const buff2 = Buffer.from("aaa");
const buff3 = Buffer.from("bbb");

console.log(buff1.equals(buff2));   // true
console.log(buff1.equals(buff3));   // false
```

#### concat
**concat**은 배열안에 있는 모든 버퍼를 하나로 합쳐 새로운 버퍼로 만들어줍니다.
```javascript
const buff1 = Buffer.from("aaa");
const buff2 = Buffer.from("bbb");
const buff3 = Buffer.from("ccc");

const arr = [buff1, buff2, buff3];

console.log(Buffer.concat(arr).toString()); // aaabbbccc
```
여기서 **toString()** 함수는 바이너리값인 버퍼를 문자열로 가져오는 함수입니다.

#### copy
**copy**는 버퍼의 값을 다른 버퍼에 복사합니다.
```javascript
const buff1 = Buffer.from("1234567890");
const buff2 = Buffer.alloc(10);
buff1.copy(buff2, 3, 2, 5);     // buff1의 인덱스2~4까지의 값을 buff2의 인덱스3부터 복사

console.log(buff2);             // Buffer(10) [0, 0, 0, 51, 52, 53, 0, 0, …]
console.log(buff2.toString());  //    345
```

#### fill
**fill**은 버퍼의 크기만큼 지정된 값으로 채웁니다
```javascript
const buff1 = Buffer.from("aaaaaaaa");
console.log(buff1.toString());  // aaaaaaaa

buff1.fill('c');
console.log(buff1.toString());  // cccccccc

buff1.fill('abc');
console.log(buff1.toString());  //  abcabcab
```

#### entries
**entries**는 버퍼의 내용을 [인덱스, 값] 형태로 만들어 배열로 반환합니다.
```javascript
const buff1 = Buffer.from("KOREA");
const arr = buff1.entries();

for(const res of arr) {
    console.log(res);
}

// output //////////
// Array(2) [0, 75]
// Array(2) [1, 79]
// Array(2) [2, 82]
// Array(2) [3, 69]
// Array(2) [4, 65]
```
#### includes
**includes**는 버퍼에 지정된 값이 있는지 확인합니다.
```javascript
const buff1 = Buffer.from("hello node.js");

console.log(buff1.includes("node"));    // true
console.log(buff1.includes("java"));    // false
```

#### indexOf
**indexOf**는 지정된 값의 위치를 반환합니다. (값이 없으면 -1을 반환합니다.)
```javascript
const buff1 = Buffer.from("hello node.js");

console.log(buff1.indexOf("node"));     // 6
console.log(buff1.indexOf("java"));     // -1
```

#### isBuffer
**isBuffer**는 버퍼객체 유무를 판단합니다.
```javascript
const buff1 = Buffer.alloc(10);
const obj1 = {};

console.log("buff1: ", Buffer.isBuffer(buff1)); // buff1: true
console.log("obj1: ", Buffer.isBuffer(obj1));   // obj1: false
```

#### keys
**keys**는 버퍼의 인덱스값을 반환합니다.
```javascript
const buff1 = Buffer.from("abcde");
const arr = buff1.keys();
for(const i of arr) {
    console.log(i);
}

// output //////////
// 0
// 1
// 2
// 3
// 4
```
