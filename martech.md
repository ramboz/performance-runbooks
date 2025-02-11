# MarTech

## Definition

Marketing Technologies (MarTech) is a term that refers to the tools, platforms, and technologies that businesses use to optimize their marketing efforts. The goal of MarTech is to make marketing processes more efficient and flexible, allowing marketers to focus on more creative and strategic work.

## Components

MarTech solutions typically include:
- Analytics tracking (like Adobe Analytics, Google Analytics, etc.) and user behavior tracking tools (like Hotjar, Microsoft Clarity, etc.)
- Content personalization and A/B testing libraries (like Adobe Target, Optimizely, etc.)
- Tag managers (like Google Tag Manager, Adobe Launch, Tealium, etc.)
- Programmatic ad platforms (like Google Ads, Facebook Ads, etc.)
- Social media tools (like Facebook, Twitter, etc. widgets)
- Email newsletters solutions (like Mailchimp, Constant Contact, etc.)
- Customer relationship management (CRM) integrations (like Salesforce, HubSpot, etc.)

## Most common issues

- Bad [TTFB](./ttfb.md), [TBT](./tbt.md), [FCP](./fcp.md), [CLS](./cls.md) or [INP](./inp.md)
- 3rd-party host that needs to be resolved (DNS + TLS handhsake)
- Use of Tag managers that loads everything at once in monolithic Tag containers

## Most common optimizations

- Don't expect too much from [Partytown](https://partytown.builder.io/), it has several limitations and still causes TBT in the end
- Delay all the MarTech integrations to the end of the page load, if content personalization and A/B tests are not required
- Remove redundant and unsed Tags from the Tag container
  - Does your marketing team really need Mirosoft Clarity, Meta Pixel, Google Analytics, Hotjar AND Google Analytics?
- Split the monolithic Tag container into smaller independent containers if possible
  - Load critical tags for the user experience first, and delay marketing tags only used to track behavior
- Split large Tags into smaller chunks to reduce the performance impact:
  - load presonalization and A/B test logic before the FCP
  - load data layer and analytics solutions at the end of the page load
  - defer any non-UI related MarTech integrations
- Self-host or proxy the MarTech libraries through your domain to reduce [TTFB](./ttfb.md) and [FCP](./fcp.md)
- Adjust Tag configurations to reduce impact on [INP](./inp.md)
- Leverage the performance helper methods from [aem-cwv-helper](https://github.com/ramboz/aem-cwv-helper) to break patch datalayer and event listeners from 3rd-party scripts to reduce long tasks
- Only enable 3rd-party libraries on pages that really need it
  - You only need conversion tracking on pages that are actually part of the conversion funnel. Your user profile config page is likely not one of those

## How to measure & debug

- Use https://3rdparty.io/ to vet your 3rd-party libraries
- See runbooks for [TTFB](./ttfb.md), [TBT](./tbt.md), [FCP](./fcp.md), [CLS](./cls.md) and [INP](./inp.md).

## References

- https://github.com/adobe-rnd/aem-martech
- https://themuralimanohar.medium.com/mastering-core-web-vitals-ffa73e7192a4
