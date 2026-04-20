# 🤖 AI Specification — CodeBloggs Web Application

> **IMPORTANT:** This document must be read before implementing, refactoring, or auditing any feature.
> All feature specification documents reference this file as their global contract.

---

## 1. Project Identity

| Field | Value |
|---|---|
| Project Name | CodeBloggs |
| Type | MERN Stack Web Application |
| Module | Full-Stack Development Program — Module 15 (DevOps) |
| Role | Junior Developer at Genesis Solutions |
| Status | Existing application — DevOps practices applied (no new features) |

**Short Description:**
CodeBloggs is a social media platform for programmers and developers to share blog posts, comment on other users' posts, and like content. Users can register, log in, manage their profiles, and interact with the developer community.

---

## 2. Project Scope

### In Scope

- User registration and login with session management
- Home page — logged-in user's profile and their posts
- Bloggs page — feed of all posts from all users
- Network page — list of all registered users
- Admin page — user and content management (admin only)
- Post creation via Post Modal
- Liking and commenting on posts
- Lighthouse performance auditing and targeted refactoring (Login + Home pages only)
- Selenium automated browser testing
- AWS cloud research documentation

### Out of Scope

- Real-time messaging or chat
- Email notifications or push alerts
- Image uploads or media hosting
- Advanced search or filtering
- Recommendation or feed-ranking algorithms
- Third-party authentication (Google, GitHub, etc.)
- Mobile or native applications
- Password reset flow
- Two-factor authentication
- Any features not defined in this document or a linked feature spec

---

## 3. Repository Structure

```
DevOps/                             ← Repository root
├── CodeBloggs/                     ← Full MERN application
│   ├── client/                     ← React frontend
│   │   ├── public/                 ← Static assets
│   │   └── src/
│   │       ├── assets/             ← Images and logos
│   │       ├── components/         ← Reusable UI components
│   │       ├── context/            ← React context providers
│   │       ├── layouts/            ← App layout wrappers
│   │       ├── pages/              ← Page-level components (one per route)
│   │       ├── stores/             ← State stores
│   │       ├── utils/              ← Helper functions
│   │       ├── App.jsx             ← Root component and routing
│   │       └── main.jsx            ← Entry point
│   ├── server/                     ← Node.js / Express backend
│   │   ├── controllers/            ← Route handler logic
│   │   ├── db/
│   │   │   ├── schemas/            ← Mongoose schemas
│   │   │   └── connection.js       ← MongoDB connection
│   │   ├── routes/                 ← Express route definitions
│   │   ├── middleware/             ← Auth and request middleware
│   │   └── server.js               ← Server entry point
│   └── docs/                       ← Legacy project documentation
├── ai/                             ← DevOps module AI specifications
│   ├── ai-spec.md                  ← THIS FILE — global spec (read first)
│   └── features/                   ← Feature-level spec documents
│       ├── login-page.feature.md
│       └── home-page.feature.md
├── Lighthouse/                     ← Audit reports (to be added)
│   ├── v1/
│   ├── v2/
│   └── benchmark/
├── Selenium/                       ← Automated test suite (to be added)
│   └── codebloggs-tests.side
├── AWS-Research/                   ← AWS and DevOps research (to be added)
└── README.md                       ← Project overview
```

---

## 4. Architecture

### Frontend

- **Framework:** React (Vite)
- **Routing:** React Router v6 — client-side, all routes defined in `App.jsx`
- **State:** React `useState` / `useEffect` for local state; Context API for global state (session, toast, post modal)
- **Styling:** Inline styles only — no external CSS frameworks (Tailwind config exists but inline styles are the pattern in use)
- **API communication:** Fetch API — no Axios or other HTTP libraries
- **Auth:** Session token stored in `localStorage` / cookie via `SessionContext`

### Backend

- **Runtime:** Node.js
- **Framework:** Express.js
- **Database:** MongoDB Atlas (shared cluster — schema must not be modified)
- **ODM:** Mongoose
- **Auth:** Session token pattern — token generated on login, stored in `sessions` collection, validated per request
- **Port:** `5050` (default)

### Database Collections

| Collection | Key Fields |
|---|---|
| `users` | `_id`, `first_name`, `last_name`, `email`, `password` (hashed), `birthday`, `occupation`, `location`, `auth_level`, `isOnline` |
| `posts` | `_id`, `user_id`, `content`, `likes`, `comments`, `createdAt` |
| `comments` | `_id`, `post_id`, `user_id`, `content`, `likes`, `createdAt` |
| `sessions` | `_id`, `session_token`, `user_id`, `created_at` |

---

## 5. API Reference

### Authentication

| Method | Endpoint | Description | Auth |
|---|---|---|---|
| POST | `/session/login` | Authenticate user, return token + user data | No |
| POST | `/session/register` | Create new user account | No |
| POST | `/session/logout` | Invalidate session token | Yes |

### Users

| Method | Endpoint | Description | Auth |
|---|---|---|---|
| GET | `/user/:id` | Get a single user by ID | Yes |
| GET | `/users` | Get all users | Yes |
| PATCH | `/user/:id` | Update user fields | Yes |
| DELETE | `/user/:id` | Delete a user (admin only) | Yes |

### Posts

| Method | Endpoint | Description | Auth |
|---|---|---|---|
| GET | `/posts` | Get all posts, sorted by most recent | Yes |
| GET | `/posts/user/:id` | Get posts by a specific user | Yes |
| POST | `/posts` | Create a new post | Yes |
| PATCH | `/post/:id` | Update post (likes, content) | Yes |
| DELETE | `/post/:id` | Delete a post | Yes |

### Comments

| Method | Endpoint | Description | Auth |
|---|---|---|---|
| POST | `/comment` | Create a comment on a post | Yes |

---

## 6. Allowed Technologies & Constraints

### Allowed

| Layer | Allowed |
|---|---|
| Frontend | React, React Router, React Icons (`react-icons`), Fetch API, Vite |
| Backend | Node.js, Express.js, Mongoose, dotenv |
| Database | MongoDB Atlas (shared cluster) |
| Testing | Selenium IDE |
| Auditing | Google Lighthouse (Chrome DevTools) |
| Version Control | Git, GitHub |

### Not Allowed

- No new npm packages without explicit approval
- No CSS frameworks (Bootstrap, Tailwind utility classes, Styled Components, etc.)
- No state management libraries (Redux, Zustand, Jotai, etc.)
- No third-party HTTP clients (Axios, etc.)
- No GraphQL
- No TypeScript (project is plain JavaScript)
- No schema changes to MongoDB Atlas collections
- No direct commits to `main`

---

## 7. Coding Standards & Conventions

### General

- Write junior-friendly, readable code — clarity over cleverness
- Keep solutions simple and maintainable
- Reuse existing components and utilities before creating new ones
- Do not add features beyond what a feature spec defines

### Naming

- React components: `PascalCase` (e.g. `UserCard.jsx`)
- Functions and variables: `camelCase`
- CSS class names: `kebab-case` (if used)
- Constants: `UPPER_SNAKE_CASE`
- Files: `PascalCase` for components, `camelCase` for utilities

### React

- One component per file
- Use functional components with hooks only — no class components
- Use `useState` for local state, Context for shared state
- Use `useEffect` with explicit dependency arrays
- Responsive breakpoint: `window.innerWidth >= 768` = desktop
- All inline styles use the project colour palette (see Section 8)

### API / Fetch

- Always use `credentials: "include"` on authenticated requests
- Always handle `.ok` check on responses before parsing
- Never log sensitive data (`session_token`, passwords) to the console
- Remove all `console.error` calls from production code — use `showToast` for user-facing errors

### Security

- Never store plain-text passwords anywhere on the client
- Use generic error messages for auth failures: `"Invalid email or password"`
- Validate inputs on both client (UX) and server (security)
- Do not expose internal error details to the browser console

### Comments

- Write comments only when the **why** is non-obvious
- Do not write comments explaining what the code does — use clear naming instead
- Do not add task references, PR numbers, or author names in comments

---

## 8. Colour Palette

| Purpose | Name | Hex |
|---|---|---|
| Primary | Purple | `#8D88EA` |
| Primary Hover | Dark Purple | `#6C63D9` |
| Accent | Teal | `#2ED3B7` |
| Background | Light Gray | `#F6F7FF` |
| Text / Dark UI | Navy | `#1F2340` |
| Borders | Soft Gray | `#E3E6F5` |
| Surface | White | `#FFFFFF` |
| Error Background | Light Red | `#FEF2F2` |
| Error Border | Red | `#FCA5A5` |
| Error Text | Dark Red | `#B91C1C` |

All UI elements must conform to this palette. No new colours may be introduced without updating this document.

---

## 9. User Roles

| Role | Access |
|---|---|
| Guest (unauthenticated) | Login page, Register page only |
| Basic User | All pages except Admin |
| Admin | All pages including Admin; can view any user's profile via `/home/:userId` |

---

## 10. Routes

| Route | Page | Auth Required | Notes |
|---|---|---|---|
| `/login` | Login | No | Redirects to `/home` if session exists |
| `/register` | Register | No | Redirects to `/home` if session exists |
| `/home` | Home (own profile) | Yes | Shows logged-in user's posts |
| `/home/:userId` | Home (other user) | Yes (admin only) | Non-admins are redirected |
| `/bloggs` | Blogs feed | Yes | All posts from all users |
| `/network` | Network | Yes | All user cards |
| `/admin` | Admin | Yes (admin only) | User + content management |

---

## 11. Global Definition of Done

A feature or refactor is considered **done** when all of the following are true:

### Functional
- [ ] All acceptance criteria in the feature spec are met
- [ ] The feature works correctly on both desktop (≥768px) and mobile (<768px)
- [ ] All existing features continue to work (no regressions)
- [ ] Authenticated routes redirect unauthenticated users to `/login`
- [ ] Admin-only routes redirect non-admin users appropriately

### Code Quality
- [ ] No `console.error`, `console.log`, or `console.warn` calls in production code
- [ ] No hardcoded API URLs — base URL is consistent and configurable
- [ ] No unused variables, imports, or dead code
- [ ] All inline styles conform to the colour palette
- [ ] No invalid CSS properties in inline style objects

### Accessibility (Lighthouse target)
- [ ] All interactive elements have accessible names (`aria-label` or visible text)
- [ ] Heading hierarchy is logical (`h1` → `h2` → `h3`)
- [ ] Form inputs have associated `<label>` elements
- [ ] Dynamic content regions use `role="alert"` or `aria-live` where appropriate
- [ ] Modal dialogs use `role="dialog"` and `aria-modal="true"`

### Git
- [ ] Work is committed on a `feature/*` branch created from `dev`
- [ ] Feature branch is merged into `dev` via pull request
- [ ] `dev` is merged into `main` before submission
- [ ] No direct commits to `main`
- [ ] Commit messages are clear and describe the change

---

## 12. Cross-Feature Rules

- **Session check:** Every authenticated page must check `SessionContext` on mount and redirect to `/login` if no valid session exists
- **Loading states:** Use `SkeletonLoader` during data fetches — never show blank content
- **Toast notifications:** Use `ToastContext.showToast()` for all user-facing success and error messages — never `alert()`
- **Post Modal:** The Post Modal is global — triggered via `PostModalContext` from the Header; any page that needs to refresh on new post must register a callback via `registerPostCreatedCallback`
- **Responsive layout:** All pages must handle both desktop and mobile breakpoints using the `isDesktop` state pattern (`window.innerWidth >= 768`)
- **Error handling:** Catch blocks must call `showToast` with a user-friendly message and must not call `console.error`
- **API base URL:** All fetch calls use `http://localhost:5050` — this must be consistent across all files
- **Scope (Module 15):** No new features are added. Refactoring is limited to the Login and Home pages only, and every change must be traceable to a specific Lighthouse audit finding

---

## 13. Lighthouse Auditing Requirements

Full audit workflow, file naming conventions, comparison document format, and external benchmark rules are defined in the dedicated spec:

> **See:** [lighthouse.feature.md](./features/lighthouse.feature.md)

**Summary:**
- Audit config: Chrome DevTools · Navigation mode · Desktop device
- Pages: Login (`/login`) and Home (`/home`)
- Process: v1 baseline → refactor → v2 re-audit → comparison document
- External benchmark: chosen site audited and compared against CodeBloggs v1 and v2
- All reports saved as `.html` files under `./Lighthouse/`

---

## 14. Selenium Testing Requirements

Full test category definitions, per-test acceptance criteria, and `.side` file requirements are defined in the dedicated spec:

> **See:** [selenium.feature.md](./features/selenium.feature.md)

**Summary:**
- Tool: Selenium IDE browser extension
- Base URL: `http://localhost:3000`
- Minimum: 10 tests across 5 categories
- All tests in a single file: `./Selenium/codebloggs-tests.side`
- Categories: Navigation, Form Submission, Form Validation, Responsiveness, Repetitive Tasks
- All tests must pass before submission

---

## 15. Feature Specification Index

All feature specs live in `./ai/features/`. Each spec references this document and must be read alongside it.

| Feature | File | Status |
|---|---|---|
| Login Page — Lighthouse Refactor | [login-page.feature.md](./features/login-page.feature.md) | Ready |
| Home Page — Lighthouse Refactor | [home-page.feature.md](./features/home-page.feature.md) | Ready |
| Lighthouse Auditing | [lighthouse.feature.md](./features/lighthouse.feature.md) | Ready |
| Selenium Automated Testing | [selenium.feature.md](./features/selenium.feature.md) | Ready |
