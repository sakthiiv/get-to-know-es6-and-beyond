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

