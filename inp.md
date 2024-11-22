# INP

## Definition

The Interaction to Next Paint (INP) metric measures user interface responsiveness – how quickly a website responds to user interactions like clicks or key presses.

## Components

- Input delay: waiting for background tasks on the page that prevent the event handler from running
- Processing time: running event handlers in JavaScript
- Presentation delay: handling other queued up interactions, recalculating the page layout, and painting page content

## Value Range

| INP               | Mobile/ Desktop  |
|-------------------|------------------|
| Good              | < 200ms          |
| Needs Improvement | 200-500ms        |
| Poor              | > 500ms          |


## Most common issues

- Long tasks running in the background delaying execution of event handlers
- 3rd-party scripts tracking user interactions (i.e. analytics and conversion tracking tags from the martech stack)
- Expensive logic in the event handlers
- Complex UI updates

## Most common optimizations

- Reduce and break up background activity on the main thread
   - Follow the [TBT](./tbT.md) optimizations
- Avoid unnecessary re-rendering of your components (especially in the case of React)
- [Reduce relayouts](https://www.debugbear.com/blog/front-end-javascript-performance#avoid-dom-access-that-requires-layout-work) due to interactions.
  - [What forces layout/reflow. The comprehensive list.](https://gist.github.com/paulirish/5d52fb081b3570c81e3a)
- Offset complex calculations to a web worker, or at least show a UI update before starting it (i.e. open a dialog with a spinning wheel) and perform further updates asynchronously
- Cache complex computation or UI updates with [memoization](https://www.debugbear.com/blog/front-end-javascript-performance#memoization) techniques where appropriate
- Defer UI updates outside of the current viewport
- [Throttle or debounce](https://www.debugbear.com/blog/front-end-javascript-performance#event-listeners) repeated events firing on a short time-frame

## How to measure

### Manually
```js
performance.getEntriesByType('long-animation-frame');
```

### Using web-vitals.js

```js
import { onINP } from 'web-vitals';

// Measure and log INP in all situations
// where it needs to be reported.
onINP(console.log);
```

## How to debug

Follow the steps in one of:
- [TBT in our performance audit](./performance-audit.md#tbt) article
- the [Interaction events](https://developer.chrome.com/docs/devtools/performance/reference#interactions) in the Chrome DevTools performance audit panel
- The DebugBear [INP Debugger](https://www.debugbear.com/inp-debugger)

## References

- https://web.dev/articles/inp
- https://web.dev/articles/optimize-inp
- https://www.debugbear.com/docs/metrics/interaction-to-next-paint
