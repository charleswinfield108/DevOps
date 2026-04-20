# 🤖 AI Feature Specification — Lighthouse Auditing

> **Must be used alongside:** `../ai-spec.md`
> **Module:** Full-Stack Development Program — Module 15 (DevOps)
> **Scope:** Audit, refactor, and benchmark CodeBloggs Login and Home pages

---

## Feature Identity

| Field | Value |
|---|---|
| Feature Name | Lighthouse Auditing — v1, Refactor, v2, Benchmark |
| Related Area | DevOps / Performance / Accessibility / SEO |
| Pages | Login (`/login`), Home (`/home`) |
| Tool | Google Lighthouse — Chrome DevTools |

---

## Feature Goal

Audit the CodeBloggs Login and Home pages using Lighthouse, refactor both pages based on findings, re-audit to demonstrate measurable improvement, and benchmark CodeBloggs against an external website.

---

## Feature Scope

### In Scope

- Lighthouse audits on the Login and Home pages (v1 and v2)
- Targeted refactoring of Login and Home pages based on audit findings
- Saving all reports as `.html` files in the defined folder structure
- A professional written comparison of v1 vs. v2 for both pages
- Lighthouse audits on an external website (login and home pages)
- A professional benchmark comparison of CodeBloggs vs. the external site (v1 and v2)

### Out of Scope

- Auditing any page other than Login and Home
- Refactoring any file other than `Login.jsx` and `Home.jsx`
- Changes to backend, database, or shared components
- Automated Lighthouse tooling or CI integration
- Mobile device configuration (Desktop mode only)

---

## Audit Configuration

All audits — CodeBloggs and external — must use these settings. Results from any other configuration are invalid.

| Setting | Required Value |
|---|---|
| Tool | Chrome DevTools → Lighthouse tab |
| Mode | Navigation |
| Device | Desktop |
| Categories | Performance, Accessibility, Best Practices, SEO |
| Network throttling | Default (as set in Chrome DevTools) |

---

## Track 1 — CodeBloggs (Localhost)

### Prerequisites

- CodeBloggs client is running at `http://localhost:3000`
- CodeBloggs server is running at `http://localhost:5050`
- MongoDB Atlas connection is active
- A valid user account exists for testing the Home page

### Step 1 — v1 Baseline Audits

Run audits **before making any code changes**.

| Page | URL | Save Report As |
|---|---|---|
| Login | `http://localhost:3000/login` | `./Lighthouse/v1/login-v1.html` |
| Home | `http://localhost:3000/home` | `./Lighthouse/v1/home-v1.html` |

**How to save an `.html` report:**
1. Run the Lighthouse audit in Chrome DevTools
2. Click the download icon (top right of the report)
3. Select "Save as HTML"
4. Save to the correct path

### Step 2 — Refactor

Refactor both pages using the feature specification documents and v1 findings.

| Page | Specification Document |
|---|---|
| Login | `./ai/features/login-page.feature.md` |
| Home | `./ai/features/home-page.feature.md` |

**Rules:**
- Every code change must map to a specific Lighthouse finding — no speculative changes
- Refactoring is limited to `Login.jsx` and `Home.jsx` only
- No visual design changes — colour palette and layout must be preserved
- All changes must follow the coding standards in `../ai-spec.md`

### Step 3 — v2 Re-Audits

Run audits **after refactoring is complete**.

| Page | URL | Save Report As |
|---|---|---|
| Login | `http://localhost:3000/login` | `./Lighthouse/v2/login-v2.html` |
| Home | `http://localhost:3000/home` | `./Lighthouse/v2/home-v2.html` |

**Pass criteria:**
- v2 must show measurable improvement over v1 in **at least one category** per page
- All previously passing scores must be maintained or improved — no regressions

### Step 4 — Comparison Document

Write a professional comparison document summarising v1 vs. v2 for both pages.

**Save as:** `./Lighthouse/lighthouse-comparison.md`

**Required content:**

1. **Score Summary Table** — one table per page

| Category | v1 Score | v2 Score | Change |
|---|---|---|---|
| Performance | | | |
| Accessibility | | | |
| Best Practices | | | |
| SEO | | | |

2. **Findings Summary** — for each page, list the specific issues identified in v1
3. **Changes Made** — for each page, list the refactoring changes applied and which finding they address
4. **Result Analysis** — explain what drove the improvement and whether any category did not improve and why
5. **Conclusion** — one paragraph professional summary

---

## Track 2 — External Website Benchmark

### Choosing an External Website

- Choose any publicly accessible website with a visible login page and a home/landing page
- Document the website name and URLs used at the top of the benchmark report
- Use the same Chrome DevTools configuration as Track 1

### Step 1 — External Audits

| Page | Save Report As |
|---|---|
| External login page | `./Lighthouse/benchmark/external-login.html` |
| External home/landing page | `./Lighthouse/benchmark/external-home.html` |

### Step 2 — Benchmark Comparison Document

Write a professional benchmark comparison document.

**Save as:** `./Lighthouse/benchmark/benchmark-comparison.md`

**Required content:**

1. **External Website Details** — name, URL, reason for selection

2. **Pre-Refactor Benchmark** — CodeBloggs v1 vs. external site

| Category | CodeBloggs v1 (Login) | External (Login) | CodeBloggs v1 (Home) | External (Home) |
|---|---|---|---|---|
| Performance | | | | |
| Accessibility | | | | |
| Best Practices | | | | |
| SEO | | | | |

3. **Post-Refactor Benchmark** — CodeBloggs v2 vs. external site

| Category | CodeBloggs v2 (Login) | External (Login) | CodeBloggs v2 (Home) | External (Home) |
|---|---|---|---|---|
| Performance | | | | |
| Accessibility | | | | |
| Best Practices | | | | |
| SEO | | | | |

4. **Analysis** — where does CodeBloggs compare favourably or unfavourably, and why
5. **Conclusion** — one paragraph professional summary of the benchmark findings

---

## File Structure

```
Lighthouse/
├── v1/
│   ├── login-v1.html               ← Baseline Login audit
│   └── home-v1.html                ← Baseline Home audit
├── v2/
│   ├── login-v2.html               ← Post-refactor Login audit
│   └── home-v2.html                ← Post-refactor Home audit
├── benchmark/
│   ├── external-login.html         ← External site login audit
│   ├── external-home.html          ← External site home audit
│   └── benchmark-comparison.md     ← v1 + v2 vs. external benchmark
└── lighthouse-comparison.md        ← v1 vs. v2 comparison for both pages
```

---

## Acceptance Criteria

### v1 Reports
- [ ] Login v1 report saved as `./Lighthouse/v1/login-v1.html`
- [ ] Home v1 report saved as `./Lighthouse/v1/home-v1.html`
- [ ] Both reports run with Desktop device and Navigation mode
- [ ] Reports captured before any refactoring

### Refactor
- [ ] Login page refactored per `login-page.feature.md`
- [ ] Home page refactored per `home-page.feature.md`
- [ ] Every code change is traceable to a specific Lighthouse finding
- [ ] No files modified other than `Login.jsx` and `Home.jsx`
- [ ] Application runs without errors after refactoring

### v2 Reports
- [ ] Login v2 report saved as `./Lighthouse/v2/login-v2.html`
- [ ] Home v2 report saved as `./Lighthouse/v2/home-v2.html`
- [ ] v2 Login score shows improvement in at least one category vs. v1
- [ ] v2 Home score shows improvement in at least one category vs. v1
- [ ] No category scores regressed from v1 to v2

### Comparison Document
- [ ] `lighthouse-comparison.md` exists at `./Lighthouse/`
- [ ] Score tables present for both Login and Home pages
- [ ] Findings, changes, and analysis sections are complete
- [ ] Writing is professional and in own words — not copied from Lighthouse output

### Benchmark
- [ ] External site chosen and documented
- [ ] Both external reports saved in `./Lighthouse/benchmark/`
- [ ] `benchmark-comparison.md` contains pre- and post-refactor comparison tables
- [ ] Analysis demonstrates genuine understanding of the score differences
