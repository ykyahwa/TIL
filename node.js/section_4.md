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

##npm request

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