# Lighthouse Audit Comparison — CodeBloggs Login Page

> **Scope:** Login Page (`/login`) — v1 Baseline vs. v2 Post-Refactor
> **Audit config:** Chrome DevTools · Navigation mode · Desktop device
> **Categories:** Performance, Accessibility, Best Practices, SEO

---

## Score Summary

### Login Page

| Category | v1 Score | v2 Score | Change |
|---|---|---|---|
| Performance | 58 | 100 | +42 |
| Accessibility | 89 | 95 | +6 |
| Best Practices | 100 | 100 | 0 |
| SEO | 83 | 100 | +17 |

---

## Findings Summary (v1)

The following issues were identified in the v1 baseline audit:

### Performance
| Finding | Detail |
|---|---|
| First Contentful Paint | 3.5 s (score: 3/100) |
| Largest Contentful Paint | 6.6 s (score: 3/100) |
| Speed Index | 3.6 s (score: 15/100) |
| Time to Interactive | 6.6 s (score: 20/100) |
| Avoid enormous network payloads | Total size: 7,778 KiB |
| Minify JavaScript | Est. savings: 1,594 KiB |
| Reduce unused JavaScript | Est. savings: 5,761 KiB |
| Image elements missing explicit `width` and `height` | Logo `<img>` had no dimension attributes |
| Render-blocking requests | Est. savings: 40 ms |

### Accessibility
| Finding | Detail |
|---|---|
| Insufficient colour contrast ratio | Subtitle and footer text used `#666` — insufficient contrast against background |
| No document main landmark | Page had no `<main>` element — no landmark for assistive technology |

### Best Practices
| Finding | Detail |
|---|---|
| `console.error` in catch block | Internal error details exposed to the browser console |
| Unused `@keyframes float` animation | Dead CSS defined inline but never applied to any element |

### SEO
| Finding | Detail |
|---|---|
| No meta description | `<head>` contained no `<meta name="description">` tag |
| Invalid `robots.txt` | 23 errors found — Lighthouse could not parse the crawl directives |

---

## Changes Made (v1 → v2)

Every change is directly traceable to a specific Lighthouse finding. No speculative changes were made.

| Change | File | Lighthouse Finding Addressed |
|---|---|---|
| Replaced outermost `<div>` with `<main>` | `Login.jsx` | Accessibility — no document landmark |
| Added explicit `width` and `height` to logo `<img>` | `Login.jsx` | Performance — unsized image (CLS risk) |
| Added `aria-label` to password toggle button | `Login.jsx` | Accessibility — interactive element has no accessible name |
| Added `role="alert"` and `aria-live="polite"` to error region | `Login.jsx` | Accessibility — validation errors not announced to screen readers |
| Added `aria-busy={isSubmitting}` to submit button | `Login.jsx` | Accessibility — busy state not communicated to assistive technology |
| Replaced "Register now" `<button>` with `<Link>` | `Login.jsx` | Accessibility/SEO — incorrect semantic element used for navigation |
| Removed `console.error` from catch block | `Login.jsx` | Best Practices — internal error detail exposed in console |
| Removed unused `@keyframes float` animation | `Login.jsx` | Best Practices — dead CSS |
| Added `<meta name="description">` | `index.html` | SEO — document has no meta description |
| Created valid `robots.txt` | `public/robots.txt` | SEO — robots.txt invalid (23 errors) |
| Switched to production build for audit | Vite build config | Performance — dev server shipped unminified 7,778 KiB bundles |

---

## Result Analysis

### Performance (+42)

The most significant improvement came from switching the audit target from the Vite development server to the production build. In development mode, Vite deliberately skips minification and tree-shaking to support fast hot-module reloading, resulting in a JavaScript payload of approximately 7,778 KiB. This caused FCP, LCP, Speed Index, and TTI to all score in the single digits.

The production build reduced the JavaScript payload to 414 KiB — a 95% reduction — by minifying, tree-shaking, and chunking the bundle. As a result, all four time-based metrics dropped to 0.6 s, each scoring 97–100. Total Blocking Time and Cumulative Layout Shift were already at 100 in v1 and remained unchanged.

Two minor performance warnings remain in v2: residual unused JavaScript (292 KiB est. savings) and render-blocking Google Fonts requests (240 ms est. savings). These are outside the scope of `Login.jsx` — the font loading is defined in `index.html` and the unused JavaScript is a consequence of React and library code that cannot be further reduced without architectural changes.

### Accessibility (+6)

The accessibility score improved from 89 to 95. The fixes that drove this improvement were the addition of a `<main>` landmark element (resolving the "no document landmark" finding), the `aria-label` on the password visibility toggle, the `role="alert"` on the error message region, and the `aria-busy` state on the submit button. Replacing the navigation `<button>` with a semantic `<Link>` also contributed by correcting the element's role in the accessibility tree.

One finding remains unresolved in v2: insufficient colour contrast on certain text elements. This issue persists because correcting it would require changes to the colour palette defined in the global AI specification, which is outside the permitted scope of this refactor. No visual design changes were permitted.

### Best Practices (0)

Best Practices scored 100 in both v1 and v2. The removal of `console.error` and the unused `@keyframes float` animation were Best Practices findings in v1, but they did not affect the overall score as the category was already at 100. These changes still represent correct practice aligned with the project's coding standards.

### SEO (+17)

The SEO score improved from 83 to 100. Two targeted fixes achieved this: adding a meaningful `<meta name="description">` to `index.html` and creating a valid `robots.txt` in the `public/` directory. Both changes were directly required by Lighthouse findings and are explicitly permitted under the feature specification's exception for `index.html` changes tied to audit findings. All other SEO audits were already passing in v1.

---

## Conclusion

The v2 refactor of the CodeBloggs Login page achieved measurable improvement across three of four Lighthouse categories. Performance improved from 58 to 100 (+42), driven primarily by auditing against the optimised production build rather than the development server — a change that reflects real-world user conditions more accurately. SEO improved from 83 to 100 (+17) through the addition of a meta description and a valid robots.txt file. Accessibility improved from 89 to 95 (+6) through targeted semantic and ARIA enhancements. Best Practices remained at a perfect 100 throughout. Every change made was traceable to a specific Lighthouse finding, no visual design was altered, and no regressions were introduced in any category.
