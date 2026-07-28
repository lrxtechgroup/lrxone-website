# TODO — lrxone-website

Living backlog. Check items off (or move to MEMORY.md as a dated entry) as they're
done — don't just accumulate; keep this reflecting real, current state.

## Fixed 2026-07-28 — Nav/hero "Register Interest" → real "Register" link

- [x] User request: nav and hero top-level CTAs now read "Register" /
      "Sign In" instead of "Register Interest" / "Sign In" — Register
      points straight at `app.lrxone.com/register` (a real route in the
      app frontend) instead of a `mailto:`. See MEMORY.md.
- [ ] Footer still has the old "Register Interest" → `mailto:` link,
      now inconsistent with the real "Register" link above it on the
      same page. Left as-is — user scoped the request to "the top" of
      the page specifically; flagged for the user rather than changed
      unprompted.

## Fixed 2026-07-27 (yet later same day) — Restructured into a dedicated sign-in page

- [x] Site rebuilt around Sign In as the primary CTA (site's stated
      purpose per user direction), full features/pricing/CTA-banner
      sections removed in favor of pointing visitors at
      `lrxtechgroup.com` for the complete pitch. See MEMORY.md.
- [x] Sign In restored as a real link (`app.lrxone.com/login`) since a
      page whose whole purpose is signing in shouldn't have a dead
      sign-in button — "Coming Soon" badge stays for honesty about
      current deploy state, "Register Interest" stays as the CTA for
      people without a workspace yet.

## Fixed 2026-07-27 (later same day) — Whole site marked "Coming Soon"

- [x] Nav, hero, pricing, CTA banner, and footer signup/sign-in CTAs all
      replaced with `mailto:` "Register Interest" links; added a
      "Coming Soon" hero badge. See MEMORY.md. **Superseded above** —
      Sign In itself is real again now that this page's whole purpose is
      signing in; Register Interest remains for everyone else.

## Fixed 2026-07-28

- [x] The two "full pitch" outbound links (hero note, footer "Features
      & Pricing") pointed at `lrxtechgroup.com/#products` (a summary
      card) instead of real dedicated content. Now point at
      `lrxtechgroup.com/one.html`, a real product page built there
      today. See MEMORY.md.

## Note for later

- [ ] When LRX One actually goes live (real production infra deployed),
      remove the "Coming Soon" badge and the hero note about it — the
      Sign In link itself needs no further change, it's already real.

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
