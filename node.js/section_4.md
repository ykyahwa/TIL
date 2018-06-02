# Section 4

## Async

```javascript
console.log('Starting app');

setTimeout(() => {
    console.log('Inside of callback');
}, 2000);

console.log('Finishing up');

실행결과
Starting app
Finishing up
Inside of callback
```

## npm request

<https://www.npmjs.com/package/request>

npm install request@2.87.0 --save

```javascript
const request = require('request');

request({
    url: 'https://maps.googleapis.com/maps/api/geocode/json?address=korea',
    json: true
}, (error, response, body) => {
    console.log(JSON.stringify(body, undefined, 2));
})
```

## Encode, Decode

```
node 터미널
> encodeURIComponent('도산대로 139')
'%EB%8F%84%EC%82%B0%EB%8C%80%EB%A1%9C%20139'
> decodeURIComponent('%EB%8F%84%EC%82%B0%EB%8C%80%EB%A1%9C%20139')
'도산대로 139'
```

## ES6 Promise

```
var asyncAdd = (a, b) => {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            if (typeof a === 'number' && typeof b === 'number') {
                resolve(a + b);
            } else {
                reject('Arguments must be numbers' );
            }
        }, 1500);
    });
};

asyncAdd(5, 7).then((res) => {
    console.log('Result: ', res);
    return asyncAdd(res, 33);
}).then((res) => {
    console.log('Should be 45', res)
}).catch((errorMessage) => {
    console.log(errorMessage);
});
```
## NPM axios

<https://www.npmjs.com/package/axios>