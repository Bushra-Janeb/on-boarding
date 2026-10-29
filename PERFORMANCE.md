# Profile performance notes

## Chrome DevTools measurements

Measured in Chrome DevTools Performance **Local metrics** at a 1024px responsive viewport with CPU and network throttling disabled, using the local HTTP server.

| Version | LCP | CLS | LCP element |
| --- | ---: | ---: | --- |
| Before: profile with embedded hero image, no image preload, and no reserved image dimensions | 0.28 s | 0.00 | `.portrait-card__image` |
| After: responsive hero `srcset`/`sizes`, matching image preload, explicit dimensions, and lazy-loaded project artwork | 0.20 s | 0.00 | `.portrait-card__image` |

The local run improved LCP by 0.08 seconds and kept CLS at 0.00. It does **not** demonstrate the requested 0.5-second LCP gain: the baseline is already under 0.3 seconds on a local server with no throttling, so this setup does not provide a meaningful slow-network comparison. The result is environment-specific; a deployed test under a representative network profile is needed to evaluate that target.

Explicit `width` and `height` reserve space for both images. The hero preload `imagesrcset` and `imagesizes` mirror the image's responsive candidates, so the browser can fetch the selected size early. The project image is below the fold and uses `loading="lazy"` and `decoding="async"`.

## `will-change`

No animated element showed a measured rendering bottleneck in the local profile. The stylesheet therefore does not promote elements with `will-change`; add it only after a browser profile demonstrates a specific benefit.

## Intentionally retained minor defects

At the user's request, these two misspelled CSS selectors are intentionally unused and have no visual effect:

- `.profile-detail__value` (markup uses `.detail-card__value`)
- `.project-card__imgae` (markup uses `.project-card__image`)
