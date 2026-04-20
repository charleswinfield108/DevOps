# 🤖 AI Feature Specification — Home Page (Lighthouse Refactor v1)

> **Must be used alongside:** `AI_SPEC — Project Specification (Main).md`
> **Source audit:** Lighthouse v1 report — Home Page (`/home`)
> **Audit config:** Chrome DevTools · Navigation mode · Desktop device

---

## Feature Identity

| Field | Value |
|---|---|
| Feature Name | Home Page — Lighthouse Refactor |
| Version | v1 → v2 |
| Related Area | Frontend / Dashboard / User Profile |
| File | `client/src/pages/Home.jsx` |
| Route | `/home`, `/home/:userId` |

---

## Feature Goal

Refactor the existing CodeBloggs Home page to address specific issues identified by a Lighthouse audit (Performance, Accessibility, Best Practices, SEO). No new dashboard features are added. The goal is measurable score improvement in at least one Lighthouse category, with every change traceable to a specific audit finding.

---

## Feature Scope

### In Scope (Included)

- Accessibility: add `aria-label` to icon-only interactive buttons (Like, Comment, modal close)
- Accessibility: add `role="dialog"`, `aria-modal`, and `aria-labelledby` to the comment modal overlay
- Accessibility: associate the comment `<textarea>` with a visible or visually-hidden `<label>`
- Accessibility: replace emoji status indicators (🟢/🔴) with text + `aria-label` for screen reader compatibility
- Accessibility: fix heading hierarchy — ensure the page has a logical `h1` → `h2` → `h3` order
- Semantic HTML: wrap individual post cards in `<article>` elements
- Best Practices: remove all `console.error` calls from catch blocks
- Best Practices: fix invalid CSS property `fontColor` in the comment textarea — replace with `color`
- Performance: replace the inefficient `/users?limit=1000` fetch with a targeted `/user/:id` endpoint to reduce payload on page load
- SEO: ensure post card content uses semantically appropriate elements

### Out of Scope (Excluded)

- No new Home page features (post editing, post deletion, profile editing)
- No changes to the Like or Comment logic — only how buttons are announced
- No changes to the `Layout` component or any shared components
- No changes to backend API contracts except utilising the existing `/user/:id` endpoint
- No visual design changes — colour palette and layout are preserved
- No changes to the `SkeletonLoader`, `AvatarInitials`, or context providers
- No changes to any page other than `Home.jsx`

---

## Lighthouse Findings Addressed (v1 → v2)

| # | Category | Finding | Element | Fix |
|---|---|---|---|---|
| 1 | Accessibility | Like button has no accessible name — icon-only with no label | `<button>` (FiThumbsUp) | Add `aria-label={likedPosts.has(post._id) ? "Unlike post" : "Like post"}` |
| 2 | Accessibility | Comment toggle button has no accessible name | `<button>` (FiMessageCircle) | Add `aria-label="View comments"` |
| 3 | Accessibility | Modal close button has no accessible name (✕ symbol) | `<button>✕</button>` | Add `aria-label="Close comments modal"` |
| 4 | Accessibility | Comment modal is not a recognised dialog — no ARIA role | Modal overlay `<div>` | Add `role="dialog"`, `aria-modal="true"`, `aria-labelledby="comment-modal-title"` |
| 5 | Accessibility | Comment modal heading has no `id` for `aria-labelledby` reference | `<h2>Comments</h2>` | Add `id="comment-modal-title"` |
| 6 | Accessibility | Comment textarea has no associated label | `<textarea>` | Add a visually-hidden `<label>` linked via `htmlFor` / `id` |
| 7 | Accessibility | Emoji-only online status is not announced meaningfully | 🟢 / 🔴 with text | Wrap emoji in `<span aria-hidden="true">` and keep visible text |
| 8 | Accessibility | Heading hierarchy is broken — no `h1` on the page; `h2` used for user name and posts heading | Left column `<h2>`, right column `<h2>` | Promote the posts heading to `h1`; downgrade user name display to `<p>` or `<h2>` appropriately |
| 9 | Best Practices | `console.error` calls expose internal errors in the browser console | Three `catch` blocks | Remove all three `console.error` calls |
| 10 | Best Practices | `fontColor` is not a valid CSS property — dead style | Comment `<textarea>` inline style | Replace `fontColor: "#1F2340"` with `color: "#1F2340"` |
| 11 | Performance | Fetches all users (`/users?limit=1000`) to find a single user — unnecessary payload | `fetchUserPostData` | Replace with `fetch(\`http://localhost:5050/user/${targetUserId}\`)` to retrieve only the target user |
| 12 | SEO | Post cards have no semantic structure — plain `<div>` wrappers with no meaning | Post card `<div>` | Replace outermost post card `<div>` with `<article>` |

---

## Requirements Breakdown

### R1 — ARIA: Like Button Label
- The Like button must have a dynamic `aria-label` based on the current liked state
- `aria-label="Like post"` when not liked
- `aria-label="Unlike post"` when already liked

### R2 — ARIA: Comment Toggle Button Label
- The comment count button must have `aria-label="View comments"`
- When the comment section is open (active state), optionally update to `aria-label="Close comments"`

### R3 — ARIA: Modal Close Button Label
- The ✕ close button inside the comment modal must have `aria-label="Close comments modal"`
- The ✕ character itself can remain as visible content

### R4 — ARIA: Comment Modal Dialog Role
- The modal inner container `<div>` must receive:
  - `role="dialog"`
  - `aria-modal="true"`
  - `aria-labelledby="comment-modal-title"`
- The `<h2>Comments</h2>` inside the modal header must receive `id="comment-modal-title"`

### R5 — Accessible Comment Textarea Label
- A `<label>` element must be added for the comment textarea
- The label text is `"Write a comment"` and can be visually hidden using a screen-reader-only CSS class or inline style (`position: absolute; width: 1px; height: 1px; overflow: hidden; clip: rect(0,0,0,0); whiteSpace: nowrap`)
- The `<textarea>` must have `id="comment-input"` and the label must have `htmlFor="comment-input"`
- The existing `placeholder` attribute is retained alongside the label

### R6 — Accessible Status Indicator
- The 🟢 / 🔴 emoji must be wrapped in `<span aria-hidden="true">` so screen readers skip it
- The text label "Online" / "Offline" that follows remains in the DOM as the readable content
- No visual change

### R7 — Heading Hierarchy Fix
- The right column section heading `{viewingUser?.first_name}'s Posts` is the primary page subject — it must be an `<h1>`
- The left column user name currently in `<h2>` must be changed to a `<p>` with equivalent styling to avoid a duplicate `h2` competing with the posts heading
- The sidebar section headings "Status" and "Information" remain `<h3>` as they are already

### R8 — Remove console.error
- Remove `console.error("Error fetching posts:", error)` in the `fetchUserPostData` catch block
- Remove `console.error("Error updating post likes:", error)` in the `handleLikePost` catch block
- Remove `console.error("Error creating comment:", error)` in the `handleCreateComment` catch block
- The `showToast` and `setError` calls in the same catch blocks are kept — only the console calls are removed

### R9 — Fix Invalid CSS Property
- In the comment `<textarea>` inline style object, replace `fontColor: "#1F2340"` with `color: "#1F2340"`
- This is a dead style correction — the colour was never applied because `fontColor` is not a valid CSS property

### R10 — Optimise User Fetch
- Replace the `/users?limit=1000` call inside `fetchUserPostData` with a single targeted call to `/user/${targetUserId}`
- Expected response shape: a single user object (matches existing `/user/:id` endpoint)
- Update the `targetUser` assignment accordingly — no `.find()` array search needed
- All downstream usage of `viewingUser` is unchanged

### R11 — Semantic Post Cards
- Replace the outermost `<div>` wrapper for each post card with `<article>`
- All inline styles applied to the `<div>` are moved to the `<article>` element unchanged
- `key`, `onMouseEnter`, `onMouseLeave`, and all event handlers are moved to the `<article>`

---

## User Flow

```
Authenticated user navigates to /home
  └── Permission check (SessionContext)
        ├── Viewing own profile → allowed
        └── Viewing other user (non-admin) → redirect to /home

Page loads
  └── fetchUserPostData()
        ├── OLD: GET /users?limit=1000 → filter for targetUserId
        └── NEW: GET /user/:targetUserId (R10 — single targeted request)
              └── Sets viewingUser state

Left column renders
  └── AvatarInitials component (unchanged)
  └── User name displayed as <p> (R7 — was <h2>)
  └── Status section with <h3> heading
        └── 🟢/🔴 emoji wrapped in <span aria-hidden="true"> (R6)
        └── "Online" / "Offline" text remains readable
  └── Information section with <h3> heading

Right column renders
  └── <h1>{viewingUser?.first_name}'s Posts</h1> (R7 — was <h2>)
  └── SkeletonLoader while loading (unchanged)

Post list renders
  └── Each post wrapped in <article> (R11 — was <div>)
        └── Post content <p> (unchanged)
        └── Like button with aria-label="Like post" / "Unlike post" (R1)
        └── Comment button with aria-label="View comments" (R2)
        └── Comments section (unchanged)

User clicks comment button
  └── openCommentModal set to post._id
  └── Comment modal renders
        └── Modal inner div has role="dialog" aria-modal="true" aria-labelledby="comment-modal-title" (R4)
        └── <h2 id="comment-modal-title">Comments</h2> (R4, R5)
        └── Close button has aria-label="Close comments modal" (R3)
        └── Comment list (unchanged)
        └── <label htmlFor="comment-input"> visually hidden (R5)
        └── <textarea id="comment-input"> (R5)
        └── "Post Comment" button (unchanged)

User submits comment
  └── handleCreateComment() (unchanged)
        └── On error: no console.error (R8), toast shown if wired

User clicks Like
  └── handleLikePost() (unchanged)
        └── On error: no console.error (R8), state reverted
```

---

## Interfaces Involved

### Frontend

| File | Change |
|---|---|
| `client/src/pages/Home.jsx` | All changes are scoped to this file only |

### Components Used (Unchanged)

| Component | Role |
|---|---|
| `Layout` | Page shell — no changes |
| `SkeletonLoader` | Loading state — no changes |
| `AvatarInitials` | Avatar display — no changes |
| `SessionContext` | Session data — no changes |
| `ToastContext` | Error toasts — no changes |
| `PostModalContext` | Post creation callback — no changes |

### Backend / API

| Endpoint | Method | Change |
|---|---|---|
| `/user/:id` | GET | Now used for fetching the target user (was `/users?limit=1000`) |
| `/users` | GET | No longer called from Home.jsx |
| `/posts` | GET | Unchanged |
| `/post/:id` | PATCH | Unchanged (like update) |
| `/comment` | POST | Unchanged |

---

## Data, Validations & Expected Behaviour

All data inputs, validation rules, and session logic remain identical to v1. Changes are limited to how the UI communicates structure and state to the browser and assistive technology.

### User Data (from `/user/:id`)

| Field | Type | Source | Unchanged? |
|---|---|---|---|
| `_id` | string | MongoDB ObjectId | Yes |
| `first_name` | string | User document | Yes |
| `last_name` | string | User document | Yes |
| `auth_level` | string (`basic` / `admin`) | User document | Yes |
| `isOnline` | boolean | User document | Yes |

### Post Data (from `/posts`)

| Field | Type | Notes | Unchanged? |
|---|---|---|---|
| `_id` | string | Post identifier | Yes |
| `user_id` | string | Used to filter posts for target user | Yes |
| `content` | string | Displayed in post card | Yes |
| `likes` | number | Displayed and updated via PATCH | Yes |
| `comments` | array | Displayed in card and modal | Yes |
| `createdAt` | ISO date string | Formatted for display | Yes |

### Comment Data (POST `/comment`)

| Field | Type | Validation | Unchanged? |
|---|---|---|---|
| `content` | string | Required, non-empty (trimmed) | Yes |
| `user_id` | string | From session | Yes |
| `post_id` | string | From `openCommentModal` state | Yes |

---

## Acceptance Criteria

### Accessibility

- [ ] Like button has `aria-label="Like post"` when the post is not liked
- [ ] Like button has `aria-label="Unlike post"` when the post is liked
- [ ] Comment toggle button has `aria-label="View comments"`
- [ ] Comment modal close button has `aria-label="Close comments modal"`
- [ ] Comment modal inner container has `role="dialog"`, `aria-modal="true"`, and `aria-labelledby="comment-modal-title"`
- [ ] Modal `<h2>` heading has `id="comment-modal-title"`
- [ ] Comment textarea has a matching `<label>` with `htmlFor="comment-input"`
- [ ] Comment textarea has `id="comment-input"`
- [ ] Online/offline emoji is wrapped in `<span aria-hidden="true">`
- [ ] "Online" / "Offline" text is present in the DOM for screen readers
- [ ] Page has exactly one `<h1>` element (the posts section heading)
- [ ] No duplicate `<h2>` headings at the same hierarchy level
- [ ] Lighthouse Accessibility score improves vs. v1

### Performance

- [ ] `fetchUserPostData` calls `GET /user/${targetUserId}` (not `/users?limit=1000`)
- [ ] Network tab shows a single user request (not a bulk users request) on `/home` page load
- [ ] Page load data payload is reduced vs. v1 (fewer bytes transferred for user fetch)
- [ ] Lighthouse Performance score is equal to or better than v1

### Best Practices

- [ ] No `console.error` calls present anywhere in `Home.jsx`
- [ ] Comment textarea inline style uses `color: "#1F2340"` (not `fontColor`)
- [ ] Browser console shows no errors or warnings on page load
- [ ] Lighthouse Best Practices score is equal to or better than v1

### SEO

- [ ] Post cards render as `<article>` elements in the DOM
- [ ] Lighthouse SEO score is equal to or better than v1

### Regression (Existing Behaviour Unchanged)

- [ ] Logged-in user's profile and posts load correctly on `/home`
- [ ] Admin user can view other users' profiles via `/home/:userId`
- [ ] Non-admin user attempting to access `/home/:userId` is redirected and shown an error toast
- [ ] Post count and last post date display correctly in the Status section
- [ ] User information (name, auth level, online status) displays correctly
- [ ] Skeleton loader displays during data fetch
- [ ] "No posts yet" empty state displays when user has no posts
- [ ] Like button toggles correctly and persists via PATCH request
- [ ] Like state reverts on network error
- [ ] Comment modal opens when comment button is clicked
- [ ] Comment modal closes when backdrop is clicked
- [ ] Comment modal closes when close button is clicked
- [ ] Submitting a comment adds it to the post and clears the input
- [ ] "Post Comment" button is disabled when input is empty
- [ ] New post created via PostModal appears in the list without a page refresh
- [ ] Layout is correct on both desktop (two-column) and mobile (single-column)
- [ ] No new console errors or warnings introduced

---

## Notes for the AI

- All changes are confined to `client/src/pages/Home.jsx` — do not modify Layout, SkeletonLoader, AvatarInitials, or context files
- For R10 (user fetch optimisation): the `/user/:id` endpoint returns a single user object directly — adjust the assignment from `usersData.users.find(...)` to simply using the response body as `targetUser`
- For R5 (textarea label): use a visually-hidden inline style rather than a CSS class to avoid modifying any stylesheet — keep the approach self-contained within the JSX
- The `aria-label` on the Like button must be dynamic — it reads differently depending on `likedPosts.has(post._id)` and must update on each render
- `role="dialog"` should go on the inner white card `<div>`, not the dark overlay backdrop — the backdrop remains a plain `<div>` with the backdrop click handler
- For R7 (heading hierarchy): the user name in the left column is decorative repetition of data shown in the Information section — demoting it to `<p>` with the same font size and weight is correct and does not change visual appearance
- Preserve all existing inline styles exactly — no visual design changes
- Do not add `useCallback` or memoisation beyond what is listed — keep changes minimal and traceable to audit findings
