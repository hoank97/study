# The event loop

Javascript has a runtime model based on a **event loop**, which is responsible for executing the code, collecting and processing events, and executing queued sub-tasks.

## Runtime concepts

### Visual representation

<div align="center">
  <img src="https://topdev.vn/blog/wp-content/uploads/2018/12/event-loop-2.png" />
</div>

### Stack

Function calls from a stack of frames.

```js
function foo(b) {
  const a = 10;
  return a + b + 11;
}

function bar(x) {
  const y = 3;
  return foo(x * y);
}

const baz = bar(7); // assigns 42 to baz
```

Order of operations:

1. When calling `bar`, a first frame is created containing references to `bar`'s arguments and local variables.
2. When `bar` calls `foo`, a second frame is created and pushed on top of the first one, containing references to `foo`'s arguments and local variables.
3. When `foo` returns, the top frame element is popped out of the stack (leaving only `bar` 's call frame).
4. When `bar` returns, the stack is empty.

**NOTE**: the arguments and local variables may continue to exist, as they are stored outside the stack - so they can be accessed by any **nested functions** long after their outer function has returned

### Heap

This is where all the memory allocation happens for your variables, that you have defined in your program

### Callback Queue

This is where your asynchronous code gets pushed to, and waits for the execution.

#### 1. Job Queue (Micro-task Queue)

Apart from Callback Queue, reserved only for `new Promise()` functionality. So when you use promises in your code, you add `.then()` method, which is a callback method. These `thenable` method are added to Job Queue once the promise has returned or resolved.

#### 2. Task Queue (Macro-task Queue)

These tasks are usually higher-level and take longer to complete. Examples include setTimeout, setInterval, DOM manipulation, and network requests.
