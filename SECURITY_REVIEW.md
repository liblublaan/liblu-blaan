# Liblu Blaan — production review

## What was checked in the HTML

- JavaScript syntax check with Node.js: passed.
- Duplicate HTML `id` values: none found in the static parse.
- Responsive CSS breakpoints: present at 980px, 700px, 650px, 560px, and 380px; the document has a viewport meta tag.
- Terms/Privacy/Content policy: removed from the homepage section and available through a footer-triggered modal.
- Word Editor: supports adding words and now includes edit/delete controls; cloud edits still depend on Supabase schema and RLS.
- Dynamic word text is HTML-escaped; media sources are restricted to HTTPS or expected base64 image/audio data URI types before insertion.
- No Supabase `service_role` key was found in the HTML. The configured key is a publishable key and will be visible to website visitors by design.

These are static checks, not a complete browser/device test, penetration test, or live Supabase verification.

## Production blockers / dependencies to verify

1. **Supabase schema and RLS:** verify `site_admins`, `dictionary_words`, and `timeline_items` exist, as well as `community_posts`, `post_comments`, and `post_reactions`. Confirm RLS is enabled and only the intended owner can create/edit/delete official content. Test as `anon` and as an authenticated non-owner, not just as the owner.
2. **Owner account:** confirm `liblublaan@gmail.com` is the intended owner, is registered in Supabase Auth, and has a matching `site_admins` row with role `owner`. The email check in JavaScript is not an authorization boundary.
3. **Media hosting:** the HTML form does not upload files to Supabase Storage. Configure a bucket with appropriate access rules and save HTTPS asset URLs before treating image/audio publishing as shared cloud functionality.
4. **Visitor abuse:** browser visitor IDs can be changed/spoofed. Add rate limiting/CAPTCHA, moderation/reporting, and an owner moderation process before high traffic.
5. **External dependency:** Supabase JS is loaded from a third-party CDN. For stronger supply-chain/reproducibility controls, pin an exact tested library version and consider self-hosting or adding a carefully tested integrity policy.
6. **Accessibility:** verify keyboard focus and Escape-to-close behavior for modals with real keyboard/screen-reader testing; static review alone does not establish accessibility compliance.
7. **Policies/legal:** validate the Terms and Privacy text against actual data collection, retention, provider settings, and applicable legal requirements.

## Manual smoke-test checklist after staging deployment

- [ ] Homepage, footer, policy modal, and navigation work at desktop, tablet, and narrow mobile widths.
- [ ] Search, category filters, pronunciation control, and empty search state work.
- [ ] Owner login succeeds with the intended Supabase Auth account; wrong email/password fails.
- [ ] A non-owner authenticated account cannot write/delete official content through the API.
- [ ] Add, edit, and delete a test dictionary word; reload in a separate browser and confirm the intended cloud behavior.
- [ ] Add and delete a test timeline item; reload in a separate browser.
- [ ] Create, edit, and delete a test cloud post; reload in an incognito window.
- [ ] Like, comment, reply, and share from an incognito window; verify limits/policies and visible moderation behavior.
- [ ] Try malicious-looking text in word/title/comment fields and confirm it renders as text, not markup or script.
- [ ] Test broken/offline Supabase responses and ensure visitors see a clear fallback instead of a blank page.
- [ ] Confirm media is hosted in the configured bucket and accessible only according to intended bucket policy.
- [ ] Back up production database and retain the previous live `index.html` for rollback.

## Deployment status

**Ready to commit as a reviewed static-site update; not yet certified production-ready.** The static checks passed, but live database policies, auth, cross-device behavior, Storage, and real browser/device behavior still need the checks above. Do not describe cloud features as verified until those tests pass.
