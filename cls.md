# CLS

## Definition

The Cummulative Layout Shift (CLS) metric measures the the largest burst of layout shift scores for every unexpected layout shift that occurs during the entire lifecycle of a page.

## Components

CLS is essentially composed of:
- [impact fraction of the total area of the viewport](https://web.dev/articles/cls#impact-fraction)
- [distance fraction of the viewport's largest dimension](https://web.dev/articles/cls#distance-fraction)

## Value Range

| CLS               | Mobile/Desktop |
|-------------------|----------------|
| Good              | < 0.1          |
| Needs Improvement | 0.1-0.25       |
| Poor              | > 0.25         |

## Most common issues

- Position of DOM elements is changed
- Dimension of DOM elements is changed
- DOM elements are inserted or removed
- CSS animations that trigger layout are running on the page
- CSS `position` of a DOM element
- Images, ads or social embeds are loaded without proper space being reserved
- Async content is rendering without proper space being reserved
- [Use of custom fonts with no proper fallback](https://www.debugbear.com/blog/web-font-layout-shift)
- Loading async content that causes the scrollbar to appear lazily

## Most common optimizations

- Insert/re-order/remove DOM elements before the page is made visible to the user
- Set proper `height` and `width` properties for images, ads or social embeds before they are loaded, or set the `aspect-ratio` CSS property
- Animate using the CSS `transform` property rather than direct CSS position and size properties
- Reserve space for content that loads asynchronously and use properly sized placeholders (leverage static `height`/`min-height`, or position it `absolute`/`fixed`)
- Optimize your fallback fonts and `size-adjust` them to your custom font
  - https://web.dev/articles/css-size-adjust
  - https://www.debugbear.com/blog/web-font-layout-shift#adjusting-font-metrics
  - https://www.aem.live/developer/font-fallback
- [Avoid inserting new content without a clear user interaction](https://web.dev/articles/optimize-cls#avoid_inserting_new_content_without_a_user_interaction) (like a "load more" button on an infinite list, or toggling an accordion panel)
- Leverage the [`scrollbar-gutter: stable`](https://developer.mozilla.org/en-US/docs/Web/CSS/scrollbar-gutter) CSS property

## How to measure

### Manually
```js
new PerformanceObserver((entryList) => {
  for (const entry of entryList.getEntries()) {
    console.log('Layout shift:', entry);
  }
}).observe({ type: 'layout-shift', buffered: true });
```

### Using web-vitals.js

```js
import { onCLS } from 'web-vitals';

// Measure and log CLS in all situations
// where it needs to be reported.
onCLS(console.log);
```

## How to debug

Follow the steps in one of:
- [CLS in our performance audit](./performance-audit.md#cls) article
- the [Layout Shift event](https://web.dev/articles/debug-layout-shifts#devtools) in the Chrome DevTools performance audit panel
- the [first contentful paint](https://docs.webpagetest.org/getting-started/#first-contentful-paint) metric in the [webpagetest.org]() site perfromance audit


## References

- https://web.dev/articles/cls
- https://web.dev/articles/optimize-cls
- https://web.dev/articles/debug-layout-shifts
- https://www.debugbear.com/docs/metrics/cumulative-layout-shift
- https://www.debugbear.com/blog/devtools-layout-shift
- https://www.woorank.com/en/core-web-vitals/improving-cumulative-layout-shift
- https://culture-tecture.adobe.com/en/publish/2024/08/26/aem-blog-cumulative-layout-shift-cls-a-developer-s-nightmare
- https://css-triggers.com/
- https://richstyle.org/?documentation/css-will-change-property-en
