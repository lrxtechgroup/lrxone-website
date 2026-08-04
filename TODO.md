# TODO — lrxone-website

Living backlog. Check items off (or move to MEMORY.md as a dated entry) as they're
done — don't just accumulate; keep this reflecting real, current state.

## Fixed 2026-08-04 (nav horizontal padding capped for large monitors)

- [x] `nav { padding: 0 5%; }` → `padding: 0 clamp(20px, 3vw, 56px);`
      across all 5 pages, same fix as `lrxtechgroup-website`. See
      MEMORY.md.

## Fixed 2026-08-04 (nav wordmark scaled to match logo height)

- [x] `.nav-brand .lrx`/`.one` ("LRX One") scaled to 54px so it spans
      the same 38px height as the logo icon, across all 5 pages.
      Stacked (two-line) variant was tried and explicitly rejected —
      single line is final. See MEMORY.md.

## Fixed 2026-08-04 (footer wordmark "One" recolored to white)

- [x] Footer's `.brand-mark` ("LRX One") split into gold "LRX" + white
      "One", matching the nav. See MEMORY.md.

## Fixed 2026-08-04 (nav wordmark "One" recolored to white)

- [x] `index.html`'s `.nav-brand .one` was `var(--gold)` (all-gold nav,
      inconsistent with the other 4 pages) — now `var(--white)`,
      matching everywhere else. See MEMORY.md.

## Fixed 2026-08-04 (nav heading optically re-centered)

- [x] `.nav-brand .lrx` and `.nav-brand .one` given
      `transform: translateY(9px)` across all 5 pages so the heading's
      visual weight lines up with the logo's. See MEMORY.md.

## Fixed 2026-08-04 (nav logo enlarged, heading kept centered)

- [x] `.nav-brand-icon` height 26px → 38px across all 5 pages; heading
      text stays vertically centered beside it via the existing
      `align-items: center` on `.nav-brand`. See MEMORY.md.

## Fixed 2026-08-04 (logo recolored to match brand gold, all 5 repos)

- [x] Recolored the logo mark to the site's actual `--gold`/
      `--gold-dark` palette instead of its original off-brand warm
      gradient. Same fix across all 5 LRX repos. See MEMORY.md and
      `lrxtechgroup-website`'s MEMORY.md for the method.

## Fixed 2026-08-02 (real logo added, both sites)

- [x] Added the real LRX Tech Group icon mark to the favicon
      (`/favicon.ico` + `/images/favicon-*.png` + `apple-touch-icon.png`)
      and to the nav (`.nav-brand-icon` image, new — this nav was
      text-only before) across all 5 pages. See this file's and
      `lrxtechgroup-website`'s MEMORY.md for extraction details and
      verification.

## Fixed 2026-07-31 (org-wide rename) — "LRX One Core" → "LRX One Hive"

- [x] Renamed across all 5 pages (`index.html`, `terms.html`,
      `privacy.html`, `refund-policy.html`, `cancellation-policy.html`),
      including the split gold/white span styling. See MEMORY.md.

## Fixed 2026-07-30 (nav wordmark weight matched to footer)

- [x] Nav wordmark's "One" changed from thin (300, letter-spaced) to
      bold (900, no extra letter-spacing) on all 5 pages, matching
      `index.html`'s footer "LRX One" brand-mark design. See MEMORY.md.

## Fixed 2026-07-30 (branding + footer cleanup)

- [x] Removed the "LRX | ONE" pipe separator sitewide (5 pages) — now
      reads "LRX One". See MEMORY.md.
- [x] Removed the trailing arrow (→) from `index.html`'s "learn more"
      strip sentence.
- [x] Removed the bare `sales@lrxtechgroup.com` line from
      `index.html`'s footer bottom.
- [x] Footer "Contact" link on `index.html` now points to
      `https://lrxtechgroup.com/contact.html` instead of a bare
      `mailto:sales@...` link.

## Fixed 2026-07-29 (footer links trimmed) — "Sign In"/"Register Interest" removed

- [x] Removed "Sign In" and "Register Interest" from the footer link
      list on `index.html`. See MEMORY.md.

## Fixed 2026-07-29 (brand/copy pass) — Logo colour, hero copy, colour scheme, dashes, arrow

- [x] Nav/footer "LRX | ONE" logo now fully gold (was gold `LRX` + white
      `ONE`). See MEMORY.md.
- [x] Removed "Product Sign-In" eyebrow + its line from next to the
      "Coming Soon" badge.
- [x] Hero headline fixed: "Every LRX Product." → "Every LRX One
      Product."
- [x] Applied lrxtechgroup.com's gold-"LRX One"/white-product-name
      colour scheme to hero subtitle, hero description, and footer
      brand paragraph.
- [x] Removed em dashes from hero description, `<title>`, and footer
      brand paragraph; rewrote hero description grammar as a result.
- [x] Removed the arrow from the hero's "Sign In →" button.

## Fixed 2026-07-28 (mobile audit) — Nav crowding on the reworked hero at phone widths

- [x] Ran the same mobile overflow sweep done on `lrxtechgroup-website`
      across this site's 5 pages at 375px/768px. `index.html` didn't
      technically overflow (no horizontal scrollbar), but the nav was
      genuinely cramped at 375px: the "LRX | ONE" logo and the "About"
      link had zero gap between them (touching directly), and the "Sign
      In" button clipped 1px past the viewport edge. Root cause:
      `.nav-right`'s three items (About, Register, Sign In) had no
      responsive handling at all, unlike `lrxtechgroup-website`'s
      equivalent nav (which hides secondary links under 768px, keeping
      just the primary CTA). Added the same `@media (max-width: 768px) {
      .nav-link { display: none; } }` rule here for consistency — only
      the gold Sign In button remains at phone widths now.
- [x] Also visually re-checked the reworked pillars band (the two
      product tiles from the earlier hero rework) at 375px — renders
      cleanly, no changes needed there.
- [x] The other 4 pages on this site (terms/privacy/refund/cancellation)
      were all already clean — their minimal single-link nav never had
      this problem.

## Fixed 2026-07-28 (second correction) — LRX One is the umbrella product, not just a login

- [x] User corrected the framing from the two entries directly below:
      "LRX One" isn't merely the shared account system/login behind
      Core and Billing — it's LRX Tech Group's actual umbrella product
      (a suite), with Core and Billing as its two products/modules,
      sign-in being one thing it does, not what it *is*. Every
      "shared account system" / "single sign-in for..." description
      across `index.html`, `terms.html`, and `privacy.html` rewritten to
      "LRX One is LRX Tech Group's product suite, comprising..." instead.
      `refund-policy.html`/`cancellation-policy.html` needed no change —
      they never defined what LRX One *is*, only referenced it by name.
- [ ] **Flagged, not decided**: `lrxtechgroup-website`'s main site
      (`index.html`) presents LRX One Core and LRX One Billing as two
      independent, equal product cards with no "LRX One" umbrella
      framing anywhere — checked, confirmed zero mentions. If LRX One is
      genuinely the umbrella brand, that site's product section may need
      restructuring (a shared "LRX One" heading above both cards, or
      merging them into one section with two modules) rather than just a
      wording fix — asked the user how far to take that rather than
      redesigning the primary marketing site's IA unilaterally.

## Fixed 2026-07-28 (correction) — app.lrxone.com is the shared login for both products

- [x] User corrected an assumption from the entry directly below this
      one: this site (and `app.lrxone.com`, the account system it signs
      you into) is **not** LRX One Core-exclusive — it's the shared login
      for both LRX One Core and LRX One Billing. Generalised `terms.html`,
      `privacy.html`, `refund-policy.html`, and `cancellation-policy.html`
      accordingly (product name "LRX One Core" → "LRX One" in
      titles/scope statements, explicit mentions of both products added
      throughout each page's substantive sections). This was a real
      compliance gap, not just a copy nit — the Terms/Privacy a LRX One
      Billing customer would be bound by, signing in through this same
      login, described a different product than the one they were using.
- [x] Lightly corrected `index.html` (the sign-in page itself) to match:
      `<title>`/meta description no longer claim to be LRX One Core
      exclusive, and the "Looking for features, pricing..." line now
      links to both products' pages on lrxtechgroup.com instead of only
      LRX One Core's. Deliberately did **not** redesign the hero pitch,
      dashboard mockup, or "LRX ONE | CORE" nav/footer brand mark — that
      content is Core-specific by evident design choice (the mockup shows
      Core's own dashboard), and changing it is a bigger product-framing
      call than a metadata correction. Flagged below if that's wanted.
- [x] **Resolved**: user asked for the hero and nav brand to be reworked
      to cover both products. See the new entry above for what shipped.

## Fixed 2026-07-28 (design follow-up) — Hero, nav brand, and dashboard mockup now represent both products

- [x] Nav brand and footer brand mark: "LRX ONE | CORE" → "LRX | ONE",
      matching the wordmark already used on this site's own legal pages
      (an inconsistency that existed even before the dual-product issue).
- [x] Hero eyebrow/headline/sub/desc rewritten from Core's specific pitch
      ("Enterprise Operating System" / "One Platform. Endless
      Possibilities." / "Connect · Automate · Analyze · Scale · Secure")
      to a shared-account framing ("Product Sign-In" / "One Account.
      Every LRX Product." / "LRX One Core · LRX One Billing").
- [x] Dashboard mockup replaced entirely — it showed Core's own internal
      UI (Workflows, AI Coach, a workflow status table), which was
      never an honest depiction of what a *shared* login lands on. Now
      shows a "Choose a product" picker with two tiles (LRX One Core,
      LRX One Billing) — the CSS classes it used (`.mock-body`,
      `.mock-sidebar`, `.mock-card`, `.mock-chart`, `.mock-table`, etc.)
      were fully unused afterward and removed rather than left as dead
      weight; replaced with `.mock-picker*` classes.
      `.mock-url` changed from "app.lrxone.com/dashboard" to
      "app.lrxone.com" to match (no single product's dashboard to point
      at anymore).
- [x] Pillars band: Core's five-pillar framework (Connect / Automate /
      Analyze / Scale / Secure) replaced with two clickable tiles — LRX
      One Core and LRX One Billing, each linking to that product's page
      on lrxtechgroup.com. `.pillar-item` needed `text-decoration: none`
      added since the markup changed from `<div>` to `<a>`.
- [x] Footer links: "Register Interest" mailto subject de-Core'd, single
      "Features & Pricing" link split into separate "LRX One Core" /
      "LRX One Billing" links.
- [x] Verified visually with a Playwright screenshot (not just HTML
      validity) — confirmed the fadeUp entrance animations need ~1.5-2s
      to fully resolve before a screenshot reads correctly; a naive
      500ms wait caught several elements mid-animation.

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
- [x] The line below this originally said the pages would deliberately
      stay LRX One Core-specific, reasoning this was a single-product
      site. That assumption was wrong — see the correction entry above:
      `app.lrxone.com` is the shared login for both products, so the
      pages were generalised after all.

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
