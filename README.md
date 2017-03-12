# get-to-know-es6-and-beyond
This repository is a self reference readme guide to ES6.

## Table of Contents

* [Generators](#generators)
* [Promises](#promises)
* [Async & Await](#async--await)

## Generators

+ A function executes until it reaches a return or end of the function.
+ An ES6 generator can be used to pause or resume at arbitary points inside the function.
+ To create a generator we place an '*' after the function keyword. `function* Kakarot()`
+ When we call a generator `Kakarot()` it returns an instance of itself and doesn't execute itself.
+ To execute, we assign the generator to a variable and call the `next()` on the variable, which executes until the next block is over.

``` javascript
function* CallKakarot() {
    console.log('Super saiyan God!');
}

const kakarot = CallKakarot();
kakarot.next();
```
+ To pause a generator we use `yield` keyword. 
+ To execute the second block, (ie) the block after the `yield` keyword, we again call `next()`
+ We can have as many as `yield` inside the generator.
``` javascript
function* CallKakarot() {
    console.log('Super saiyan!');
    yield
    console.log('Super saiyan God!');
}

const kakarot = CallKakarot();
kakarot.next();
kakarot.next();
```
+ We can pass message from an instance of a generator to a generator and vice versa
+ Calling `next()` returns an object with properties `value` and `done`. `value` holds the passed data and `done` denotes whether the generator is done with the execution or not.


``` javascript
/* Pass from an instance to a generator */
function* CallKakarot() {
    const message = yield 
    console.log(`Message to Kakarot: ${message}`);
}

const kakarot = CallKakarot();
// { value: undefined, done: false }
console.log(kakarot.next());
// Message to Kakarot: A new threat is approaching!
// { value: undefined, done: true }
console.log(kakarot.next('A new threat is approaching!'));
```
``` javascript
/* Pass from a generator to an instance */
function* CallKakarot() {
    yield 'Kame hame ha!'
}

const kakarot = CallKakarot();
// { value: 'Kame hame ha!', done: false }
console.log(kakarot.next());
// { value: undefined, done: true }
console.log(kakarot.next());
```
+ Generators looks like synchronous code, but it is actually asynchronous. We can do multiple asynchronous work between the blocks before resuming. 
+ Because of `yield` keyword we can pause and resume at arbitary points, there are chances where arbitary amount of things may go wrong between different blocks.
+ If something goes wrong between the first and second `next()`, we can use `throw()` to throw an error inside the generator where it is paused.

``` javascript
function* CallKakarot() {
    const message = yield 
    console.log(`A message to Kakarot: ${message}`);
}

const kakarot = CallKakarot();
kakarot.next();
/* Our app will crash without a try-catch block */
//    const message = yield 
//                    ^
// undefined
kakarot.throw();
```
``` javascript
function* CallKakarot() {
    try {
        const message = yield 
        console.log(`A message to Kakarot: ${message}`);
    } catch(e) {
        console.log(`Error Message: ${e}`);
    }
}

const kakarot = CallKakarot();
kakarot.next();
// Error Message: Kakarot is not in Earth right now!
kakarot.throw('Kakarot is not in Earth right now!');
```
+ To get different values from generator, we can call `next()` multiple times until `done` is true or use a `while` to loop through. 
+ We can prevent the ugliness of calling the `next()` multiple times by using `for...of` loop in ES6. 
+ `for...of` loop pauses and resumes the generator dramatically and passes on the value yielded by the generator.

```javascript 
function* CallKakarot() {
    yield '1st try'
    yield '2nd try'
    yield '3rd try'
    yield '4th try'
}

const kakarot = CallKakarot();
// { value: '1st try', done: false }
console.log(kakarot.next());
// { value: '2nd try', done: false }
console.log(kakarot.next());
// { value: '3rd try', done: false }
console.log(kakarot.next());
// { value: '4th try', done: false }
console.log(kakarot.next());
// { value: undefined, done: true }
console.log(kakarot.next());
```
```javascript
function* CallKakarot() {
    yield '1st try'
    yield '2nd try'
    yield '3rd try'
    yield '4th try'
}

const kakarot = CallKakarot();
let next = kakarot.next();
// 1st try
// 2nd try
// 3rd try
// 4th try
while(!next.done) {
    console.log(next.value);
    next = kakarot.next();
}
```
```javascript
function* CallKakarot() {
    yield '1st try'
    yield '2nd try'
    yield '3rd try'
    yield '4th try'
}

const kakarot = CallKakarot();
for (let count of kakarot) {
    // 1st try
    // 2nd try
    // 3rd try
    // 4th try
    console.log(count);
}
```
+ `yield` keyword has something similar to the `function` keyword. If we use `yield* CallKakarotAgain()` along with a generator, the current generator (`CallKakarot()`) scope delegates its control to the other generator (`CallKakarotAgain()`).
+ We can even return a value from the delegated generator and receive it in the current generator.
```javascript
function* CallKakarot() {
    yield '1st try'
    yield '4th try'
}

function* CallKakarotAgain() {
    yield '2nd try'
    yield '3rd try'
}

for (let called of CallKakarot()) {
    // 1st try
    // 4th try
    console.log(called);
}
```
```javascript
function* CallKakarot() {
    yield '1st try'
    yield* CallKakarotAgain()
    yield '4th try'
}

function* CallKakarotAgain() {
    yield '2nd try'
    yield '3rd try'
}

for (let called of CallKakarot()) {
    // 1st try
    // 2nd try
    // 3rd try
    // 4th try
    console.log(called);
}
```
```javascript
function* CallKakarot() {
    yield '1st try'
    const tried = yield* CallKakarotAgain()
    console.log(tried)
    yield '4th try'
}

function* CallKakarotAgain() {
    yield '2nd try'
    return '3rd try'
}

for (let called of CallKakarot()) {
    // 1st try
    // 2nd try
    // 3rd try
    // 4th try
    console.log(called);
}
```
## Promises

+ Promises are used to manage callbacks. 
+ Promises act as an intermediary between our calling code and async code. 
+ Promises are not a replacement to callbacks but an alternate.
+ Promises are a special kind of object, which contains another object. 
+ Promises can be thought of as an event listener, upon which we can register to a specific task. When the task is completed, the event will be fired once. It is important to note that it will be fired only once.
+ We can chain promise for sequencing a series of async operations.
+ A promise can only be one of the following,  
  + **fulfilled** - Final value called fulfillment (- succeeded)
  + **rejected** - Final value called reason (reason for rejected) (- failed)
  + **pending** - (- neither fulfilled nor rejected)
  + **settled** - (- either fulfilled or rejected)
+ Promises can only be resolved once. Any further attempts will be ignored (value is immutable).
+ If a promise has been fulfilled or rejected and a success/failure callback is added later, the correct callback will be called, even though the async operation completed earlier.
+ Testing promises are easier, because we can use `setTimeout()` as an async task.
+ Its provides the following improvement over callbacks-only async, namely predictability, order and trustability.


```javascript
// Use Promise(..) constructor to construct a promise function
// The Promise(..) constructor should only be used for legacy async tasks (setTimeout / XMLHttpRequest)

var promise = new Promise((resolve, reject) => {

  if (isKakarot) {
    resolve(true); // Success
  } else {
    reject('Just a mortal!'); // Failure
  }

});

promise
.then(isSaiyanGod => { /* success */ })
.catch(msg => { /* failure */ });

// then(..) also takes two arguments, a success callback and a failure callback

promise.then((result) => {
  console.log(result); // "Alternate timeline is at peace!"
}, (err) => {
  console.log(err); // Error: "Black Goku must be destroyed!"
});

```

+ If we call resolve(..) and pass in another promise, current promise simply adopts the state (immediate/eventual) of the passed promise (fulfilled/rejected).

## Async & Await

