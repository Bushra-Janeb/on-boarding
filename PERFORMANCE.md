# Profile performance notes

## Chrome DevTools measurements

Measured in Chrome DevTools Performance **Local metrics** at a 1024px responsive viewport with CPU and network throttling disabled, using the local HTTP server.

| Version | LCP | CLS | LCP element |
| --- | ---: | ---: | --- |
| Before: profile with embedded hero image, no image preload, and no reserved image dimensions | 0.28 s | 0.00 | `.portrait-card__image` |
| After: responsive hero `srcset`/`sizes`, matching image preload, explicit dimensions, and lazy-loaded project artwork | 0.20 s | 0.00 | `.portrait-card__image` |

The local run improved LCP by 0.08 seconds and kept CLS at 0.00. It does **not** demonstrate the requested 0.5-second LCP gain: the baseline is already under 0.3 seconds on a local server with no throttling, so this setup does not provide a meaningful slow-network comparison. The result is environment-specific; a deployed test under a representative network profile is needed to evaluate that target.

## Throttled follow-up (Chrome DevTools, local HTTP host)

I repeated a first-visit Performance-panel recording at a 1024px responsive viewport with the Chrome **3G** preset and **Disable cache** enabled. The trace reported **CLS 0.00**, but its selected time range began at 1.935 seconds and omitted the beginning of page load, so DevTools reported **LCP unavailable** for that selection. This is not a valid LCP measurement and cannot be compared with the unthrottled numbers above. No 0.5-second LCP improvement is claimed.

The live DevTools local-metrics view previously identified `h1#profile-title` as the LCP element. That means this page's LCP is currently text, rather than the preloaded portrait; the image preload and below-the-fold lazy loading do not, by themselves, establish an LCP improvement for this page. A valid before/after comparison still requires full-load traces with the same network profile and cache state, ideally on the deployed host.

Explicit `width` and `height` reserve space for both images. The hero preload `imagesrcset` and `imagesizes` mirror the image's responsive candidates, so the browser can fetch the selected size early. The project image is below the fold and uses `loading="lazy"` and `decoding="async"`.

## `will-change`

No animated element showed a measured rendering bottleneck in the local profile. The stylesheet therefore does not promote elements with `will-change`; add it only after a browser profile demonstrates a specific benefit.

## Intentionally retained minor defects

At the user's request, these two misspelled CSS selectors are intentionally unused and have no visual effect:

- `.profile-detail__value` (markup uses `.detail-card__value`)
- `.project-card__imgae` (markup uses `.project-card__image`)
