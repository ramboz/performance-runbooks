# SI

## Definition

The Speed Index (SI) metric measures how quickly content is visually displayed during page load and how long it takes for the viewport to completely render.

## Components

- [TTFB](./ttfb.md)
- [TBT](./tbt.md)
- [FCP](./fcp.md)

## Value Range

| SI                | Mobile   | Desktop  |
|-------------------|----------|----------|
| Good              | < 3.4s   | < 1.3s   |
| Needs Improvement | 3.4-5.8s | 1.3-2.3s |
| Poor              | > 5.8s   | > 2.3s   |

## Most common issues

- Bad [TTFB](./ttfb.md), [TBT](./tbt.md) and/or [FCP](./fcp.md)
- Content visible in the initial viewport is loaded late
- Consent popups, ads or other async elements are loaded late inside the initial viewport

## Most common optimizations

- Follow the Bad [TTFB](./ttfb.md), [TBT](./tbt.md) and [FCP](./fcp.md) optimizations
- [Minimize main thread work](https://developer.chrome.com/docs/lighthouse/performance/mainthread-work-breakdown)
- Prioritize the rendering of the content in the initial viewport

## How to measure

Follow the steps in one of:
- the [Speed Index](https://developer.chrome.com/docs/lighthouse/performance/speed-index) in the Chrome DevTools Lighthouse or PageSpeed Insights report
- the [Speed Index](https://docs.webpagetest.org/metrics/speedindex/) metric in the [webpagetest.org]() site perfromance audit

## How to debug

The speed index is not a metric you can directly debug in the browser, but it is directly impacted by the following metrics:
- [TTFB](./ttfb.md)
- [TBT](./tbt.md)
- [FCP](./fcp.md)

## References

- https://developer.chrome.com/docs/lighthouse/performance/speed-index
- https://www.debugbear.com/docs/metrics/speed-index