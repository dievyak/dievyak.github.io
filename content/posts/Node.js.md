+++
date = '2026-02-28T15:01:00+01:00'
draft = false
title = 'Node.js'
+++

Synchronous vs Asynchronous in Node.js:

When we talk about synchronous code in Node.js, we mean code that runs step by step, in order. Each line must finish before the next one can start. If a function takes a long time (for example, reading a large file), the rest of the program has to wait until that function completes. That’s why synchronous code is often called blocking.

Asynchronous code, on the other hand, does not wait for a long‑running operation to finish. Instead, Node.js starts the operation (like reading a file, making a network request, or querying a database) and then immediately moves on to the next task. When the operation is done, Node.js “comes back” with the result via a callback, promise, or async/await. This is why asynchronous code is called non‑blocking.

Because Node.js is single‑threaded, non‑blocking I/O is a huge advantage: the event loop can continue serving other requests while waiting for I/O, which makes Node.js very good at handling many concurrent connections.

What Are Promises?

Promises provide a cleaner way to work with asynchronous code. A Promise represents a value that will be available in the future (or an error that might happen). It can be in one of three states:

Pending: the async operation is still running.

Fulfilled: the operation finished successfully and a result is available.

Rejected: the operation failed and an error is available.

Instead of nesting callbacks, you attach handlers using .then() for success and .catch() for errors. This leads to more readable and maintainable code:

myPromise

```javascript
  .then((result) => {
    console.log("Promise fulfilled with:", result);
  })
  .catch((error) => {
    console.error("Promise rejected with:", error);
  });
```


Why Promises Are Powerful:

Simplified error handling
Errors thrown anywhere in a promise chain can be caught in a single .catch(), instead of passing error objects manually through nested callbacks.

Chaining
Promises allow you to express a sequence of async steps in a linear way:

```javascript
fetchData()
  .then(processData)
  .then(displayResults)
  .catch(handleError);
```


Running tasks in parallel with Promise.all:

You can run multiple async operations at the same time and wait for all of them to finish:

```javascript
const tasks = [fetchData1(), fetchData2(), fetchData3()];

Promise.all(tasks)
  .then((results) => {
    // results is an array of all resolved values
  })
  .catch(handleError);
```
  

Drawbacks of Promises:

Error flows can still become tricky in very long or branching promise chains.

Promises don’t support cancellation out of the box. Once started, they usually run to completion, which can waste resources in some cases.

First‑Class Functions in JavaScript:

In JavaScript, functions are first‑class citizens. That means you can treat them like any other value:

Assign them to variables

Pass them as arguments to other functions

Return them from functions

Examples:

// a) Assigning a function to a variable

```javascript
const greet = function () {
  return "Hello World";
};
console.log(greet());

```

// b) Passing a function as an argument

```javascript
function callFunction(fn) {
  console.log(fn());
}
function sayHi() {
  return "Hi";
}
callFunction(sayHi);
```


// c) Returning a function from another function

```javascript
function multiplier(factor) {
  return function (num) {
    return num * factor;
  };
}
const double = multiplier(2);
console.log(double(5)); // 10
```

This property is the basis of higher‑order functions like map, filter, and reduce.

Why Node.js Is Often Preferred:

Node.js has become a popular choice for building APIs and backend services for several reasons:

Non‑blocking I/O model: Node’s event‑driven, non‑blocking architecture allows it to handle many concurrent requests with good performance and low overhead, without manually managing threads.

V8 engine: Node runs on Google’s V8 JavaScript engine (written in C++), which compiles JavaScript to fast machine code and is continuously optimized.

Same language everywhere: Using JavaScript on both frontend and backend simplifies full‑stack development, code sharing, and collaboration.

Rich ecosystem: npm offers a huge collection of packages for almost every use case you can think of.

Control Flow in JavaScript and Node.js
Control flow describes the order in which your JavaScript code runs. In Node.js, control flow tools help you:

Decide which function runs next and in what order.

Collect results from multiple async operations.

Limit concurrency, so you don’t overload external systems.

Structure complex flows into clear, readable steps.

Because JavaScript is single‑threaded, you control concurrency by deciding how many async operations you start at once and how you handle their completion.

Common Timing Functions in Node.js
Node.js provides several timing utilities that interact with the event loop:

setTimeout / clearTimeout: run a function once after a delay.

setInterval / clearInterval: run a function repeatedly at a fixed interval.

setImmediate / clearImmediate: schedule a callback to run on the next iteration of the event loop, after I/O events.

process.nextTick: schedule a callback to run before the next event loop tick, making it very high priority.

These tools give you precise control over when certain pieces of code are executed.

Callbacks and Callback Hell
A callback is simply a function you pass to another function to be executed later, usually after an asynchronous operation finishes.

Callback Hell happens when you nest many callbacks inside each other, for example:

```javascript
doSomething(() => {
  doSomethingElse(() => {
    doThirdThing(() => {
      // and so on...
    });
  });
});
```

This pyramid‑shaped code is hard to read, debug, and maintain. Promises and async/await are the modern ways to avoid callback hell and write async code that looks more like synchronous code.

Why Promises Are Better Than Plain Callbacks
Using promises instead of raw callbacks has clear benefits:

More readable, linear style of async code (especially with async/await).

Centralized error handling with .catch() rather than checking and propagating errors at every level.

Built‑in helpers (Promise.all, Promise.race, etc.) for advanced workflows like parallel execution and “first result wins”.

What Is a Stub?

A stub is a fake implementation of a function used in tests. Instead of calling real services (like a database, email service, or external API), you:

Control exactly what data it returns.

Avoid side effects and slow operations.

Inspect how your code calls it (arguments, call count, etc.).

For example, if you have a function that reads from a database, in tests you can stub the DB function to return a fixed object. This keeps tests fast, deterministic, and independent of external systems.

Node.js Process Exit Codes (Short Overview):

When a Node.js process exits, it returns an exit code to the operating system. Some common ones:

Code 1 – Uncaught fatal exception: an error wasn’t handled anywhere and crashed the app.

Code 2 – Reserved/incorrect usage: often used by shells for misuse of built‑in commands; not typically used by Node core.

Code 4 – Internal JavaScript evaluation failure: Node couldn’t evaluate some internal script (rare; usually indicates a corrupted install or environment).

Code 5 – Fatal error (V8 initialization failed): the V8 engine failed to start, for example due to invalid flags or severe resource constraints.

Code 7 – Internal exception handler failure: even Node’s internal error handling failed while trying to manage another error.

Knowing these helps diagnose why your app exited unexpectedly.


Why Node Uses the V8 Engine:

There are several JavaScript engines: SpiderMonkey (Firefox), Chakra (older Edge), etc. Node.js chose Google’s V8 because:

It’s open‑source and very actively maintained.

It’s written in C++, optimized for speed, and compiles JS to machine code.

It’s battle‑tested by Chrome’s massive user base and supports modern JavaScript features and WebAssembly.

This combination makes V8 a performant and reliable foundation for server‑side JavaScript.

Separating Express App and Server
A common Express pattern is to separate your code into:

An app module: defines routes, middleware, and business logic.

A server file: imports the app and calls app.listen().

This separation makes your project easier to test, maintain, and scale. For example, you can import the app into test files without actually starting an HTTP server, or run the same app on different ports/environments.

Reactor Pattern in Node.js
The Reactor pattern is a design pattern used to handle non‑blocking I/O in event‑driven systems like Node.js.

It consists of:

A Reactor: listens for I/O events (like incoming connections or data) and dispatches them to the right handlers.

Handlers: functions or objects that perform the actual work when an event occurs.

Node’s event loop, combined with its callbacks and event emitters, is a practical implementation of the Reactor pattern.

What Is Middleware?

In Express.js, middleware is a function that runs between the incoming request and the final response. It can:

Inspect or modify the request (req) or response (res).

Perform tasks like authentication, logging, body parsing, or error handling.

Decide whether to pass control to the next middleware or end the response.

You can think of middleware like checkpoints in an airport: security, immigration, boarding, etc. Each step can approve you, modify something (e.g., check luggage), or stop you.

Types of Middleware:

Application‑level middleware: attached directly to the Express app instance (app.use(...)). Runs for all or specific routes.

Router‑level middleware: attached to an express.Router() instance, useful for grouping related routes (like /users, /admin).

Built‑in middleware: provided by Express, such as express.json(), express.urlencoded(), and express.static().

Error‑handling middleware: functions with four parameters (err, req, res, next) that catch and handle errors.

Third‑party middleware: installed from npm, like morgan, cors, cookie-parser, etc.

Buffers in Node.js

A Buffer is a Node.js data structure used to handle raw binary data. It’s especially useful for operations like:

Reading and writing files.

Working with network sockets.

Handling streams (e.g., video or audio data).

Key points:

Buffers have a fixed size once allocated.

They act as temporary storage while data is being transferred.

Many Node core APIs (like the fs and net modules) use buffers under the hood.

Streams in Node.js
Streams are objects that let you read or write data piece by piece, instead of loading everything into memory at once. They are built on top of Node’s EventEmitter.

Main stream types:

Readable: you can read data from them (fs.createReadStream).

Writable: you can write data to them (fs.createWriteStream).

Duplex: both readable and writable (net.Socket).

Transform: like duplex streams but can modify data as it passes through (zlib.createDeflate()).

Streams are ideal for handling large files or data pipelines efficiently.

Example: Sum Using a Promise
Here’s a simple example that wraps a calculation in a promise:

```javascript
function sum(a, b) {
  return new Promise((resolve, reject) => {
    if (typeof a !== "number" || typeof b !== "number") {
      reject("Both arguments must be numbers");
    } else {
      resolve(a + b);
    }
  });
}

sum(5, 10)
  .then((result) => {
    console.log("Sum is:", result);
  })
  .catch((error) => {
    console.error("Error:", error);
  });
```

What Is Multer?

Multer is an Express middleware for handling multipart/form-data, which is the format used for file uploads in forms.

Example setup:

```javascript
const express = require("express");
const multer = require("multer");
const app = express();

const storage = multer.diskStorage({
  destination(req, file, cb) {
    cb(null, "uploads/");
  },
  filename(req, file, cb) {
    cb(null, Date.now() + "-" + file.originalname);
  },
});

const upload = multer({ storage });
```

// Handle single file upload

```javascript
app.post("/upload", upload.single("myFile"), (req, res) => {
  res.send("File uploaded successfully!");
});

app.listen(3000, () => {
  console.log("Server started on port 3000");
});
```

Streams vs Buffers (Conceptual Difference):

Buffer: holds a chunk of data in memory; good for relatively small, complete pieces of binary data.

Stream: processes data in smaller chunks over time; better for large or continuous data, reducing memory usage.

Streams often use buffers internally, but give you a higher‑level, event‑driven interface.

Child Processes in Node.js
Node.js is single‑threaded, but sometimes you need to offload heavy work or run external commands. That’s where child processes come in: they let you run another program or Node script from your Node app.

Main methods:

exec(): runs a command in a shell and buffers the entire output in memory (good for short results).

execFile(): runs a specific executable directly, without a shell.

spawn(): starts a process and returns streams for stdin/stdout/stderr (good for streaming large output).

fork(): a special case of spawn() optimized for running another Node.js module.

Deadlocks in Node.js

A deadlock is a situation where two or more operations are waiting on each other and none can proceed. In Node.js, this can happen if multiple resources or locks are involved and the code is structured in such a way that each part is waiting for the other. The result is a hung or frozen system.

OAuth in Simple Terms

OAuth (Open Authorization) is a framework that lets one application access data from another service on a user’s behalf, without needing the user’s password.

Typical flow:

User clicks “Login with Google” (or similar).

They’re redirected to Google to log in and approve permissions.

Google sends the app an access token.

The app uses the token to access the user’s data securely.

Event Loop Starvation

Event loop starvation happens when long‑running or CPU‑intensive tasks block the event loop so much that it can’t process new events quickly. Symptoms include:

Slow or delayed responses.

Timers firing late.

The server feeling “stuck” even though it’s technically running.

To avoid it, move heavy computations off the main thread (for example using child processes or worker threads) and keep event loop tasks small.

Promise.all, Promise.race, Promise.allSettled, Promise.any
Promise.all([p1, p2, ...]): waits for all promises to fulfill; rejects immediately if any one fails.

Promise.race([p1, p2, ...]): settles as soon as the first promise settles (resolve or reject).

Promise.allSettled([p1, p2, ...]): waits for all promises to settle and returns an array with each promise’s status and value/reason.

Promise.any([p1, p2, ...]): resolves with the first fulfilled promise; rejects with an AggregateError if all of them reject.

body‑parser
body-parser is a middleware (now largely integrated into Express) used to parse the body of incoming requests. It makes form data or JSON payloads available as req.body, so you don’t have to manually read and parse the raw request stream.

CORS (Cross‑Origin Resource Sharing)
Browsers block requests made from one origin (domain + protocol + port) to another by default for security reasons. CORS is a mechanism that lets servers explicitly say which other origins are allowed to access their resources.

Example scenario:

Frontend at http://localhost:3000

Backend API at http://localhost:5000

Without CORS configured on the backend, the browser will block the frontend’s requests and show a CORS error. Enabling CORS on the server tells the browser it’s safe to allow those cross‑origin calls.

Redis in a Nutshell

Redis (Remote Dictionary Server) is an in‑memory key–value data store, known for being extremely fast. Common uses:

Caching frequently accessed data.

Storing user sessions.

Implementing queues and pub/sub messaging.

Real‑time analytics and leaderboards.

In Node.js, you use a Redis client (like redis or ioredis) to connect to a Redis server and run commands like SET, GET, DEL, PUBLISH, etc.

DNS Module in Node.js

The Node.js dns module gives you programmatic access to DNS lookups. It lets you translate domain names (like example.com) into IP addresses and vice versa, among other DNS operations. This is useful when building network tools or when you need fine‑grained control over how hostnames are resolved.

Improving Database Query Performance

Some practical tips to speed up database queries:

Use indexes on columns that you search, filter, or join on frequently.

Avoid SELECT *; fetch only the columns you actually need.

Apply WHERE filters to reduce the number of rows scanned.

Use batch operations instead of looping many single inserts/updates.

Partition very large tables (by date, region, etc.) when appropriate.

Avoid unnecessary ORDER BY or complex sorts unless they’re really needed.

Indexing and Its Limitations

An index is like a lookup table that lets the database find rows much faster than scanning the entire table.

Trade‑offs:

Every INSERT, UPDATE, and DELETE must also update the index, which slows write operations.

Indexes consume extra disk space.

They can become fragmented over time and need maintenance.

Small tables often don’t benefit much from indexing.

Bulk writes are slower when many indexes need to be updated.

