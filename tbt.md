# TBT

## Definition

The Total Blocking Time (TBT) metric measures the total amount of time after [First Contentful Paint](./fcp.md) (FCP) where the main thread was blocked for long enough to prevent input responsiveness.

## Components

- [Long tasks](https://web.dev/articles/custom-metrics#long-tasks-api) that run for >50ms
- [Long animation frames](https://web.dev/articles/custom-metrics#long-tasks-api) that run for >50ms


## Value Range

| TBT               | Mobile    | Desktop   |
|-------------------|-----------|-----------|
| Good              | < 200ms   | < 150ms   |
| Needs Improvement | 200-600ms | 150-350ms |
| Poor              | > 600ms   | > 350ms   |

## Most common issues

- Complex JS logic at the project level, typically when iterating over a large dataset (like a list of products on a paginated page)
- Leveraging expensive JavaScript APIs, like the `Intl` formatting
- Use of non-optimized 3rd party libraries
- Use of Marketing Tag Managers (like Google Tag Manager, Adobe Launch, etc.)
- Social media or other iframe-like embeds

## Most common optimizations

- Delay long running tasks until after the page is loaded if possible
  - Defer the loading of tag managers (typically a good option for the MarTech)
  - Leverage the [`IntersectionObserver`](https://developer.mozilla.org/en-US/docs/Web/API/IntersectionObserver) API to run tasks when an element becomes visible in the viewport
- Break up long running tasks into smaller chunks
  - Leverage `async`/`await` and the [`Promise`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) API to refactor code logic from sync to async
  - Use the [`setTimeout`](https://developer.mozilla.org/en-US/docs/Web/API/Window/setTimeout) API to run high priority tasks
  - Use the [`queueMicrotask`](https://developer.mozilla.org/en-US/docs/Web/API/Window/queueMicrotask) API to run medium priority tasks (what `Promise.then()` does)
  - Use the [`requestIdleCallback`](https://developer.mozilla.org/en-US/docs/Web/API/Window/requestIdleCallback) API to run low priority tasks when the browser is idle between rendering frames
  - Use the [`requestAnimationFrame`](https://developer.mozilla.org/en-US/docs/Web/API/Window/requestAnimationFrame) API to run tasks that modify the DOM and require a new frame to render
  - Use the [`scheduler.yield`](https://developer.mozilla.org/en-US/docs/Web/API/Scheduler/yield) API to hand over control to the main thread during a long task, so it can execute other tasks in-between if needed
- [Offload complex background work to a web worker](https://web.dev/articles/off-main-thread)
- For `iframe`-like embeds, try do defer loading with any of these techniques:
  - load the `iframe` only when the user scrolls to the embed, by leveraging an the [`IntersectionObserver`](https://developer.mozilla.org/en-US/docs/Web/API/IntersectionObserver) API
  - use the [`loading="lazy"`](https://developer.mozilla.org/en-US/docs/Web/Performance/Lazy_loading#images_and_iframes) attributes
  - use a static [_facade_ as a placeholder](https://developer.chrome.com/docs/lighthouse/performance/third-party-facades) and load the `iframe` only when the user interacts with the placeholder (i.e. [lite youtube embed](https://github.com/paulirish/lite-youtube-embed), [lite tiktok](https://github.com/justinribeiro/lite-tiktok))


## How to measure

```js
let totalBlockingTime = 0;
new PerformanceObserver((entryList) => {
  for (const entry of entryList.getEntries()) {
    totalBlockingTime += entry.duration - 50;
  }
  console.log({ totalBlockingTime });
}).observe({ type: 'longtask', buffered: true });
```

## How to debug

Follow the steps in one of:
- [TBT in our performance audit](./performance-audit.md#tbt) article
- the [flame chart](https://developer.chrome.com/docs/devtools/performance/reference#timings) in the Chrome DevTools performance audit
- the [total blocking time](https://docs.webpagetest.org/getting-started/#total-blocking-time) metric in the [webpagetest.org]() site perfromance audit


## References

- https://web.dev/articles/tbt
- https://web.dev/articles/optimize-long-tasks
- https://www.debugbear.com/docs/metrics/total-blocking-time