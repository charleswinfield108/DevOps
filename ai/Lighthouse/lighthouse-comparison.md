# Lighthouse Audit Comparison — CodeBloggs

> **Scope:** Login Page (`/login`) and Home Page (`/home`) — v1 Baseline vs. v2 Post-Refactor
> **Audit config:** Chrome DevTools · Navigation mode · Desktop device
> **Categories:** Performance, Accessibility, Best Practices, SEO

---

## Score Summary

### Login Page (`/login`)

| Category | v1 Score | v2 Score | Change |
|---|---|---|---|
| Performance | 58 | 100 | +42 |
| Accessibility | 89 | 95 | +6 |
| Best Practices | 100 | 100 | 0 |
| SEO | 83 | 100 | +17 |

### Home Page (`/home`)

| Category | v1 Score | v2 Score | Change |
|---|---|---|---|
| Performance | 55 | 98 | +43 |
| Accessibility | 92 | 95 | +3 |
| Best Practices | 100 | 100 | 0 |
| SEO | 83 | 100 | +17 |

---

## Login Page — Findings Summary (v1)

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

## Login Page — Changes Made (v1 → v2)

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

## Home Page — Findings Summary (v1)

### Performance
| Finding | Detail |
|---|---|
| First Contentful Paint | 6.5 s (score: 0/100) |
| Largest Contentful Paint | 10.9 s (score: 0/100) |
| Speed Index | 6.5 s (score: 0/100) |
| Time to Interactive | 10.9 s (score: 2/100) |
| Avoid enormous network payloads | Total size: 7,776 KiB |
| Minify JavaScript | Est. savings: 1,594 KiB |
| Reduce unused JavaScript | Est. savings: 5,669 KiB |
| Forced reflow | JavaScript read/write pattern causing repeated layout recalculation |
| Render-blocking requests | Google Fonts stylesheet blocking initial render |

### Accessibility
| Finding | Detail |
|---|---|
| Insufficient colour contrast ratio | Label text used `#666` and `#999` — insufficient contrast against white background |
| Image elements missing explicit `width` and `height` | Images had no dimension attributes set |

### Best Practices
| Finding | Detail |
|---|---|
| Three `console.error` calls | Internal errors in fetch, like, and comment catch blocks exposed to the console |
| Invalid CSS property `fontColor` | Comment textarea used `fontColor` — not a valid CSS property, colour was never applied |

### SEO
| Finding | Detail |
|---|---|
| No meta description | `<head>` contained no `<meta name="description">` tag |
| Invalid `robots.txt` | 23 errors found — Lighthouse could not parse the crawl directives |

---

## Home Page — Changes Made (v1 → v2)

| Change | File | Lighthouse Finding Addressed |
|---|---|---|
| Replaced `/users?limit=1000` fetch with `/user/:id` | `Home.jsx` | Performance — unnecessary bulk payload to find a single user |
| Promoted posts heading from `<h2>` to `<h1>` | `Home.jsx` | Accessibility — no `h1` on page, broken heading hierarchy |
| Demoted user name from `<h2>` to `<p>` | `Home.jsx` | Accessibility — duplicate `h2` competing with posts heading |
| Wrapped emoji status indicators in `<span aria-hidden="true">` | `Home.jsx` | Accessibility — emoji-only status not meaningful to screen readers |
| Added dynamic `aria-label` to Like button (includes count) | `Home.jsx` | Accessibility — icon-only button has no accessible name |
| Added dynamic `aria-label` to Comment button (includes count) | `Home.jsx` | Accessibility — icon-only button has no accessible name |
| Added `aria-label="Close comments modal"` to close button | `Home.jsx` | Accessibility — modal close button has no accessible name |
| Added `role="dialog"`, `aria-modal`, `aria-labelledby` to modal | `Home.jsx` | Accessibility — comment modal not recognised as a dialog |
| Added `id="comment-modal-title"` to modal heading | `Home.jsx` | Accessibility — no `aria-labelledby` target for modal |
| Added visually-hidden `<label>` for comment textarea | `Home.jsx` | Accessibility — form input has no associated label |
| Replaced post card `<div>` with `<article>` | `Home.jsx` | SEO — post cards had no semantic structure |
| Removed all three `console.error` calls | `Home.jsx` | Best Practices — internal errors exposed in console |
| Fixed `fontColor` to `color` on comment textarea | `Home.jsx` | Best Practices — invalid CSS property, colour was never applied |
| Production build used for audit | Vite build config | Performance — dev server shipped unminified 7,776 KiB bundles |

---

## Result Analysis

### Performance

**Login (+42) / Home (+43)**

The most significant performance improvement on both pages came from switching the audit target from the Vite development server to the production build. In development mode, Vite deliberately skips minification and tree-shaking to support fast hot-module reloading, resulting in JavaScript payloads exceeding 7,700 KiB on both pages. The production build reduced this to 414 KiB — a 95% reduction — by minifying, tree-shaking, and chunking the bundle. As a direct result, FCP dropped from 3.5 s to 0.6 s on Login and from 6.5 s to 0.6 s on Home. LCP dropped from 6.6 s to 0.6 s on Login and from 10.9 s to 1.0 s on Home.

An additional code-level improvement on the Home page was replacing the `/users?limit=1000` bulk fetch with a targeted `/user/:id` request. This eliminated an unnecessary large network payload just to find a single user, reducing both data transfer and server processing time on each page load.

Minor performance warnings remain in v2 on both pages — residual unused JavaScript and render-blocking Google Fonts requests. These are outside the scope of `Login.jsx` and `Home.jsx`; the font loading is defined in `index.html` and the unused JavaScript is a consequence of shared React and library code.

### Accessibility

**Login (+6) / Home (+3)**

On the Login page, accessibility improved from 89 to 95. The fixes that drove this were the addition of a `<main>` landmark, a dynamic `aria-label` on the password toggle, `role="alert"` on the error region, `aria-busy` on the submit button, and replacing the navigation `<button>` with a semantic `<Link>`.

On the Home page, accessibility improved from 92 to 95. The heading hierarchy was corrected by promoting the posts section to `<h1>` and demoting the redundant user name display to `<p>`. Icon-only Like and Comment buttons received dynamic `aria-label` attributes that include the current count, resolving both the missing accessible name and label-content mismatch findings. The comment modal was upgraded with full dialog ARIA (`role="dialog"`, `aria-modal`, `aria-labelledby`), and the comment textarea received a visually-hidden associated `<label>`.

One finding remains unresolved on both pages: insufficient colour contrast on certain secondary text elements. Correcting this would require changes to the global colour palette defined in the AI specification, which is outside the permitted scope of this refactor.

### Best Practices

**Login (0) / Home (0)**

Best Practices scored 100 in both v1 and v2 on both pages. All `console.error` calls were removed from `Login.jsx` and `Home.jsx`, and the invalid `fontColor` CSS property in the Home page comment textarea was corrected to `color`. These changes align with the project coding standards and eliminate sources of future confusion, even though they did not affect the already-perfect Best Practices score.

### SEO

**Login (+17) / Home (+17)**

Both pages improved from 83 to 100. The same two fixes applied to both: a meaningful `<meta name="description">` was added to `index.html`, and a valid `robots.txt` was created in the `public/` directory. On the Home page, replacing post card `<div>` elements with `<article>` elements also contributed to semantic correctness. All other SEO audits were already passing in v1.

---

## Conclusion

The v2 refactor of the CodeBloggs Login and Home pages delivered consistent, measurable improvement across three of four Lighthouse categories on both pages. Performance improved by +42 on Login and +43 on Home, driven by the production build eliminating over 7,700 KiB of unminified development JavaScript. SEO improved by +17 on both pages through the addition of a meta description and a valid robots.txt. Accessibility improved by +6 on Login and +3 on Home through targeted semantic and ARIA enhancements. Best Practices remained at a perfect 100 throughout. Every change was directly traceable to a specific Lighthouse audit finding, no visual design was altered, and no regressions were introduced in any category across either page.
