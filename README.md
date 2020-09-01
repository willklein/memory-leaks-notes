# Memory Leaks and the Havoc They Wreak

## Presentation

Video: coming soon
Slides: coming soon

## Notes

Confused about Stack and Heap? by Franziska Hinkelmann: https://medium.com/fhinkel/confused-about-stack-and-heap-2cf3e6adb771

JavaScript primitives: https://developer.mozilla.org/en-US/docs/Glossary/Primitive

JavaScript primitives: https://developer.mozilla.org/en-US/docs/Glossary/Primitive

JavaScript objects (stuff that leaks): https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures#Objects

Maximum call stack size exceeded example: https://github.com/christianalfoni/formsy-react/issues/298

Debugging example, closure:

```
  var run = function () {
    var str = new Array(1_000_000).join('*');

    var doSomethingWithStr = function () {
      if (str === 'something')
        console.log("str was something");
    };

    doSomethingWithStr();

    var logIt = function () {
      console.log('interval');
    }

    setInterval(logIt, 100);
  };

  setInterval(run, 1000);
```

David Glasser's article: http://point.davidglasser.net/2013/06/27/surprising-javascript-memory-leak.html


Caching example (lru-cache):
```javascript
// https://www.npmjs.com/package/lru-cache
var LRU = require("lru-cache"),

var cache = new LRU(50) // max size of 50

cache.set("MC", "Cassidy Williams")
cache.get("MC") // "Cassidy Williams"
```

Basic closure:

```javascript
var closureScope = { functions: 'get me' }

function getMessage(key) {
  return closureScope[key]
}
```

performance.measureMemory API

Proposal on GitHub: https://github.com/WICG/performance-measure-memory

Chrome Platform status: https://www.chromestatus.com/feature/5685965186138112

```javascript
if (performance.measureMemory) {
  let result;
  try {
    result = await performance.measureMemory(); // this is it
  } catch (error) {
    /* ... */
  }

  // you could send telemetry data to an API
  console.log(result);
}
```

Full quote from Ulan Degenbaev, from https://web.dev/monitor-total-page-memory-usage/

> Browsers manage the memory of web pages automatically. Whenever a web page creates an object, the browser allocates a chunk of memory "under the hood" to store the object. Since memory is a finite resource, the browser performs garbage collection to detect when an object is no longer needed and to free the underlying memory chunk. The detection is not perfect though, and it was proven that perfect detection is an impossible task. Therefore browsers approximate the notion of "an object is needed" with the notion of "an object is reachable". If the web page cannot reach an object via its variables and the fields of other reachable objects, then the browser can safely reclaim the object. The difference between these two notions leads to memory leaks

### Site Reliability Engineering

If you're building services on the backend, look into topics of observability and site reliability engineering. Our dev ops friends can definitely help with this but it’s something you can pick up as you go. For more background, check out the set of Site Reliability Engineering books from the folks at Google and published by O’Reilly here: https://landing.google.com/sre/books/