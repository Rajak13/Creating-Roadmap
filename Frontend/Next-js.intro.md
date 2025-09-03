# Next.js Learning Notes

## 1. Getting Started
- Next.js is a **React framework** for building fast, scalable web applications.
- Provides **server-side rendering (SSR)** and **static site generation (SSG)** out-of-the-box.
- Supports **API routes** to create backend endpoints directly in the project.
- File-based routing: pages in the `/pages` or `/app` folder automatically become routes.
- `next dev` → starts development server.  
- `next build` → builds app for production.  
- `next start` → runs the production build.

---

## 2. CSS and Styling
- Supports **global CSS**, **CSS modules**, and **Tailwind CSS** integration.
- Global CSS must be imported in `app/layout.tsx` or `pages/_app.tsx`.
- CSS Modules allow **component-level scoped styles**:
  ```ts
  import styles from './Button.module.css';
  <button className={styles.primary}>Click me</button>
Supports Sass/SCSS and other CSS preprocessors.

Can use styled-components or emotion for CSS-in-JS solutions.

## 3. Optimizing Images and Fonts
- next/image component automatically optimizes images:
- Supports lazy loading, resizing, and modern formats (WebP/AVIF).

Example:

```tsx
Copy code
import Image from 'next/image';

<Image src="/profile.png" alt="Profile" width={200} height={200} />
```
Built-in Next.js font optimization via next/font.

Automatically loads Google Fonts with minimal CSS.

Reduces layout shifts and improves performance.

## 4. Key Learnings

- Next.js simplifies React app development with routing, SSR, and SSG.

- Styling can be modular, global, or utility-first (Tailwind).

- Image and font optimization is automatic, improving performance and SEO.

## Commands to remember:
```tsx
pnpm dev → start dev server

pnpm build → build production

pnpm start → run production
```

# Next.js Basics: Layouts, Pages, Navigation, and Database Setup

## 1. Creating Pages in Next.js
In Next.js, the **`app/`** directory (or **`pages/`** in older versions) defines routes automatically based on the file structure.
```tsx
Example:
app/
├─ page.tsx → /
├─ about/
│ └─ page.tsx → /about
└─ contact/
└─ page.tsx → /contact
```

- Each `page.tsx` file exports a React component that represents the page content.
- Nested folders create nested routes.

```tsx
// app/about/page.tsx
export default function AboutPage() {
  return <h1>About Us</h1>;
}
```
## 2. Creating Layouts
Layouts allow you to define shared UI (like headers, sidebars, footers) across multiple pages.

layout.tsx wraps around the page.tsx inside the same folder.

Useful for consistent design and structure.

Example:
```tsx
app/
 ├─ layout.tsx       → Root layout for all pages
 ├─ dashboard/
 │   ├─ layout.tsx   → Layout for dashboard pages only
 │   └─ page.tsx     → `/dashboard`
```

```tsx
// app/layout.tsx
export default function RootLayout({ children }: { children: React.ReactNode }) {
  return (
    <html lang="en">
      <body>
        <header>My Navbar</header>
        <main>{children}</main>
        <footer>© 2025 My App</footer>
      </body>
    </html>
  );
}
```
## 3. Navigating Between Pages
Navigation is handled using the Link component from next/link.

```tsx
import Link from "next/link";

export default function HomePage() {
  return (
    <nav>
      <Link href="/about">About</Link>
      <Link href="/contact">Contact</Link>
    </nav>
  );
}
```
- This prevents full page reloads and enables client-side transitions.

- For programmatic navigation, use the useRouter hook from next/navigation.

```tsx
"use client";
import { useRouter } from "next/navigation";

export default function NavigateButton() {
  const router = useRouter();

  return <button onClick={() => router.push("/dashboard")}>Go to Dashboard</button>;
}
```
## 4. Setting Up a Database in Next.js
- Next.js does not include a database by default, but you can connect to one (e.g., PostgreSQL, MySQL, MongoDB, etc.) using an ORM or query client.

Example with PostgreSQL and postgres.js
Install package:

```bash
npm install postgres
```
Create a database utility file:

```ts
// lib/db.ts
import postgres from "postgres";

const sql = postgres(process.env.DATABASE_URL!, { ssl: "require" });

export default sql;
```
Query inside server components or API routes:

```ts
// app/api/users/route.ts
import sql from "@/lib/db";

export async function GET() {
  const users = await sql`SELECT * FROM users`;
  return Response.json(users);
}
```
Use in your pages:

```tsx
// app/page.tsx
import sql from "@/lib/db";

export default async function HomePage() {
  const users = await sql`SELECT name FROM users`;

  return (
    <div>
      <h1>User List</h1>
      <ul>
        {users.map((u) => (
          <li key={u.name}>{u.name}</li>
        ))}
      </ul>
    </div>
  );
}
```
## Key Takeaways

- Pages define routes automatically from folder structure.

- Layouts provide shared UI across pages.

- Navigation uses Link or useRouter.

- Database setup requires external packages (like postgres, Prisma, or Mongoose).