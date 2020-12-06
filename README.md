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

const resolvedPromise = Promise.resolve(anyValue)

const rejectedPromise = Promise.reject(new Error('Error'));

```

### Status

   pending
   
   fulfilled
   
   rejected


### How to use Promise

```javascript

myPromise.then(value => {
    console.log('This is inside onFulfilled function');
}).catch(function (error) {
        console.log(error);
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
### Promisifiy any funciton

```javascript

// Ordinary capitalize function
function capitalize(text) {
    return text[0].toUpperCase() + text.substr(1);
}


// Promisified capitalize function
function capitalize(text) {
    return new Promise(function (resolve, reject) {
        if (typeof text !== 'string') {
            return reject(new Error('Argument must be a string'));
            // Don't forget to put return statement here, otherwise
            // the execution will go further
        }
        const result = text[0].toUpperCase() + text.substr(1);
        return resolve(result);
    })
}

// use Promise to read data from DB

var mysql      = require('mysql');
var connection = mysql.createConnection({
    host     : 'localhost',
    user     : 'root',
    password : 'password',
    database : 'ecommerce_website'
});
connection.connect();

function getUsers() {
    return new Promise(function (resolve, reject) {
        connection.query('SELECT * FROM users', function (error, results, fields) {
            if (error) {
                return reject(error);
            }
            return resolve(results);
        });
    });
}

getUsers()
    .then(function (users) {
        console.log('List of users: ');
        users.forEach(function (user) {
            console.log(`${user.user_id}. ${user.firstname} ${user.lastname}`);
        });
    })
    .catch(function (error) {
        console.log(error);
    });

connection.end();

```
### chaining-promises
```javascript
calculateSquare(1)
    .then(value => {
        console.log(value);
        return calculateSquare(2);
    })
    .then(value2 => {
        console.log(value2);
    }, reason => {
        console.log('error happened: ' + reason);
    });
    
```    


### Convert any value to promise

```javascript

const resolvedPromise = Promise.resolve(anyValue)

const rejectedPromise = Promise.reject(anyValue);


```
### Promise.all() - executing promises in parallel

```javascript
// Declare 3 functions which imitate the Dealer API
function askFirstDealer() {
    return new Promise((resolve, reject) => {
        setTimeout(() => resolve(8000), 3000);
    });
}
function askSecondDealer() {
    return new Promise((resolve, reject) => {
        setTimeout(() => resolve(12000), 5000);
    });
}
function askThirdDealer() {
    return new Promise((resolve, reject) => {
        setTimeout(() => resolve(10000), 8000);
    });
}

// Invoke these 3 functions in parallel
Promise.all([
    askFirstDealer(), 
    askSecondDealer(), 
    askThirdDealer()
])
.then(prices => {
    console.log(prices);
});
```

