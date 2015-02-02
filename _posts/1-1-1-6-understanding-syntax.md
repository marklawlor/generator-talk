## Generators syntax in detail

--

Generators can only be invoked though the iterator they return.

```javascript
var generatorFunction,
    iterator;

generatorFunction = function * () {
    // This does not get executed.
    console.log('a');
};
console.log(1);
iterator = generatorFunction();
console.log(2);
 
// 1
// 2

```

--

Walking the iterator allows the generator function to be processed

```
var generatorFunction,
    iterator;
 
generatorFunction = function * () {
    console.log('a');
};
console.log(1);
iterator = generatorFunction();
console.log(2);
iterator.next();
console.log(3);
 
// 1
// 2
// a
// 3
```

--

The next() function will provide you information about the generator

```javascript
var generatorFunction,
    iterator;
 
generatorFunction = function * () {};
iterator = generatorFunction();
 
console.log(iterator.next());
 
// Object {value: undefined, done: true}
```

```javascript
var generatorFunction,
    iterator;
 
generatorFunction = function * () {
    yield;
};
iterator = generatorFunction();
 
console.log(iterator.next());
console.log(iterator.next());
 
// Object {value: undefined, done: false}
// Object {value: undefined, done: true}
```          

--

Generators do not block the event queue while suspended

```javascript
var generatorFunction,
    iterator;
 
generatorFunction = function * () {
    var i = 0;
    while (true) {
        yield i++;
    }
};
 
iterator = generatorFunction();
 
console.log(iterator.next());
console.log(iterator.next());
console.log(iterator.next());
 
// Object {value: 0, done: false}
// Object {value: 1, done: false}
// Object {value: 2, done: false}
```

--

Yield can return a value

```
var generatorFunction,
    iterator;
 
generatorFunction = function * () {
    yield 'foo';
};
iterator = generatorFunction();
 
console.log(iterator.next());
console.log(iterator.next());
 
// Object {value: "foo", done: false}
// Object {value: undefined, done: true}
```

--

Normal 'run-to-completion' statements still apply.

```
var generatorFunction,
    iterator;
 
generatorFunction = function * () {
    return 'bar';
    console.log("a"); // Will not be printed
    yield 'foo';
};
iterator = generatorFunction();
 
console.log(iterator.next());
console.log(iterator.next());
 
// Object {value: "bar", done: true}
// Object {value: undefined, done: true}
```

--

Yield can recieve values from the iterator

```
var generatorFunction,
    iterator;
 
generatorFunction = function * () {
    console.log(yield);
};
 
iterator = generatorFunction();
 
iterator.next('foo');
iterator.next('bar');
 
// bar
```

foo is not printed, as yield halts the processing. When next() is called again, the value of yield is 'bar' which is passed to console.log

--

Generators can delegate control to other generators via yield *

```
var foo, bar, baz, i;
 
foo = function * () {
    yield 'foo';
    yield * bar();
};
 
bar = function * () {
    yield 'bar';
};
 
for (i of foo()) {
    console.log(i);
}
 
// foo
// bar
```

--

They can also catch and throw errors

```
var generatorFunction,
    iterator,
 
generatorFunction = function * () {
    while (true) {
        try {
            yield;
        } catch (e) {
            if (e != 'a') {
                throw e;
            }
            console.log('Generator caught', e);
        }
    }
};
 
iterator = generatorFunction();
 
iterator.next();
 
try {
    iterator.throw('a');
    iterator.throw('b');
} catch (e) {
    console.log('Uncaught', e);
}
 
// Generator caught a
// Uncaught b
```
