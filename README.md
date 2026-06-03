# Portfolio Admin

Admin portal for managing the portfolio content used by the public CV website.

This repository is intended to host the static GitHub Pages output generated from the main Nx monorepo admin app:

- Source app: `apps/admin`
- Build output: `dist/apps/admin`
- Backend: Supabase Auth, PostgreSQL, and Storage
- Deployment target: separate GitHub Pages repository

## What The Admin App Does

- Signs in with Supabase Email/Password authentication.
- Protects all admin routes behind an authenticated session.
- Manages portfolio content stored in Supabase PostgreSQL.
- Uploads portfolio assets to Supabase Storage.
- Builds as a static React/Vite app that can be deployed to GitHub Pages.

## Content Managed

The admin CMS supports CRUD operations for:

- Profile content: `site_profile`
- Skills: `skills`
- Work experience: `experiences`
- Projects: `projects`
- Certificates: `certifications`
- Achievements: `achievements`
- Testimonials: `testimonials`
- Blog posts: `blog_posts`

The first implementation uses a table-driven CMS. Simple values use normal inputs, booleans use checkboxes, arrays use newline-separated text areas, and JSON fields use JSON text areas.

## Storage Managed

Uploads go to the existing Supabase Storage bucket named `portfolio`.

Supported paths:

```text
profile/ayi-hardiyanto-profile.png
projects/{projectSlug}/thumbnail.webp
certificates/{certificateSlug}/certificate.pdf
certificates/{certificateSlug}/preview.webp
```

Uploaded files are replaced with `upsert: true`, and the admin UI displays the public URL after upload.

## Supabase Setup

The admin portal is browser-only and must never use a Supabase service role key. It uses the public anon key, with write access controlled by Supabase Auth and Row Level Security.

In the main monorepo, run this SQL in the Supabase SQL Editor:

```text
docs/supabase-admin-policies.sql
```

Then add your Supabase Auth user UUID to the admin allowlist:

```sql
insert into public.app_admins (user_id)
values ('YOUR_AUTH_USER_UUID')
on conflict (user_id) do nothing;
```

The SQL adds:

- `public.app_admins`
- `public.is_admin()`
- Admin read/write/delete policies for portfolio content tables
- Admin upload/update/delete policies for the `portfolio` storage bucket

## Environment Variables

The admin app needs these Vite variables at build time:

```bash
VITE_SUPABASE_URL=https://your-project.supabase.co
VITE_SUPABASE_ANON_KEY=your-public-anon-key
```

For GitHub Actions deployment from the main monorepo, configure:

```text
VITE_SUPABASE_URL
VITE_SUPABASE_ANON_KEY
ADMIN_PAGES_REPOSITORY
ADMIN_PAGES_TOKEN
```

`ADMIN_PAGES_REPOSITORY` should point to this repository, for example:

```text
your-github-username/my-online-cv-admin
```

`ADMIN_PAGES_TOKEN` must have permission to push to this repository.

## Local Development

Run these commands from the main monorepo, not from this deployment repository:

```bash
npm install
npm run dev:admin
```

Useful commands:

```bash
npm run lint:admin
npm run build:admin
npm run preview:admin
```

The production admin artifact is generated at:

```text
dist/apps/admin
```

## Deployment

The main monorepo contains a workflow:

```text
.github/workflows/deploy-admin-pages.yml
```

That workflow:

1. Installs dependencies.
2. Runs `npm run lint:admin`.
3. Runs `npm run build:admin`.
4. Publishes `dist/apps/admin` to this repository's `gh-pages` branch.

In this repository, enable GitHub Pages with:

```text
Settings -> Pages -> Deploy from branch -> gh-pages
```

## Security Notes

- Do not commit `.env` files.
- Do not expose `SUPABASE_SERVICE_ROLE_KEY` in the admin app.
- Keep write access restricted through RLS and `public.app_admins`.
- Remove users from `public.app_admins` to revoke admin access.
- Keep the public portfolio app read-only through published-row policies.

## Source Of Truth

This repository is for the deployed admin artifact. The source code lives in the main portfolio monorepo under:

```text
apps/admin
packages/shared-types
packages/shared-services
packages/supabase
docs/supabase-admin-policies.sql
```
