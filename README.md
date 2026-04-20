# CodeBloggs — DevOps Module (Module 15)

## Project Title

**CodeBloggs DevOps** — Applying DevOps practices to the CodeBloggs MERN application: Lighthouse performance auditing, Selenium automated browser testing, and AWS cloud research.

---

## Description

This project is **Module 15** of the Full-Stack Development Program, completed in the role of Junior Developer at **Genesis Solutions**.

Rather than adding new features, this module focuses entirely on quality, tooling, and operations. DevOps practices are applied to the existing CodeBloggs blogging platform across three tracks:

1. **Lighthouse Auditing** — Audit the Login and Home pages, refactor based on findings, re-audit to demonstrate measurable improvement, and benchmark against an external website.
2. **Selenium Testing** — Build an automated browser test suite of 10+ tests across 5 categories using Selenium IDE.
3. **AWS Research** — Survey the DevOps engineering ecosystem via AWS Skill Builder and research CLI vs. Console trade-offs.

No new features were added. All code changes are scoped to targeted refactoring of the Login and Home pages only, and every change is traceable to a specific Lighthouse recommendation.

---

## Tech Stack

| Layer | Technology |
|---|---|
| Frontend | React.js |
| Backend | Node.js, Express.js |
| Database | MongoDB Atlas |
| Authentication | JSON Web Tokens (JWT) |
| Auditing | Google Lighthouse (Chrome DevTools) |
| Browser Testing | Selenium IDE |
| Cloud Research | AWS (Skill Builder, CLI, Console) |
| Version Control | Git, GitHub |

---

## Project Structure

```
CodeBloggs-DevOps/
├── client/                         # React frontend
│   ├── public/
│   └── src/
│       ├── pages/
│       │   ├── Login.jsx           # Refactored for Lighthouse improvements
│       │   └── Home.jsx            # Refactored for Lighthouse improvements
│       └── components/
├── server/                         # Express backend
│   ├── routes/
│   ├── models/
│   └── controllers/
├── Lighthouse/                     # Lighthouse audit reports
│   ├── v1/                         # Pre-refactor reports
│   │   ├── login-v1.html
│   │   └── home-v1.html
│   ├── v2/                         # Post-refactor reports
│   │   ├── login-v2.html
│   │   └── home-v2.html
│   └── benchmark/                  # External site comparison reports
├── Selenium/                       # Automated test suite
│   └── codebloggs-tests.side       # Single Selenium IDE test file
├── AWS-Research/                   # AWS and DevOps ecosystem research
│   ├── aws-overview.md
│   └── cli-vs-console.md
├── LeetCode-Challenges/            # LeetCode solution screenshots
│   └── <challenge-name>.png
├── AI-Specification/               # AI spec and feature spec documents
├── CONCEPTS.md                     # 3 challenging concepts with explanations
├── .env.example                    # Environment variable template
└── README.md
```

---

## Setup Instructions

### Prerequisites

- Node.js v18+
- npm or yarn
- MongoDB Atlas account (shared cluster — do **not** modify the schema)
- Google Chrome (for Lighthouse audits)
- Selenium IDE browser extension

### 1. Clone the Repository

```bash
git clone https://github.com/<your-username>/CodeBloggs-DevOps.git
cd CodeBloggs-DevOps
```

### 2. Install Dependencies

```bash
# Install server dependencies
cd server
npm install

# Install client dependencies
cd ../client
npm install
```

### 3. Configure Environment Variables

Copy the example file and fill in your values:

```bash
cp .env.example .env
```

See the [Environment Variables](#environment-variables) section below for details.

### 4. Run the Application

Open two terminals:

```bash
# Terminal 1 — Start the backend server
cd server
npm run dev

# Terminal 2 — Start the React frontend
cd client
npm start
```

The app will be available at `http://localhost:3000`.

---

## Environment Variables

Create a `.env` file in the `/server` directory using the following template:

```env
# MongoDB Atlas connection string
MONGODB_URI=mongodb+srv://<username>:<password>@<cluster>.mongodb.net/<dbname>

# JWT secret key
JWT_SECRET=your_jwt_secret_here

# Server port
PORT=5000

# Client origin (for CORS)
CLIENT_ORIGIN=http://localhost:3000
```

> **Note:** Never commit your `.env` file. It is listed in `.gitignore`.

---

## API Documentation

### Authentication

| Method | Endpoint | Description | Auth Required |
|---|---|---|---|
| POST | `/api/auth/register` | Register a new user | No |
| POST | `/api/auth/login` | Log in and receive a JWT | No |

### Blog Posts

| Method | Endpoint | Description | Auth Required |
|---|---|---|---|
| GET | `/api/posts` | Get all blog posts | No |
| GET | `/api/posts/:id` | Get a single post by ID | No |
| POST | `/api/posts` | Create a new post | Yes |
| PUT | `/api/posts/:id` | Update a post | Yes |
| DELETE | `/api/posts/:id` | Delete a post | Yes |

### Request / Response Format

All requests and responses use `application/json`.

**Login request body:**
```json
{
  "email": "user@example.com",
  "password": "yourpassword"
}
```

**Login response:**
```json
{
  "token": "<jwt_token>",
  "user": {
    "id": "abc123",
    "username": "johndoe",
    "email": "user@example.com"
  }
}
```

---

## Lighthouse Auditing

Audits were run in **Chrome DevTools** using **Navigation mode** and **Desktop device** configuration.

### Pages Audited
- Login page (`/login`)
- Home page (`/`)

### Process
1. **v1** — Baseline audit recorded before any changes.
2. **Refactor** — Login and Home pages updated based on specific Lighthouse recommendations.
3. **v2** — Re-audit run to confirm measurable improvement in at least one category.
4. **Benchmark** — Both v1 and v2 results compared against an equivalent external website.

Reports are saved in the [`/Lighthouse`](Lighthouse/) directory.

---

## Selenium Testing

The automated test suite is located at [`/Selenium/codebloggs-tests.side`](Selenium/codebloggs-tests.side).

- **10+ tests** across **5 categories**
- All tests run in a single `.side` file
- All tests must pass before submission

### Test Categories
1. Navigation
2. Authentication (Login / Logout)
3. Blog Post interactions
4. Form validation
5. UI visibility / layout checks

To run tests, open the `.side` file in the Selenium IDE browser extension and execute the full suite.

---

## AWS Research

Research documents are located in [`/AWS-Research`](AWS-Research/).

Topics covered:
- Overview of core AWS services relevant to web application deployment
- AWS DevOps ecosystem: CodePipeline, CodeBuild, CodeDeploy, Elastic Beanstalk, EC2, S3
- CLI vs. Console trade-offs — when to use each in a professional workflow
- Notes from AWS Skill Builder (free tier) courses

---

## Branching Model

```
main         ← final stable version (graded branch)
└── dev      ← integration branch
    └── feature/*  ← individual feature/task branches
```

- All work is done on `feature/*` branches created from `dev`.
- Feature branches are merged back into `dev` via pull request.
- `dev` is merged into `main` before final submission.
- No direct commits to `main`.

---

## Author

**Charles Winfield**
Junior Developer — Genesis Solutions (Full-Stack Development Program, Module 15)
