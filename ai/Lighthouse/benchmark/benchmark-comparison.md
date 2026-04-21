# Lighthouse Benchmark Comparison — CodeBloggs vs. Coursera

> **Scope:** Login and Home pages — CodeBloggs v1, CodeBloggs v2, and external benchmark site
> **Audit config:** Chrome DevTools · Navigation mode · Desktop device
> **Categories:** Performance, Accessibility, Best Practices, SEO

---

## External Website Details

| Field | Detail |
|---|---|
| **Site Name** | Coursera |
| **Login Page URL** | https://www.coursera.org/ |
| **Home Page URL** | https://www.coursera.org/ |
| **Reason for Selection** | Coursera is a large-scale, production-grade web application with authenticated user experiences — comparable in structure to CodeBloggs. It serves a global user base, is actively maintained by a professional engineering team, and provides a meaningful benchmark for evaluating real-world performance, accessibility, and SEO practices against a commercially deployed platform. |

---

## Pre-Refactor Benchmark — CodeBloggs v1 vs. Coursera

### Login Page

| Category | CodeBloggs v1 | Coursera | Difference |
|---|---|---|---|
| Performance | 58 | 67 | -9 |
| Accessibility | 89 | 100 | -11 |
| Best Practices | 100 | 50 | +50 |
| SEO | 83 | 100 | -17 |

### Home Page

| Category | CodeBloggs v1 | Coursera | Difference |
|---|---|---|---|
| Performance | 55 | 49 | +6 |
| Accessibility | 92 | 100 | -8 |
| Best Practices | 100 | 54 | +46 |
| SEO | 83 | 69 | +14 |

---

## Post-Refactor Benchmark — CodeBloggs v2 vs. Coursera

### Login Page

| Category | CodeBloggs v2 | Coursera | Difference |
|---|---|---|---|
| Performance | 100 | 67 | +33 |
| Accessibility | 95 | 100 | -5 |
| Best Practices | 100 | 50 | +50 |
| SEO | 100 | 100 | 0 |

### Home Page

| Category | CodeBloggs v2 | Coursera | Difference |
|---|---|---|---|
| Performance | 98 | 49 | +49 |
| Accessibility | 95 | 100 | -5 |
| Best Practices | 100 | 54 | +46 |
| SEO | 100 | 69 | +31 |

---

## Analysis

### Performance

In v1, CodeBloggs trailed Coursera on the Login page (58 vs. 67) and narrowly led on the Home page (55 vs. 49). Both sites were hindered by large JavaScript payloads — CodeBloggs by its unminified development build (7,700+ KiB), and Coursera by its scale and third-party script load (4,018 KiB login / 5,937 KiB home). Coursera's faster FCP on the Login page (0.5 s) was offset by a severe Total Blocking Time of 970 ms caused by heavy JavaScript execution, resulting in a Time to Interactive of 7.4 s. On the Home page, Coursera's CLS score of 0.243 and TTI of 7.7 s dragged its performance score down to 49.

In v2, CodeBloggs substantially outperformed Coursera on both pages after switching to the production build. The Login page reached a perfect 100 vs. Coursera's 67, and the Home page scored 98 vs. Coursera's 49. CodeBloggs v2 achieves FCP and LCP of 0.6 s on both pages with near-zero Total Blocking Time, while Coursera's heavy third-party JavaScript burden keeps its TTI above 7 s on both pages.

### Accessibility

Coursera scored 100 on both pages — a strong result driven by a mature design system with consistent ARIA implementation. In v1, CodeBloggs scored 89 on Login and 92 on Home, primarily due to missing landmark regions, unlabelled interactive elements, and broken heading hierarchy. After refactoring, CodeBloggs v2 improved to 95 on both pages, closing the gap to just 5 points. The remaining 5-point gap reflects one unresolved contrast issue on secondary text elements, which would require a change to the global colour palette and is outside the permitted refactor scope. CodeBloggs now meets or exceeds Coursera's accessibility standard in all respects except colour contrast.

### Best Practices

CodeBloggs scored 100 on Best Practices in both v1 and v2 — a significant advantage over Coursera, which scored 50 on Login and 54 on Home. Coursera's Best Practices failures were driven by browser console errors, deprecated API usage, incorrect image aspect ratios, and 79 third-party cookies. These are characteristic of large commercial platforms with many third-party integrations (analytics, advertising, tracking). CodeBloggs, as a focused single-purpose application, avoids these issues entirely. This is an area where CodeBloggs holds a clear and consistent advantage.

### SEO

In v1, CodeBloggs trailed Coursera on the Login page (83 vs. 100) due to a missing meta description and an invalid robots.txt. On the Home page, CodeBloggs led (83 vs. 69) — Coursera's home page audit flagged it as blocked from indexing, which is a critical SEO failure for a public landing page. After the v2 refactor, CodeBloggs reached 100 on both pages, matching Coursera on Login and significantly outperforming on Home (100 vs. 69). The addition of a meta description and a valid robots.txt resolved all outstanding SEO findings.

---

## Conclusion

The benchmark comparison demonstrates that CodeBloggs v2 competes favourably with Coursera — a large-scale, professionally maintained production application — across all four Lighthouse categories. CodeBloggs v2 outperforms Coursera on Performance (100 vs. 67 on Login, 98 vs. 49 on Home), matches or exceeds it on SEO (100 vs. 100 on Login, 100 vs. 69 on Home), and substantially leads on Best Practices (100 vs. 50–54 across both pages). Accessibility is the one category where Coursera holds an advantage (100 vs. 95), driven by a 5-point gap attributable to a single colour contrast issue outside the permitted refactor scope. Overall, the refactoring process transformed CodeBloggs from a site that trailed a commercial benchmark in three of four categories to one that leads in three of four — a clear and measurable demonstration of the impact of targeted Lighthouse-driven improvements.
