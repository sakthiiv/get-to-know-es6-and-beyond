# get-to-know-es6-and-beyond
This repository is a self reference readme guide to ES6.

## Table of Contents

* [Promises](#promises)
* [Async & Await](#async--await)

## Promises

+ Promises are used to manage callbacks. 
+ Promises act as an intermediary between our calling code and async code. 
+ Promises are not a replacement to callbacks but an alternate.
+ Promises are a special kind of object, which contains another object. 
+ Promises can be thought of as an event listener, upon which we can register to a specific task. When the task is completed, the event will be fired once. It is important to note that it will be fired only once.
+ We can chain promise for sequencing a series of async operations.
+ A promise can only have one of the two resolutions (resolved),  
  + **fulfilled** - Final value called fulfillment
  + **rejected** - Final valued called reason (reason for rejected)
+ Promises can only be resolved once. Any further attempts will be ignored (value is immutable).
+ Testing promises are easier, because we can use `setTimeout()` as an async task.
+ Its provides the following improvement over callbacks-only async, namely predictability, order and trustability.

```javascript
// Use Promise(..) constructor to construct a promise function
// The Promise(..) constructor should only be used for legacy async tasks (setTimeout / XMLHttpRequest)

var promise = new Promise(function (resolve, reject) {

  if (isKakarot) {
    resolve(true); // Success
  } else {
    reject('Just a mortal!'); // Failure
  }

});

promise
.then(isSaiyanGod => { /* success */ })
.catch(msg => { /* failure */ });

```

## Async & Await

