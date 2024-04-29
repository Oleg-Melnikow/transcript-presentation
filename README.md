# [Presentaion](https://oleg-melnikow.github.io/demo-presentation/)

### Preview

Hello everyone, in this video we talk about Event Loop, what is it, how does it work and why do we need it?

### JS synchronous

Javascript is a synchronous and single-threaded programming language, that is it executes the code frames synchronously and only one statement gets processed at a time.

To execute the code, we need a JavaScript Engine - a program that “reads and executes” what we wrote. <br>
Any JS engine always contains a call stack and a heap.

### Stack: Static memory allocation

A stack is a data structure that JavaScript uses to store static data. Static data is data where the engine knows the size at compile time. In JavaScript, this includes **primitive values** (_strings, numbers, booleans, undefined, and null_) and references, which point to objects and functions.

### Heap: Dynamic memory allocation

The heap is a different space for storing data where JavaScript stores **objects** and **functions**.
Unlike the stack, the engine **doesn't allocate a fixed amount of memory for these objects**. Instead, more space will be allocated as needed.

## References in JavaScript

All variables first point to the stack. In case it's a non-primitive value, the stack contains a reference to the object in the heap.
The memory of the heap is not ordered in any particular way, which is why we need to keep a reference to it in the stack. 
You can think of references as addresses and the objects in the heap as houses that these addresses belong to.

## The call stack

Chances are that you’ve already heard about something called “call stack”. This is not the same as the stack we previously discussed. As you know, stack is a place JavaScript uses to store variables assigned with primitive values. Call stack is something different.

The call stack in JavaScript is a mechanism used to keep track of the function calls in a program. Whenever a function is called in JavaScript, a new frame is created and pushed onto the call stack. This frame contains the function's arguments and local variables.

The call stack is a Last In, First Out (LIFO) data structure, which means that the most recently added frame is the first one to be removed. When a function finishes executing, its frame is popped off the top of the stack, and program execution resumes from the previous point on the stack.

### Web API

But the engine alone isn't enough. In order to work properly, we need Web APIs.

JS runtimes, especially in the context of web browsers, come with Web APIs that provide additional functionalities beyond the core JavaScript language. These APIs include interactions with the Document Object Model (DOM), XMLHttpRequest (for making HTTP requests), timers, and more.

Web APIs extend the capabilities of JavaScript, enabling it to interact with the browser environment and handle tasks like manipulating the webpage structure, handling user events, and making network requests.

So, basically Web APIs are functionalities that are provided to the engine but they are not a part of the JavaScript language itself. JavaScript gets access to these APIs through the window object.

Asynchronous operations in JavaScript, such as handling user input or making network requests, utilize callback functions. These functions are placed in a queue known as the callback queue, awaiting execution. The callback queue ensures that asynchronous tasks are handled in an organized manner.

### Microtasks and (Macro)tasks in Event Loop

Within the Event Loop, there are actually 2 type of queues: the (macro)task queue (or just called the task queue), and the microtask queue. The (macro)task queue is for (macro)tasks and the microtask queue is for microtasks.

Explanation
When a Promise resolves and calls its then(), catch() or finally(), method, the callback within the method gets added to the microtask queue! This means that the callback within the then(), catch() or finally() method isn’t executed immediately, essentially adding some async behavior to our JavaScript code!

Here the event loop gives a different priority to the tasks:

All functions in that are currently in the call stack get executed. When they returned a value, they get popped off the stack.

When the call stack is empty, all queued up microtasks are popped onto the callstack one by one, and get executed! (Microtasks themselves can also return new microtasks, effectively creating an infinite microtask loop)

If both the call stack and microtask queue are empty, the event loop checks if there are tasks left on the (macro)task queue. The tasks get popped onto the callstack, executed, and popped off!
