## Run to completion

Run-to-completion scheduling is a scheduling model in which each task runs until it either finishes, or explicitly yields control back to the scheduler.
<br><br>

* A core part of Javascript, describes how functions/events are processed by the event loop
* Guarantees a task will run without interuption or until it surrenders control
* return and throw are control flow primitives that will surrender control

Note:

- Again, Run to completion is the name for a concept you are familiar with
- Code blocks will always run without interuption from external methods
- Or until they give up control themselves.
- Thats fancy talk for if there is a return keyword, the function will stop and return a value. The rest of the function will not process 

--

## Run to completion and Generators

Generators introduces the yield keyword that allows functions to surrender their current control, but continue on from that point sometime in the future.
<br><br>
Effectively "pausing" a function mid-execution

Note:

- Generators introduce a unique control flow mechicism though the yield keyword
- What yield does, it it will surrender control of a function and return a value - like return
- But it will preserve the function state.
- Think of it like pausing a function.
- What is unique about generators is that they are then able to unpause that function and continue where they left off
- Confused? Example

--

## Comparision between a function and a generator

<div class="split-code"><pre>
```
// Function returning an iterable result

function count(n){
 // Needs to build a set of results.
 var results = [];
 for(var i = 0; i < n; i++){
    results.push(i);
 }
 return results;
}

for(var x of count(5)){
  console.log(x);
}

// 1
// 2 
// 3
// 4
// 5
```
</pre></div>


<div class="split-code"><pre>
```
// Using a generator

function* count(n){
  // yield pauses the processing
  // and returns a result
  // the generator can be re-entered
  for(var i = 0; i < n; i++){
    yield i + 1;
  }
}

for(var x of count(5)){
  console.log(x);
}

// 1
// 2 
// 3
// 4
// 5
```
</pre></div>

Note:

- Talk about function, and its control flow
- Talk about the generator and its control flow

- Wait!? How does the generator work in that for loop?
