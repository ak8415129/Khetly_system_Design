# Khetly · Interactive System Design

A single-file, offline, interactive reference for the architecture of **Khetly**, an Indian farmland-rental marketplace where urban renters lease plots from verified farmers. Open `khetly-system-design.html` in any browser. There is nothing to install or build.

It is meant to do two jobs:

- **Onboarding reference:** anyone joining the project can see how requests, data and services fit together, then build on top of it.
- **Interview revision:** every part of the system can be explained from the page, including its weak spots.

> Synced to [`ak8415129/Khetly-v2`](https://github.com/ak8415129/Khetly-v2) at commit `f486f94` (PR #1: RAG assistant, reviews and disputes).

---

## What's inside

| Tab | What you can do |
|---|---|
| **Overview** | Clickable architecture map: React SPA, Firebase, Express API (middleware and modules), PostgreSQL, Groq LLM, Vercel and Render. Click a box or an arrow for details. Includes a "what changed in PR #1" summary. |
| **Request Flows** | 10 step-by-step sequence diagrams (play, pause, step with ← →): Google sign-in, profile completion, token refresh, public search, listing moderation, booking lifecycle, suspension, persona AI chat, RAG assistant, review and dispute. |
| **User Journeys** | Click paths for Visitor, Renter, Farmer and Admin. Each screen shows its route guard and API calls. |
| **Database** | ER map of 8 tables. Click a table for columns, indexes, relations and the endpoints that read or write it. Click an enum for its values and users. Status state machines for listings and bookings. |
| **API Reference** | Every endpoint with filters (module, auth, text, "only with known gaps"). Each row shows middleware, request and response, tables touched and notes. |
| **Auth & RBAC** | The three tokens, the exact `requireAuth` steps, the role capability matrix and security properties. |
| **Frontend** | Route table with guards, state management, the Axios 401 refresh queue and folder layout. |
| **Deploy & Config** | Vercel, Render, Supabase and Firebase topology, deploy order, local run commands and an environment-variable table. |
| **Interview Prep** | 20+ Q&As with flashcard-style reveal, a 30-second pitch, a scaling roadmap and a verified list of known gaps. |

### Handy features

- **Service highlighting:** click *Frontend, Firebase, Auth/JWT, API, Database, AI/LLM or Hosting* in the header to dim everything not involved.
- **Drill-down everywhere:** Database → table → schema; endpoint → tables it touches → schema; gap → affected endpoint.
- **Global search:** tables, columns, endpoints, routes, enums, gaps and questions.
- **Light and dark theme**, responsive layout, keyboard support (`Esc` closes the side panel, `←/→` steps through a flow).

---

## Tech stack described

React 18 · Vite · TypeScript · Tailwind · Zustand · React Query · Express · Prisma · PostgreSQL (Supabase) · Firebase Auth (Google) · own JWT access and refresh tokens · Groq (llama-3.3-70b) · `@xenova/transformers` (MiniLM, 384-d) · pgvector · Vercel · Render

---

## Known gaps highlighted in the page

The page lists issues found by reading the code, with evidence, severity and a suggested fix. Fix the first two before putting real users on the app:

- **Critical:** `PATCH /v1/users/me` accepts the whole request body, so a user can set their own `role` to `ADMIN`.
- **High:** `PATCH /v1/listings/:id` is also mass-assignable, so a farmer can approve their own listing.
- **High:** farmers can set their own KYC flags (`aadhaarVerified`, `bankVerified`).
- **Medium:** unvalidated review and dispute endpoints, no booking state machine, `GROQ_API_KEY` missing from the deploy config, and more.

Items already fixed (for example the `/farmer/listings` route mismatch) are kept in the list and marked **fixed**.

---

## Running it

```bash
# nothing to build: just open the file
open khetly-system-design.html        # macOS
xdg-open khetly-system-design.html    # Linux
start khetly-system-design.html       # Windows
```

It can also be hosted as a static page (GitHub Pages, Vercel, Netlify) or dropped into the repo's `docs/` folder.

## Keeping it up to date

All content lives in data structures at the top of the file's script, so updates are mostly editing arrays:

| Change in the app | Edit in the HTML |
|---|---|
| New or changed table or column | `TABLES` (and `ENUMS`) |
| New endpoint | `ENDPOINTS` |
| New request flow | `FLOWS` |
| New page or route | `ROUTES` and `JOURNEYS` |
| New env var | `ENVVARS` |
| Found or fixed a bug | `GAPS` (set `sev: 'fixed'`) |
| New interview question | `QA` |

Update the commit hash in the footer when you re-sync.

## Contributing

Open a PR that updates the data arrays together with the code change they describe, so the page never drifts from the repo. Keep it a single self-contained file with no external dependencies.

## License

MIT. Use it, fork it and adapt it for your own project.
