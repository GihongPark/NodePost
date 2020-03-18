# Crypto

`crypto`모듈은 암호화 기능을 제공하는 모듈입니다.  
암호화에는 **단방향 암호화**와 **양방향 암호화**가 있고, 암호화를 하기 위해선 `crypto`모듈을 require 하여 사용하면 됩니다.

```javascript
const crypto = require('crypto');
```

## 단방향 암호화

단방향 암호화의 경우 비밀번호와 같이 한번 암호화를 한 후, 복호화가 필요하지 않을 때 사용하는 방식입니다.

#### 해시 함수

해시함수는 가장 간단한 단방향 암호화 방식입니다.

```javascript
const hash = crypto.createHash('sha512').update('password').digest('base64');
console.log(hash);  // sQnzu7wkTrgkQZF+0G1hi5AI3Qmzvv0bXgc5THBqi7mAsdd4Xll27ASbRt9fEyavWi6m0QP9B8lThf+rDKy8hg==
```

`createHash()` 메소드에 인자 값으로 사용할 알고리즘을 넣어준 뒤, `update()` 메소드에 암호화할 비밀번호를, `digest()` 메소드에 인코딩 방식을 넣어주면 됩니다.

하지만 해시 함수의 경우 매번 같은 결과를 반환하기 때문에 이미 **레인보우 테이블**이 존재하기 때문에 비밀번호 암호화에는 사용하지 않습니다.  
또 이러한 **레인보우 테이블**을 피하기 위해 `salt`를 사용하여 랜덤 문자열을 생성하여 비밀번호 암호화를 합니다.

#### PBKDF2

해시의 문제점을 해결하기 위해 `pbkdf2 방식을` 사용해 봅시다.  
먼저 `randomBytes()` 메소드를 이용하여 `salt`를 생성하고 암호화를 하겠습니다.

```javascript
crypto.randomBytes(64, (err, buff) => {
    console.log('buff: ', buff.toString('base64'));   // buff값은 매번 달라짐
    crypto.pbkdf2('password', buff.toString('base64'), 1000, 64, 'sha512', (err, key) => {
        console.log('key: ', key.toString('base64'));
    })
});
```

`randomBytes(size [, callback])` 메소드는 `size` 크기의 난수를 반환해 줍니다.  
`pbkdf2(password, salt, iterations, keylen, digest, callback) 메서드는` 비밀번호, salt, 반복 횟수, 비밀번호 길이, 암호화 알고리즘, 콜백 함수를 인자 값으로 가집니다.  
여기서 `buff`와 `key`값은 바이너리 값으로 반환되기 때문에 `toString('base64')`로 변경해줍니다.

#### scrypt

`scrypt`방식은 `pbkdf2`방식 보다 더 안전하다고 평가받는 방식이라고 합니다.

```javascript
crypto.randomBytes(64, (err, buff) => {
    crypto.scrypt('password', buff.toString('base64'), 64, (err, key) => {
        console.log(key.toString('base64'));
    })
});
```

`scrypt메서드는` 비밀번호, salt, 비밀번호 길이, 콜백 함수를 인자 값으로 가집니다.
