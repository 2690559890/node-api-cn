
<!--introduced_in=v8.5.0-->

> Stability: 2 - Stable

The Performance Timing API provides an implementation of the
[W3C Performance Timeline][] specification. The purpose of the API
is to support collection of high resolution performance metrics.
This is the same Performance API as implemented in modern Web browsers.

```js
const { PerformanceObserver, performance } = require('perf_hooks');

const obs = new PerformanceObserver((items) => {
  console.log(items.getEntries()[0].duration);
  performance.clearMarks();
});
obs.observe({ entryTypes: ['measure'] });
performance.measure('Start to Now');

performance.mark('A');
doSomeLongRunningProcess(() => {
  performance.measure('A to Now', 'A');

  performance.mark('B');
  performance.measure('A to B', 'A', 'B');
});
```

