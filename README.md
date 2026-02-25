# 🏅 Sports Courses App — Next.js

A **Next.js full-stack application** for a sports courses platform, built step by step across homework assignments 13–18.

Homework project for lessons 13–18 at **AIT TR GmbH**.

---

## 🛠 Tech Stack

- Next.js (App Router)
- TypeScript
- Drizzle ORM
- PostgreSQL
- Zod (validation)
- Server Components & Server Actions

---

## 📋 Homework Progress

### HW 13 — Next.js Pages & Routing
Created a multi-page sports courses site using the `app/` directory with shared layout and nested routing.

**Pages:**

| Route | Description |
|-------|-------------|
| `/` | Home — welcome message, platform description, call to action |
| `/about` | About — project mission and goals |
| `/sports` | Courses list — Football, Tennis, Swimming with descriptions |
| `/sports/football` | Football course — topics, target audience |
| `/sports/tennis` | Tennis course — topics, who it's for |
| `/sports/swimming` | Swimming course — styles, suitable for all ages |

- Shared `Navbar` component with links: Home / About / Sports
- `/sports` has its own nested `layout.tsx` with sidebar links to each sport

---

### HW 14 — Server vs Client Components
Created two versions of the products page:
- **Server Component** — fetches data on the server, no client-side JS
- **Client Component** — uses `useState` / `useEffect`, interactive on the client

---

### HW 15 — Dynamic User Page
- Created a server component for `/users/client-version/[id]`
- **Additional:** fetched and displayed individual user data from an external API by `id`

---

### HW 16 — Add Product Form with Server Action
- Created a form for adding a product as a **Server Component**
- Used a **Server Action** to handle form submission directly on the server (no API route needed)

---

### HW 17 — Events API (Drizzle + PostgreSQL)
Created the `events` table with Drizzle ORM and implemented REST endpoints.

**Schema:**
```ts
export const eventsTable = pgTable('events', {
  id: integer().primaryKey().generatedAlwaysAsIdentity(),
  name: varchar({ length: 255 }).notNull(),
  description: text().notNull(),
})
```

**Endpoints:**

| Method | Route | Description |
|--------|-------|-------------|
| `GET` | `/api/events` | Get all events |
| `POST` | `/api/events` | Create a new event |

After creating the table: `npx drizzle-kit push`

---

### HW 18 — Zod Validation for Sports API
Added **Zod schema validation** to the `POST /api/sports` endpoint.

**Validation schema:**
```ts
const sportInsertSchema = z.object({
  title: z.string().min(5, "Minimal length is 5"),
  image: z.url({ message: "Invalid image URL" }),
  description: z.string().max(255, "Description should be less then 255"),
});
```

**Error responses:**
- `400` — invalid input with Zod issue details
- `500` — internal server error
- `201` — created successfully

---

## 🗄 Database Schema

```ts
// sports table
export const sportsTable = pgTable("sports", {
  id: integer().primaryKey().generatedAlwaysAsIdentity(),
  title: varchar({ length: 255 }).notNull(),
  image: varchar({ length: 255 }).notNull(),
  description: text().notNull(),
})

// events table
export const eventsTable = pgTable('events', {
  id: integer().primaryKey().generatedAlwaysAsIdentity(),
  name: varchar({ length: 255 }).notNull(),
  description: text().notNull(),
})
```

---

## 📡 API Endpoints

| Method | Route | Description | Validation |
|--------|-------|-------------|------------|
| `GET` | `/api/sports` | Get all sports | — |
| `POST` | `/api/sports` | Create a sport | ✅ Zod |
| `GET` | `/api/events` | Get all events | — |
| `POST` | `/api/events` | Create an event | — |

---

## 🚀 Getting Started

```bash
git clone https://github.com/AOgit/ait-sports-app.git
cd ait-sports-app
npm install
```

Configure `.env`:
```env
DATABASE_URL=postgresql://user:password@localhost:5432/sports_db
```

Push the DB schema and run:
```bash
npx drizzle-kit push
npm run dev
```

Open [http://localhost:3000](http://localhost:3000)

---

## 📁 Project Structure

```
src/
├── app/
│   ├── page.tsx                        # HW13: Home
│   ├── about/page.tsx                  # HW13: About
│   ├── sports/
│   │   ├── layout.tsx                  # HW13: Sports nested layout
│   │   ├── page.tsx                    # HW13: Sports list
│   │   ├── football/page.tsx           # HW13: Football course
│   │   ├── tennis/page.tsx             # HW13: Tennis course
│   │   └── swimming/page.tsx           # HW13: Swimming course
│   ├── users/client-version/[id]/      # HW15: Dynamic user page
│   └── api/
│       ├── sports/route.ts             # HW17+18: GET/POST + Zod validation
│       └── events/route.ts             # HW17: GET/POST events
├── components/
│   └── Navbar.tsx                      # HW13: Shared navigation
└── db/
    ├── index.ts                        # Drizzle DB connection
    └── schema.ts                       # HW17: sportsTable, eventsTable
drizzle/
└── 0000_faulty_blob.sql                # Migration
```
