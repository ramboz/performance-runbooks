# TTFB

## Definition

TTFB (_time to first byte_) measures the time it takes for the network to respond to a user request with the first byte of a resource.

## Components

TTFB is essentially composed of:
- DNS lookup
- TCP connection
- SSL connection
- HTTP request time

## Value Range

| TTFB              | Mobile/Desktop |
|-------------------|----------------|
| Good              | < 800ms        |
| Needs Improvement | 800ms – 1.6s   |
| Poor              | > 1.8s         |

## Most common issues

- Overhead of connections to other hosts
  - Slow DNS lookup
  - Overhead of TLS handshake for SSL connections
- Slow network times due to remote access
- Overhead of redirections
- Slow server processing times
- Nested network calls
- Cloud instance cold-starts

## Most common optimizations

- Use a CDN with global locations for all hosts (including DNS)
- Use local 3rd-party APIs, co-located within the CDN pop regions
- [Avoid redirect chains](https://developer.chrome.com/docs/lighthouse/performance/redirects)
- Leverage caching at every level
  - Database
  - Server response
  - CDN
  - Browser
- Ensure warm cloud instances are available in every region

## How to measure

Follow the steps in one of:
- the [network resource details](https://developer.chrome.com/docs/devtools/network#details) in the Chrome DevTools
- the [time to first byte](https://docs.webpagetest.org/getting-started/#time-to-first-byte) metric in the [webpagetest.org]() site perfromance audit


## References

- https://web.dev/articles/ttfb
- https://web.dev/articles/optimize-ttfb
- https://www.debugbear.com/docs/metrics/time-to-first-byte