# Aleksandr Lerko

Fullstack developer based in Tallinn, Estonia. I build web applications end to end — from the database schema and API to the interface a user actually clicks.

## Contacts

- Location: Tallinn, Estonia
- Email: [alexandr.lerko@gmail.com](mailto:alexandr.lerko@gmail.com)
- GitHub: [github.com/allerk](https://github.com/allerk)
- LinkedIn: [aleksandr-lerko](https://www.linkedin.com/in/aleksandr-lerko-a34863209/)

## About

I graduated in IT Systems Development at TalTech and started in backend-heavy work — C#/.NET and Java — before moving towards the full stack. Today I am most productive in TypeScript: SvelteKit and React on the front, Node.js and Cloudflare Workers on the back, PostgreSQL or D1 underneath.

What I care about in a codebase: explicit data flow, typed boundaries, migrations that can be replayed from zero, and environments that do not leak into each other. I prefer shipping a narrow feature that is fully wired — validation, error states, deployment — over a broad one that is half done.

I joined The Rolling Scopes School to close the gaps that self-taught practice leaves behind and to work against a review process instead of only against my own judgement.

## Skills

| Area | Technologies |
| --- | --- |
| Languages | TypeScript, JavaScript, C#, Java, Python, SQL |
| Frontend | Svelte 5 / SvelteKit, React, HTML5, CSS3, Tailwind CSS |
| Backend | Node.js, Cloudflare Workers, ASP.NET Core, Spring Boot |
| Data | PostgreSQL, Cloudflare D1, Drizzle ORM, Entity Framework |
| Tooling | Git, Vite, Docker, Vitest, Wrangler, GitHub Actions |

## Code Examples

A typed, cached fetch helper — deduplicates concurrent calls for the same key so a component tree can request the same resource without stampeding the API:

```ts
type Loader<T> = () => Promise<T>;

const inflight = new Map<string, Promise<unknown>>();

export function dedupe<T>(key: string, load: Loader<T>): Promise<T> {
  const running = inflight.get(key);
  if (running) return running as Promise<T>;

  const promise = load().finally(() => inflight.delete(key));
  inflight.set(key, promise);
  return promise;
}
```

Server-side validation of a form submission before it reaches the database:

```ts
import { z } from 'zod';

const requestSchema = z.object({
  name: z.string().trim().min(2).max(80),
  email: z.string().email(),
  phone: z.string().regex(/^\+?\d{7,15}$/),
  message: z.string().trim().max(2000).optional()
});

export async function submit(formData: FormData) {
  const parsed = requestSchema.safeParse(Object.fromEntries(formData));

  if (!parsed.success) {
    return { status: 400, errors: parsed.error.flatten().fieldErrors };
  }

  await db.insert(requests).values(parsed.data);
  return { status: 201 };
}
```

## Experience

**Intern Software Engineer — GrabCAD / Stratasys, Tallinn**

Worked on internal web tooling for the additive manufacturing platform: feature work across the stack, bug triage, and code review inside an established engineering process.

**Selected projects**

- **DPFLAB** — trilingual (RU/ET/EN) marketing site and admin panel for a DPF cleaning service. SvelteKit 2, Svelte 5, Tailwind CSS 4, Drizzle ORM on Cloudflare D1, images in R2, admin protected by Cloudflare Access. Includes a three-step qualifying form with server-side validation, a request pipeline with statuses and loss reasons, and Meta Pixel plus Conversions API with shared event IDs for deduplication. Separate local, develop and production environments. — [github.com/allerk/dpflab](https://github.com/allerk/dpflab)
- **Nullam** — .NET technical assignment for RIK (Estonian Centre of Registers and Information Systems): C# backend with a TypeScript frontend for event and participant management.
- **Weatherman** — weather application on Angular and Spring Boot, built as a technical assignment for CGI.
- **Getpart companies aggregator** — thesis prototype aggregating auto-parts suppliers; Python scrapers feeding a JavaScript service.

## Education

**Tallinn University of Technology (TalTech)** — IT Systems Development

Coursework archived on GitHub: JavaScript (2022), Distributed Systems in C# (2022), C# (2021).

**The Rolling Scopes School** — Fullstack Engineering, in progress. Previously completed RS School React and Node.js modules.

## English

**B2 — Upper-Intermediate.** Daily working language: documentation, code review, technical assignments and correspondence. Comfortable in written communication and technical discussion; continuing to work on spoken fluency.
