# Liblu Blaan — GitHub Pages deployment

This package contains the reviewed static site entry point and deployment/security checklist.

## Files

- `index.html` — the website. GitHub Pages must serve this file from the repository root.
- `SECURITY_REVIEW.md` — review findings, remaining production gates, and smoke-test checklist.
- `.nojekyll` — prevents Jekyll processing; optional for this plain HTML site.

## Deploy to GitHub Pages

1. Download/extract this package.
2. In the `liblu-blaan` repository, back up the current live `index.html` or make a branch first.
3. Replace the repository-root `index.html` with this package's `index.html`. Keep the filename exactly `index.html`.
4. Add `README.md`, `SECURITY_REVIEW.md`, and `.nojekyll` at the repository root if desired.
5. Review the diff before committing. Do not commit any Supabase `service_role` or secret key.
6. Commit to the configured deployment branch (`main`, root `/` according to the current project setup).
7. In GitHub, open **Settings → Pages** and confirm the source is the expected branch and `/ (root)`.
8. After deployment, hard-refresh the public site and complete the checklist in `SECURITY_REVIEW.md`.

## Supabase requirements before calling this production-ready

The browser code uses the configured Supabase project URL and a **publishable** key. A publishable key is expected to be visible in a static website; database safety depends on Row Level Security (RLS), table grants, and policies—not on hiding this key.

The site expects these database objects to be present and correctly permissioned:

- `site_admins` with the authenticated owner's `user_id` and `role = 'owner'`
- `dictionary_words`
- `timeline_items`
- `community_posts`
- `post_comments`
- `post_reactions`

The previously prepared community SQL only creates the community-post/comment/reaction tables and **assumes `site_admins` already exists**. It does not provision the dictionary or timeline tables. Do not run it blindly against production; inspect the existing schema and policies first, back up the database, and test on a staging project.

## Important behavior

- Dictionary words and timeline items are shared across visitors only when the corresponding Supabase tables and policies work. If the cloud query fails, the page can fall back to browser-local data; local edits are not automatically shared.
- The owner login is Supabase Auth. The frontend email check is only a UI check; RLS must enforce owner-only writes.
- Image/audio file selection is not a cloud-storage upload. Data-URI media can be local to a browser and does not become a reliable shared asset. Configure Supabase Storage and store validated HTTPS asset URLs before relying on media publishing for everyone.
- Likes/comments use browser-generated visitor IDs that can be spoofed. Add abuse controls/rate limits and moderation before a high-traffic launch.
