## Iterators

 In computer programming, an iterator is an object that enables a programmer to traverse a container.
 <br><br>

* Iterators are associated with a container, not part of the container
* Provides a common interface, eg next()
* Native control flow primatives understand iterators (such as for..of) 

--


Generators look like functions, but you can only interact with them though iterator.

Calling a generator function will provide you with an iterator to interact with the generator

--


## Controlling a generator though its iterator


<div class="split-code"><pre>
```
/** Using a function */

function count(n){
 var results = [];
 for(var i = 0; i < n; i++){
    results.push(i);
 }
 return results;
}

// External code cannot manage the
// control flow of count()
for(var x of count(5)){
  console.log(x);
}

```
</div>


<div class="split-code"><pre>
```
/** Using a generator */

function* count(n){
  for(var i = 0; i < n; i++){
    yield i + 1;
  }
}

// Generators abstract the 
// internal control flow of count()
// It to be externally controlled
var countIterator = new count(5);

console.log(countIterator.next()); // 1
console.log(countIterator.next()); // 2
console.log(countIterator.next()); // 3
console.log(countIterator.next()); // 4
console.log(countIterator.next()); // 5
```
</div>
