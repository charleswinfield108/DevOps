# 🤖 AI Feature Specification — Login Page (Lighthouse Refactor v1)

> **Must be used alongside:** `AI_SPEC — Project Specification (Main).md`
> **Source audit:** Lighthouse v1 report — Login Page (`/login`)
> **Audit config:** Chrome DevTools · Navigation mode · Desktop device

---

## Feature Identity

| Field | Value |
|---|---|
| Feature Name | Login Page — Lighthouse Refactor |
| Version | v1 → v2 |
| Related Area | Frontend / Authentication / Performance |
| File | `client/src/pages/Login.jsx` |
| Route | `/login` |

---

## Feature Goal

Refactor the existing CodeBloggs Login page to address specific issues identified by a Lighthouse audit (Performance, Accessibility, Best Practices, SEO). No new login features are added. The goal is measurable score improvement in at least one Lighthouse category, with every change traceable to a specific audit finding.

---

## Feature Scope

### In Scope (Included)

- Accessibility improvements: ARIA roles, labels, and live regions on interactive elements
- Semantic HTML: replace generic `<div>` wrappers with `<main>` and `<section>` where appropriate
- Image optimisation: add explicit `width` and `height` attributes to the logo `<img>` to eliminate Cumulative Layout Shift (CLS)
- Remove unused CSS: the `@keyframes float` animation is defined inline but never applied to any element
- Remove `console.error` calls that expose internal errors to the browser console
- Password toggle button: add descriptive `aria-label` so screen readers can announce the action
- Error message region: add `role="alert"` and `aria-live="polite"` so assistive technology announces validation errors
- Submit button: add `aria-busy` state during form submission
- Navigation link: replace the `<button>` used for "Register now" navigation with a proper `<a>` element via React Router `<Link>` for correct semantic meaning

### Out of Scope (Excluded)

- No new login features (password reset, remember me, social login, 2FA)
- No changes to backend API or session logic
- No changes to form validation rules — only how errors are announced
- No changes to the overall visual design or colour palette
- No changes to any page other than `Login.jsx`
- No changes to `public/index.html` unless directly required by an audit finding

---

## Lighthouse Findings Addressed (v1 → v2)

| # | Category | Finding | Element | Fix |
|---|---|---|---|---|
| 1 | Accessibility | Password toggle has no accessible name | `<button>` (eye icon) | Add `aria-label="Show password"` / `"Hide password"` |
| 2 | Accessibility | Error message not announced to screen readers | Error `<div>` | Add `role="alert"` and `aria-live="polite"` |
| 3 | Accessibility | Submit button state not communicated during load | Submit `<button>` | Add `aria-busy={isSubmitting}` |
| 4 | Accessibility | "Register now" is a `<button>` used as a navigation link | Footer `<button>` | Replace with React Router `<Link>` styled as inline text |
| 5 | Performance | Logo `<img>` has no explicit `width`/`height` — causes layout shift (CLS) | `<img src={logo}>` | Add `width` and `height` attributes matching rendered size |
| 6 | Best Practices | `console.error` leaks internal error detail to the browser console | `catch` block | Remove or replace with silent handling |
| 7 | Best Practices | Unused `@keyframes float` animation defined in inline `<style>` | `<style>` block | Remove the unused keyframe rule |
| 8 | SEO | Login form uses `<div>` as page root — no landmark region | Page wrapper | Wrap form in `<main>` for document landmark structure |

---

## Requirements Breakdown

### R1 — ARIA: Password Toggle Button
- The toggle button must have a dynamic `aria-label` that updates based on `showPassword` state
- `aria-label="Show password"` when password is hidden
- `aria-label="Hide password"` when password is visible

### R2 — ARIA: Error Message Region
- The error `<div>` must include `role="alert"` so it is announced immediately on appearance
- Add `aria-live="polite"` as a fallback for broader screen reader support
- Behaviour is unchanged — error still clears when user starts typing

### R3 — ARIA: Submit Button Busy State
- The submit `<button>` must receive `aria-busy={isSubmitting}`
- When `isSubmitting` is `true`, `aria-busy="true"` signals to assistive technology that an operation is in progress

### R4 — Semantic Navigation Link
- The "Register now" `<button onClick={() => navigate("/register")}>` must be replaced with a React Router `<Link to="/register">`
- Visual styling must be preserved (inline, purple, underline on hover)
- This corrects the semantic meaning from an action button to a navigation link

### R5 — Image CLS Fix
- The logo `<img>` must include explicit `width` and `height` HTML attributes
- Values must match the CSS-rendered dimensions (`height: 40px` on desktop, `height: 30px` on mobile)
- The `objectFit: "contain"` style is preserved

### R6 — Remove console.error
- The `catch (err)` block in `handleSubmit` must not call `console.error`
- The user-facing error state (`setError(...)`) is kept — only the console call is removed

### R7 — Remove Unused Keyframe
- The `<style>` block containing `@keyframes float` must be removed entirely
- Confirm no element in the component references this animation before removal

### R8 — Semantic Landmark
- The outermost page `<div>` must be replaced with `<main>` to provide a document landmark
- No layout or styling changes — only the element type changes

---

## User Flow

```
User arrives at /login
  └── Session check (SessionContext)
        ├── Session exists → redirect to /home (unchanged)
        └── No session → render Login page

User views Login page
  └── <main> landmark wraps the page (NEW: R8)
        └── Logo <img width height> renders without layout shift (NEW: R5)
        └── Form card renders

User fills email field
  └── onChange clears error state (unchanged)

User fills password field
  └── Password toggle <button aria-label="..."> (NEW: R1)
        ├── Click → toggles showPassword state
        └── aria-label updates: "Show password" / "Hide password"

User submits form
  └── Validation runs (unchanged)
        ├── Invalid → error div with role="alert" appears (NEW: R2)
        │             └── Screen reader announces error immediately
        └── Valid → isSubmitting = true
                    └── Submit button aria-busy="true" (NEW: R3)
                    └── POST /session/login (unchanged)
                          ├── Success → session stored, navigate /home
                          └── Failure → setError(...), no console.error (NEW: R6)

User clicks "Register now"
  └── <Link to="/register"> navigates to /register (NEW: R4)
```

---

## Interfaces Involved

### Frontend

| File | Change |
|---|---|
| `client/src/pages/Login.jsx` | All changes are scoped to this file only |

### Components Used (Unchanged)

| Component | Role |
|---|---|
| `SessionContext` | Session check and login function — no changes |
| `useNavigate` | Routing — replaced by `<Link>` for register nav (R4) |

### Backend / API (Unchanged)

| Endpoint | Method | Description |
|---|---|---|
| `/session/login` | POST | Authenticate user — no changes to contract or behaviour |

---

## Data, Validations & Expected Behaviour

All data inputs, validation rules, and API contracts remain identical to v1. The only changes are to how the UI communicates state to the browser and assistive technology.

| Field | Type | Validation | Unchanged? |
|---|---|---|---|
| `email` | string | Required, valid email format (regex) | Yes |
| `password` | string | Required, non-empty | Yes |

**Error messages (unchanged):**
- `"Email and password are required"` — empty field submission
- `"Please enter a valid email address"` — invalid email format
- `"Login failed. Please try again."` — 4xx API response
- `"An error occurred. Please try again."` — network/timeout error

**Session storage (unchanged):**
- `session_token`, `id`, `first_name`, `last_name`, `auth_level`, `isOnline` stored via `SessionContext.login()`

---

## Acceptance Criteria

### Accessibility

- [ ] Password toggle button has `aria-label="Show password"` when password is hidden
- [ ] Password toggle button has `aria-label="Hide password"` when password is visible
- [ ] Error message container has `role="alert"` attribute
- [ ] Error message container has `aria-live="polite"` attribute
- [ ] Error is announced by screen reader (VoiceOver / NVDA) when it appears
- [ ] Submit button has `aria-busy="true"` while `isSubmitting` is true
- [ ] Submit button has `aria-busy="false"` when not submitting
- [ ] "Register now" renders as an `<a>` element in the DOM (not `<button>`)
- [ ] "Register now" navigates to `/register` on click
- [ ] Lighthouse Accessibility score improves vs. v1

### Performance

- [ ] Logo `<img>` element has explicit `width` attribute set
- [ ] Logo `<img>` element has explicit `height` attribute set
- [ ] No layout shift visible on page load (CLS reduced)
- [ ] Lighthouse Performance score is equal to or better than v1

### Best Practices

- [ ] No `console.error` calls present in `Login.jsx`
- [ ] No `@keyframes float` rule present in the inline `<style>` block (or block removed entirely)
- [ ] Lighthouse Best Practices score is equal to or better than v1

### SEO

- [ ] Page outermost wrapper renders as `<main>` in the DOM
- [ ] Lighthouse SEO score is equal to or better than v1

### Regression (Existing Behaviour Unchanged)

- [ ] Login page displays when no session token exists
- [ ] Logged-in users are redirected to `/home` automatically
- [ ] Email and password fields accept user input
- [ ] Form prevents submission when fields are empty
- [ ] Form prevents submission when email format is invalid
- [ ] Successful login stores session and redirects to `/home`
- [ ] Invalid credentials display error message
- [ ] Error message clears when user starts typing
- [ ] Password visibility toggle works correctly
- [ ] Page renders correctly on desktop and mobile breakpoints
- [ ] No new console errors or warnings introduced

---

## Notes for the AI

- All changes are confined to `client/src/pages/Login.jsx` — do not modify any other file unless a Lighthouse finding explicitly requires it
- Preserve all existing inline styles exactly — visual design must not change
- The `<main>` element replaces the outermost `<div>` only; inner structure is unchanged
- For the `<Link>` component: import from `react-router-dom` and apply all existing inline styles to the Link element using its `style` prop and `className` if needed
- The `aria-label` on the password toggle should be a string, not the emoji — e.g. `aria-label={showPassword ? "Hide password" : "Show password"}`
- Do not add any new state, hooks, or logic beyond what is listed in the requirements
- After changes, confirm that `@keyframes float` is not referenced anywhere else in the file before removing it
- The fix for CLS (R5) requires static values — use `width={isDesktop ? 200 : 150}` or similar based on the rendered size, keeping aspect ratio consistent with the original image
