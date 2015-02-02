##  Common Use Case

Asychronous code is a common use case where generators are used to seperate logic from control flow

--

## Promises

In computer science, a promises refers to constructs used for synchronization. 
They act as a proxy for a result that is initially unknown, usually because the computation of its value is yet incomplete.
<br><br>

* A common solution to avoiding "Inversion of Control"
* Part of ES6, but various implementations have been in use for a while
* Allows functions to retain Control Flow of external tasks

--

## Promises solve Inversion Of Control

(aka Callback hell)

--

###Common misconception

> Callbacks are bad because they create to much indentation

--

###Actual issue 

Callbacks create an inversion of control within a function

```
function myFunction(){
  // All code here, myFunction controls its own control flow

  someAsyncThing( function(){
    // everything else in here, someAsync has taken over control flow
  } );
}
```

--

### Inversion Of Control mismanaged

Inversion of control creates a trust layer in your code.

Your trusting that this function won't:

* Don’t call my callback too early
* Don’t call my callback too late
* Don’t call my callback too few times
* Don’t call my callback too many times
* Make sure to provide my callback with any necessary state/parameters
* Make sure to notify me if my callback fails in some way

--

## Promises + Generators = ♡

If each yield returns a promise, you can automatically process complex tasks.

--

With this information, we can use write a function to complete all tasks, based upon their state

```javascript
function completeAllTasks(generator){
  return function(){
    var iterator = generator.apply(this, arguments);

    function handle(result){ // Result = {done: [Boolean], value: [Object] }
      if(result.done) return result.value;

      return result.value.then(function(res){
        return handle(iterator.next(res));
      }, function(err){
       return handle(iterator.throw(err));
      });
    }
    return handle(iterator.next());
  };
}
```

--

```
var myFunction = completeAllTasks(function* (file) {
  var fileContents = yield taskA(file);
  var response = yield taskB(fileContents);
  yield processResponse(response);
});
```
