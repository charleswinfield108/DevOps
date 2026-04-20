# 🤖 AI Feature Specification — Selenium Automated Testing

> **Must be used alongside:** `../ai-spec.md`
> **Module:** Full-Stack Development Program — Module 15 (DevOps)
> **Scope:** Automated browser test suite for CodeBloggs using Selenium IDE

---

## Feature Identity

| Field | Value |
|---|---|
| Feature Name | Selenium Automated Test Suite |
| Related Area | DevOps / Quality Assurance / Automated Testing |
| Tool | Selenium IDE (browser extension) |
| Application | CodeBloggs — `http://localhost:3000` |

---

## Feature Goal

Build a repeatable automated browser test suite that covers the core functionality of the CodeBloggs application across five test categories. All tests must pass before submission.

---

## Feature Scope

### In Scope

- 10 or more automated tests across 5 defined categories
- Tests exported as a single `.side` file
- Tests targeting the CodeBloggs application running locally
- Tests covering navigation, form submission, form validation, responsiveness, and repetitive task automation

### Out of Scope

- Backend API testing (use Postman for that)
- Unit or integration testing (Jest, Vitest, etc.)
- CI/CD pipeline integration
- Tests against any deployed/production URL
- Performance or load testing

---

## Configuration

| Setting | Value |
|---|---|
| Tool | Selenium IDE browser extension (Chrome or Firefox) |
| Base URL | `http://localhost:3000` |
| Export file | `./Selenium/codebloggs-tests.side` |
| File count | Single `.side` file containing all tests |
| Minimum tests | 10 tests across 5 categories |

### Prerequisites

- CodeBloggs client running at `http://localhost:3000`
- CodeBloggs server running at `http://localhost:5050`
- MongoDB Atlas connection active
- At least one valid user account available for login tests
- Selenium IDE installed as a browser extension

---

## Test Categories

---

### Category 1 — Navigation

**Goal:** Automate visiting multiple pages, clicking links, and confirming the correct page loads after each action.

| Test | Steps | Assertion |
|---|---|---|
| Navigate to Login page | Open `http://localhost:3000` | Login page heading is visible |
| Navigate to Register page | Click "Register now" link on Login | Register page heading or form is visible |
| Navigate to Home after login | Submit valid credentials | URL contains `/home` |
| Navigate to Bloggs page | Click Bloggs link in navbar | Bloggs feed heading is visible |
| Navigate to Network page | Click Network link in navbar | Network page heading or user cards are visible |

**Minimum tests:** 2

---

### Category 2 — Filling Out Forms and Submitting Data

**Goal:** Automate inputting data into forms, submitting, and verifying the expected outcome.

| Test | Steps | Assertion |
|---|---|---|
| Login with valid credentials | Enter valid email + password, click Sign In | Redirected to `/home` |
| Register a new user | Fill all registration fields, submit | Success state (redirect or confirmation) |
| Create a new post | Log in, click Post button, enter content, submit | New post appears in the user's post list |

**Minimum tests:** 2

**Notes:**
- Use a dedicated test account — do not use a shared or admin account for registration tests
- Post content should be unique enough to verify it appeared (e.g. include a timestamp string)

---

### Category 3 — Form Validation

**Goal:** Verify that forms correctly reject invalid or incomplete input and display the appropriate error messages.

| Test | Steps | Assertion |
|---|---|---|
| Login — empty email | Leave email blank, submit | Error message is visible |
| Login — empty password | Leave password blank, submit | Error message is visible |
| Login — invalid email format | Enter `notanemail`, submit | Error message is visible |
| Login — wrong credentials | Enter valid format but incorrect credentials, submit | Generic error message is visible |
| Post Modal — empty content | Open Post Modal, leave content blank, attempt submit | Submit button is disabled or error is shown |

**Minimum tests:** 2

**Notes:**
- Assertions should check that the error element is present and visible in the DOM
- The page must not navigate away when validation fails

---

### Category 4 — Responsiveness and Cross-Browser Compatibility

**Goal:** Verify that the application layout and core functionality work correctly across different viewport sizes.

| Test | Steps | Assertion |
|---|---|---|
| Desktop layout — Home page | Set window to 1280×800, navigate to `/home` | Two-column layout is visible (left sidebar + right posts) |
| Mobile layout — Home page | Set window to 375×812, navigate to `/home` | Single-column layout is visible |
| Mobile layout — Login page | Set window to 375×812, navigate to `/login` | Form is visible and usable |
| Mobile navigation | Set window to 375×812, log in, check navbar | Mobile navigation renders correctly |

**Minimum tests:** 2

**Notes:**
- Use Selenium's `setWindowSize` command to control viewport dimensions
- Desktop breakpoint: width ≥ 768px
- Mobile breakpoint: width < 768px

---

### Category 5 — Automating Repetitive Tasks

**Goal:** Automate complete multi-step workflows that would otherwise require repeated manual effort.

| Test | Steps | Assertion |
|---|---|---|
| Full login workflow | Open app → navigate to login → enter credentials → submit | Home page loads with user's name visible |
| Full logout workflow | Log in → open user menu → click Logout → confirm | Redirected to `/login` |
| Create and verify post | Log in → open Post Modal → enter content → submit → check feed | Post is visible in Home post list |
| Create new user account | Navigate to register → fill all fields → submit → log in with new credentials | Successful login with new account |

**Minimum tests:** 2

---

## File Structure

```
Selenium/
└── codebloggs-tests.side       ← Single file containing all tests and suites
```

### Organising the `.side` File

Group tests into named suites within Selenium IDE to match the five categories:

| Suite Name | Category |
|---|---|
| `01 - Navigation` | Category 1 |
| `02 - Form Submission` | Category 2 |
| `03 - Form Validation` | Category 3 |
| `04 - Responsiveness` | Category 4 |
| `05 - Repetitive Tasks` | Category 5 |

---

## Acceptance Criteria

### Setup
- [ ] Selenium IDE is installed and CodeBloggs is running locally
- [ ] Base URL is set to `http://localhost:3000` in the Selenium IDE project
- [ ] All tests are saved in a single `.side` file at `./Selenium/codebloggs-tests.side`

### Category 1 — Navigation
- [ ] At least 2 navigation tests are written
- [ ] Each test asserts the correct page loaded after navigation
- [ ] Tests cover at least 2 different pages

### Category 2 — Form Submission
- [ ] At least 2 form submission tests are written
- [ ] Login with valid credentials test passes and asserts redirect to `/home`
- [ ] At least one other form (registration or post creation) is tested

### Category 3 — Form Validation
- [ ] At least 2 validation tests are written
- [ ] Tests cover empty field and invalid format scenarios
- [ ] Each test asserts the error message is visible
- [ ] Tests confirm the page does not navigate away on validation failure

### Category 4 — Responsiveness
- [ ] At least 2 responsiveness tests are written
- [ ] Tests use `setWindowSize` to control viewport
- [ ] Tests cover both desktop (≥768px) and mobile (<768px) breakpoints
- [ ] Assertions confirm layout elements are visible at each breakpoint

### Category 5 — Repetitive Tasks
- [ ] At least 2 repetitive task tests are written
- [ ] Full login workflow test passes end-to-end
- [ ] Full logout workflow test passes end-to-end

### Overall
- [ ] Minimum 10 tests across all 5 categories
- [ ] All tests pass when run against the local CodeBloggs application
- [ ] All tests are exported in the single `.side` file
- [ ] Tests are organised into named suites matching the 5 categories
