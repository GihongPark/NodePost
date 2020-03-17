# Assert
**Assert**는 테스트를 위해 데이터나 수식을 검사하는 모듈입니다.
node.js에서 모듈로 제공해주므로 import하여 바로 사용할 수 있습니다.
```javascript
const assert = require('assert');
```

## Assert 내장함수
**Assert**는 자체적으로 참,거짓을 판별하는 함수이기도 하지만, assert.equal, assert.deepEqual, assert.throws 함수들도 자주 사용됩니다.

### assert()
`assert()`는 주어진 인자가 거짓인 경우 오류를 발생시킵니다.
`assert.ok()`는 `assert()`와 같이 작동하면 `assert.isError()`는 반대로 작동합니다.
```javascript
const foo = 20,
      bar = 10;
assert(foo < bar);            // [ERR_ASSERTION]
assert.ok(foo < bar);         // [ERR_ASSERTION]
assert.isError(foo < bar);    // 정상작동
```

### assert.equal() / assert.notEqual()
`assert.equal()`은 두 인자를 받아 값이 다를 경우 오류를 발생시킵니다. (`assert.notEqual()`은 부정)
이때 값의 타입은 무시합니다.
```javascript
const foo = 10,
      bar = '10',
      baz = 20;
assert.equal(foo, bar);     // 정상작동
assert.equal(foo, baz);     // [ERR_ASSERTION]: 10 == 20
assert.notEqual(foo, bar);  // [ERR_ASSERTION]: 10 != '10'
```
타입 검사가 필요할 경우, `assert.strictEqual()`을 사용할 수 있습니다. (`assert.notStrictEqual()`은 부정)
```javascript
const foo = 10,
      bar = 10,
      baz = '10';
assert.strictEqual(foo, bar);       // 정상작동
assert.strictEqual(foo, baz);       // [ERR_ASSERTION]: 10 !== '10'
assert.notStrictEqual(foo, bar);    // [ERR_ASSERTION]: Expected "actual" to be strictly unequal to: 10
```

### assert.deepEqual() / assert.notDeepEqual()
`assert.deepEqual()`은 인자로 받은 두 객체의 속성 값이 다를 경우 오류를 발생시킵니다. (`assert.notDeepEqual()`은 부정)
```javascript
const foo = { a: 10, b: 20 },
      bar = { a: 10, b: '20' },
      baz = { a: 20, b: 20 };
assert.deepEqual(foo, bar);         // 정상작동
assert.deepEqual(foo, baz);         // [ERR_ASSERTION]
assert.notDeepEqual(foo, bar);      // [ERR_ASSERTION]
```
마찬가지로 `assert.deepStrictEqual()`은 객체 속성 값의 타입까지 검사합니다.
```javascript
const foo = { a: 10, b: 20 },
      bar = { a: 10, b: '20' };
assert.deepStrictEqual(foo, bar);       // [ERR_ASSERTION]
assert.notDeepStrictEqual(foo, bar);    // 정상작동
```

### assert.throws()
`assert.throws()`는 인자로 받은 함수가 오류를 throws하는지 체크하는 함수입니다.
```javascript
const foo = () => {
    throw new Error();
};
const bar = () => {
    // 정상작동
}
assert.throws(foo);     // 정상작동
assert.throws(bar);     // [ERR_ASSERTION]
```
이때, `assert.throws()`는 오류가 발생하지 않아야 에러를 발생한다는 것을 알 수 있습니다.

여기서 하나더! 두번째 인자에 오류 유형을 같이 넣어 줄 경우
함수의 오류유형과 인자로 넣어준 오류 유형과 일치 하지않으면 오류를 발생시킵니다.
```javascript
const foo = () => {
    return data;    // ReferenceError
}
assert.throws(foo, ReferenceError);     // 정상작동
assert.throws(foo, TypeError);          // ReferenceError: data is not defined
```

