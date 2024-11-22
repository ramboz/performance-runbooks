# FCP

## Definition

The First Contentful Paint (FCP) metric measures the time from when the page starts loading to when any part of the page's content is rendered on the screen.

## Components

- [Server response time](./ttfb.md)
- [Critical rendering path](https://web.dev/learn/performance/understanding-the-critical-path)

## Value Range

| FCP               | Mobile   | Desktop  |
|-------------------|----------|----------|
| Good              |    <1.8s |   <0.9s  |
| Needs Improvement | 1.8-3.0s | 0.9-1.6s |
| Poor              |    >3.0s |   >1.6s  |
| Theoretical min   |     0.6s |    160ms |

## Most common issues

- Render-blocking resources in the critical path
- Bad [TTFB](./ttfb.md)
- Unnecessary resources in the critical path
- Non-optimized custom fonts

## Most common optimizations

- Follow the [TTFB](./ttfb.md) optimizations
- Include the FCP element in the initial HTML document
- Optimize the critical path and defer non-essential CSS & JS
  - [Minimize critical request depth](https://developer.chrome.com/docs/lighthouse/performance/critical-request-chains)
  - Parallelize dependency loading
  - [Keep request counts low and transfer sizes small](https://developer.chrome.com/docs/lighthouse/performance/resource-summary)
  - [Avoid enormous render-blocking payloads](https://developer.chrome.com/docs/lighthouse/performance/total-byte-weight)
- Use [preload](https://developer.chrome.com/docs/lighthouse/performance/uses-rel-preload)/[preconnect](https://developer.chrome.com/docs/lighthouse/performance/uses-rel-preconnect) headers or meta tags for required 3rd-party connections
- [Optimize custom fonts](https://developer.chrome.com/docs/lighthouse/performance/font-display)
- Minify large CSS/JS files

## How to measure

### Manually
```js
new PerformanceObserver((entryList) => {
  for (const entry of entryList.getEntriesByName('first-contentful-paint')) {
    console.log('FCP candidate:', entry.startTime, entry);
  }
}).observe({ type: 'paint', buffered: true });
```

### Using web-vitals.js

```js
import { onFCP } from 'web-vitals';

// Measure and log FCP as soon as it's available.
onFCP(console.log);
```

## How to debug

Follow the steps in one of:
- [FCP in our performance audit](./performance-audit.md#fcplcp) article
- the [FCP event](https://developer.chrome.com/docs/devtools/performance/reference#timings) in the Chrome DevTools performance audit view timings
- the [first contentful paint](https://docs.webpagetest.org/getting-started/#first-contentful-paint) metric in the [webpagetest.org]() site perfromance audit


## References
- https://web.dev/articles/fcp
- https://www.debugbear.com/docs/metrics/first-contentful-paint
