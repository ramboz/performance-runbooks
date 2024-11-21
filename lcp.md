# LCP

## Definition

LCP (_largest contentful paint_) measures the time from when the page starts loading to when the largest text block.

## Components

  - [Server response time](./ttfb.md)
  - Resource load delay
  - Resource load duration
  - Element render delay
  - [Critical rendering path](https://web.dev/learn/performance/understanding-the-critical-path)

## Value Range

| LCP               | Mobile   | Desktop  |
|-------------------|----------|----------|
| Good              |    <2.5s |   <1.2s  |
| Needs Improvement | 2.5-4.0s | 1.2-2.4s |
| Poor              |    >3.0s |   >2.4s  |
| Theoretical min   |     0.6s |    160ms |

## Most common issues

- Bad [TTFB](./ttfb.md)
- Bad [FCP](./fcp.md)
- Render-blocking resources in the critical path that delay the LCP resource loading
- Unnecessary resources in the critical path that delay the LCP resource loading
- Non-optimized custom fonts

## Most common optimizations

- Follow the [TTFB](./ttfb.md) optimizations
- Follow the [LCP](./lcp.md) optimizations
- Include the LCP element in the initial HTML document
- Try to have the LCP element being the same as the LCP one
- Leverage modern media formats (webp, avif, webm) and optimize for the viewport (responsive image)
  - if the LCP element is a video, make sure to use a poster image
  - if the LCP element is an image, make sure it is `loading` as `eager` and its `fetchpriority` is set to `high`
- Use speculative pre-rendering

## How to measure

Follow the steps in one of:
- [LCP in our performance audit](./performance-audit.md#fcplcp) article
- the [LCP event](https://developer.chrome.com/docs/devtools/performance/reference#timings) in the Chrome DevTools performance audit view timings
- the [largest contentful paint](https://docs.webpagetest.org/getting-started/#largest-contentful-paint) metric in the [webpagetest.org]() site perfromance audit

## References

- https://web.dev/articles/lcp
- https://web.dev/articles/optimize-lcp
- https://www.debugbear.com/docs/metrics/largest-contentful-paint