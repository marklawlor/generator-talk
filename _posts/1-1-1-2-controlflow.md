## Control Flow

Control flow refers to the order in which the individual statements, instructions or function calls of an program are executed or evaluated.

Languages include control flow primitives such as if, do, for, etc...

Note: 

- What is control flow? Well you already know
- The If and loop keyworks are example of javascript control flow primitives

--

<pre><div class="code"><span class="comment">/&#42;&#42;  
  Runs tasks A, B, C in sequence.
 &#42;/</span>
<span class="function"><span class="keyword">function</span> <span class="title">myFunction</span><span class="params container"><span class="fragment fade-out" data-fragment-index="0">(){</span><span class="fragment fade-in" data-fragment-index="0">(configPath){</span></span></span>
  <span class="container">
  <span class="fragment fade-out" data-fragment-index="0">taskA();
taskB();
taskC();
</span>
  <span class="fragment fade-in" data-fragment-index="0">var config = readFileAsync(configPath) // Task A
var response = makeRequestToServer(config); // Task B
return processResponse(response); // Task C
</span>
}
</div></pre>

Function can be 'fixed' by adding control flow via callbacks & promises {% fragment %}

Adding logic can bloat code and reduces reusability {% fragment %}

Note:

Control flow is important for logic
It can reduce readability and reusability


--

## You can seperate control flow from logic

```
/**
  Runs tasks A, B, C in sequence.
 */
var myFunction = completeAllTasks(function* (configPath) {
  var config = yield readFileAsync(configPath) // Task A
  var response = yield makeRequest(config); // Task B
  yield processResponse(response); // Task C
});

```

Compared to
{% fragment %}

```
/**
  Runs tasks A, B, C in sequence.
 */
var myFunction = function(configPath){
  var config = readFileAsync(configPath) // Task A
  var response = makeRequest(config); // Task B
  return processResponse(response); // Task C
}

``` 
{% fragment %}

Note: 

- Compare the two functions
- First function works fine, the control flow has just been abstracted out of the function
