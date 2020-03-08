# Global 모듈
node.js로 개발하기 위해서는 여러 모듈을 사용하게 되고, 이 모듈은 각각 그에 맞는 여러 객체들을 제공합니다.

예)
```javascript
const http = require('http');   // http 통신을 위한 모듈

const hostname = '127.0.0.1';   // hostIP
const port = 3000;              // 포트번호

// 서버 생성
const server = http.createServer((req, res) => {
  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/plain');
  res.end('Hello World');
});

// 서버 실행
server.listen(port, hostname, () => {
  console.log(`Server running at http://${hostname}:${port}/`);
});
```
이때 require() 함수와 같이 별도의 모듈을 등록하지 않고 사용할 수 있는 객체들을 모아둔 모듈을 Global 모듈이라고 합니다.

## Global 모듈의 종류
그럼 이제 Global 모듈에 어떤 객체들이 있는지 알아 보겠습니다.

#### Buffer
**Buffer**는 메모리를 동적 할당할 때 사용 합니다.

#### __dirname
**__dirname**은 현재 실행 중인 파일의 경로를 반환합니다.
```javascript
console.log("__dirname:", __dirname)   // __dirname: [파일경로]
```

#### __filename
**__filename**은 현재 실행 중인 파일의 경로와 파일명을 반환합니다.
```javascript
console.log("__filename:", __filename)  // __filename: [파일경로]\[파일명]
```

#### setImmediate
**setImmediate**는 하나의 사건처리가 끝나면 동작할 코드를 등록합니다.
```javascript
function func1() { 
    console.log("func1 실행");
    setImmediate(function() { 
        console.log("func1 Immediate 실행"); 
    });
}
function func2() { 
    console.log("func2 실행"); 
    console.log("func2 실행"); 
}

func1();
setImmediate(function() { 
    console.log("Immediate 실행"); 
});
func2();

// output ///////////////
// func1 실행
// func2 실행
// func2 실행
// func1 Immediate 실행
// Immediate 실행
```

#### clearImmediate
**clearImmediate**는 Immediate를 제거합니다.
```javascript
function func1() { 
    console.log("func1 실행");
    setImmediate(function() { 
        console.log("func1 Immediate 실행"); 
    });
}
function func2() { 
    console.log("func2 실행"); 
    console.log("func2 실행"); 
}

func1();
const foo = setImmediate(function() { 
    console.log("Immediate 실행"); 
});
func2();
clearImmediate(foo);

// output ///////////////
// func1 실행
// func2 실행
// func2 실행
// func1 Immediate 실행
```

#### setInterval
**setInterval**은 정해진 시간마다 함수를 반복 호출합니다.
```javascript
let i = 0;
setInterval(function() {
    console.log(i++);
}, 1000);

// output ///////////////
// 0
// 1
// 2
// ... (1초마다 무한 반복)
```

#### clearInterval
**clearInterval**은 Interval을 제거합니다.
```javascript
let i = 0;
const foo = setInterval(function() {
    console.log(i++);
    if(i > 3) {
        clearInterval(foo);
    }
}, 1000);

// output ///////////////
// 0
// 1
// 2
// 3
```

#### setTimeout
**setTimeout**은 정해진 시간 후 한번만 함수를 호출합니다.
```javascript
setTimeout(function() {
    console.log("setTimeout 동작");
}, 1000);

// output ///////////////
// (1초 후)setTimeout 동작
```

#### clearTimeout
**clearTimeout**은 Timeout을 제거합니다.
```javascript
const foo = setTimeout(function() {
    console.log("setTimeout 동작");
}, 1000);
clearTimeout(foo);

// output ///////////////
// (결과 없음)
```

#### exports, require
**exports**는 사용자가 모듈을 생성할 때 사용합니다.
**require**는 만들어진 모듈을 가져옵니다.
```javascript
/**
 * moduleTest.js
 */
exports.func1 = function() {
    console.log("exports를 이용하여 모듈 생성");
}

/**
 * main.js
 */
const foo = require("./main.js");
foo.func1();

// output ///////////////
// exports를 이용하여 모듈 생성
