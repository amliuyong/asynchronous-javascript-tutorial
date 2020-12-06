# asynchronous-javascript-tutorial

## test callback

- Dependencies

```
"devDependencies": {
    "chai": "^4.2.0",
    "mocha": "^6.2.2"
  }
  
```

- Test callback
```javascript

  
const calculateSquare = require('../calculate-square.js');
const expect = require('chai').expect;

describe('calculateSquare', function () {
    it('should return 4 if passed 2', function (done) {
        calculateSquare(2, function (error, result) {
            expect(result).to.equal( 4);
            done();
        });
    });

    it('should return an error if passed string', function (done) {
        calculateSquare('string', function (error, result) {
            expect(error).to.not.equal(null);
            expect(error.message).to.equal('Argument of type number is expected');
            done();
        });
    });    
});

```


## Promise

### Create
```javascript

const myPromise = new Promise(funciton(resolve, reject) {
    resolve("value");
});
```

### Status

   pending
   
   fulfilled
   
   rejected


### How to use Promise

```javascript

myPromise.then(value => {
    console.log('This is inside onFulfilled function');
});

```

### how-to-avoid-callback-hell 

```javascript

// Declaring calculateSquare function
function calculateSquare(number) {
    const promise = new Promise(function (resolve, reject) {
        setTimeout(function () {
            if (typeof number !== 'number') {
                return reject(new Error('Argument of type number is expected'));
            }
            const result = number * number;
            resolve(result);
        }, 1000);
    });
    return promise;
}

calculateSquare(1)
    .then(value => {
        console.log(value);
        return calculateSquare(2);
    })
    .then(value => {
        console.log(value);
        return calculateSquare(3);
    })
    .then(value => {
        console.log(value);
        return calculateSquare(4);
    })
    .then(value => {
        console.log(value);
        return calculateSquare(5);
    })
    .then(value => {
        console.log(value);
        return calculateSquare(6);
    })
    .then(value => {
        console.log(value);
    });
    
```



