# TODO — lrxone-website

Living backlog. Check items off (or move to MEMORY.md as a dated entry) as they're
done — don't just accumulate; keep this reflecting real, current state.

## Fixed 2026-07-28 (final one today) — Reconciled legal pages with lrxtechgroup-website

- [x] `cancellation-policy.html` Section 3 still said "or to the free
      Starter tier" — the STARTER-pricing pass a few commits back only
      touched `terms.html`, missing this one. Fixed, and added an
      explicit "we don't offer a free tier" statement so it can't drift
      back silently. See MEMORY.md.
- [x] `terms.html`, `refund-policy.html`, `cancellation-policy.html` all
      routed every contact point to `sales@lrxtechgroup.com` — brought in
      line with `lrxtechgroup-website`'s now-current pages, which split
      billing/finance-related contact (refund requests, cancellation
      requests, each policy's general contact section) to
      `billing@lrxtechgroup.com`. `terms.html`'s general "Contact us"
      section now explicitly splits general vs. billing questions, same
      as the other site.
- [ ] **Not changed, deliberately**: this site's pages stay LRX One
      Core-specific (product name, `app.lrxone.com` references, nav
      branding) rather than being generalised to also cover LRX One
      Billing the way `lrxtechgroup-website`'s versions are — that's a
      legitimate difference given this domain is the single-product
      sign-in site, not the corporate umbrella. Only the underlying
      *rules* (no free tier, refund window, cancellation timing, contact
      routing) needed to match; the copy scope doesn't.

## Fixed 2026-07-28 (very last one today) — STARTER tier wording updated (no longer free)

- [x] `terms.html`'s payment clause no longer calls STARTER "free" — see
      MEMORY.md and `lrxone`'s own MEMORY.md for the real pricing change
      (R199/mo).

## Fixed 2026-07-28 (last one today) — Refund Policy + Cancellation Policy (PayFast merchant verification)

- [x] Built `refund-policy.html` and `cancellation-policy.html` — PayFast's
      merchant account verification asked for dedicated Terms and
      Conditions, Refund Policy, and Cancellation Policy pages. Terms was
      satisfied by the existing `terms.html`; the other two didn't exist
      as standalone pages (only a brief clause inside terms.html). See
      MEMORY.md — the actual policy stance (no refunds except billed-after-
      cancellation) was confirmed with the user, not invented.
- [x] Linked both from `index.html`'s footer and cross-referenced from
      `terms.html`'s payment section.
- [ ] Confirm with PayFast that these two pages (plus the existing
      terms.html) actually satisfy their verification checklist — built to
      a reasonable, standard interpretation of what a payment processor
      checks for, not against PayFast's own written requirements
      (not available in this session).

## Fixed 2026-07-28 (later same day) — Renamed to "LRX One Core"

- [x] Flagship product renamed "LRX One" → "LRX One Core" everywhere on
      this site (nav-brand wordmark, footer brand-mark, title/meta,
      hero desc, learn-more strip, footer mailto subject) and in
      `privacy.html`/`terms.html`'s legal text. See MEMORY.md.

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
