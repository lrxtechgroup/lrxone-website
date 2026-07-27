# TODO — lrxone-website

Living backlog. Check items off (or move to MEMORY.md as a dated entry) as they're
done — don't just accumulate; keep this reflecting real, current state.

## Fixed 2026-07-27 — Real Privacy Policy / Terms of Service pages

- [x] Built `privacy.html` and `terms.html` (real, user-supplied POPIA
      content — Information Officer, registered address, data
      processors, retention periods; not fabricated). Footer links in
      `index.html` updated from `mailto:` placeholders to the real
      pages. See MEMORY.md for detail.
- [x] The app frontend's (`lrxtechgroup/lrxone`) signup form linked to
      `/terms`/`/privacy` on `app.lrxone.com`, which don't exist as
      routes there — fixed in that repo to point here instead.

## Fixed 2026-07-25

- [x] Footer `/privacy` and `/terms` links pointed at pages that don't
      exist. Not writing fabricated legal content — pointed both at a
      `mailto:` instead, which at least goes somewhere real. Real Privacy
      Policy / Terms of Service pages still need to be written (by the
      site owner, not autonomously) and linked once they exist.
- [x] Added a favicon (inline SVG, matches the site's branding) and
      `robots.txt`.
- [x] Footer copyright year was a hardcoded "2025" — made it dynamic via
      a small inline script so it doesn't need a yearly manual bump.
- [x] Added `sitemap.xml`, linked from `robots.txt`.

## Not investigated yet

- [ ] No deployment/hosting config found in the repo (no CI/CD workflow) —
      unclear how/where this actually gets published. Worth documenting
      once known.
