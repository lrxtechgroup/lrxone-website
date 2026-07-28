# MEMORY — lrxone-website

Running log of what's been done on this repo across sessions, so work can be picked up
without re-deriving context. Newest entries at the top. Update this file every time you
finish a unit of work here — don't just leave it to the next session to reconstruct.

---

## 2026-07-27 (yet later same day) — Restructured this whole site into a dedicated sign-in page

User's explicit direction: `lrxtechgroup.com` becomes where both
products (LRX One, LRX One Billing) are fully advertised — features,
pricing, the works — and this site (`lrxone.com`) becomes a dedicated
sign-in gateway with only light product advertising on the landing
page, not a full competing marketing funnel.

Cut from ~528 lines to ~373. **Removed entirely**: the full features
grid section (6 detailed feature cards), the pricing section (3-tier
cards with feature lists), and the CTA banner (redundant with the
hero once Sign In is the hero's own primary action). Verified no
orphaned CSS/HTML references were left behind after the cut.

**Kept**: the hero (headline, short pitch, dashboard mockup) and the
pillars band (Connect/Automate/Analyze/Scale/Secure) — compact, still
genuinely "some advertising of the actual product" without duplicating
the full pitch that now lives on the corporate site.

**Sign In restored as the real, primary CTA** (`https://app.lrxone.com/login`,
in both the nav and the hero) — reasoned through this carefully rather
than just flipping it back on reflex: a page whose *entire stated
purpose* is "sign in" contradicts itself if the sign-in button doesn't
actually work, in a way that "coming soon, register your interest"
elsewhere on a general marketing page doesn't. The link is real and
structurally correct (it'll just start working the moment the app is
actually deployed — no code change needed then), while the "Coming
Soon" badge stays in the hero so the page is still honest about
current state. "Register Interest" stays as the secondary CTA for
people who don't have a workspace yet.

Added a "Looking for features, pricing, and the full product tour?"
strip linking to `lrxtechgroup.com/#products`, and a matching footer
link — the site now actively points visitors at the corporate site for
the full pitch rather than trying to also be that page.

Footer trimmed from 4 columns (Platform/Company/Get Started, several
linking to the now-removed `#features`/`#pricing` anchors) to a single
flat link list, since there's no longer a multi-section page to
sub-navigate.

Verified `index.html` still parses cleanly.

---

## 2026-07-27 (even later same day) — Added Deputy Information Officer to privacy.html

User confirmed `brandon@lrxtechgroup.com` should stay as the Information
Officer contact (not swapped to `sales@` like the marketing CTAs) and
asked to add Jessica Le Roux as Deputy Information Officer instead —
`jessica@lrxtechgroup.com`, added to the same info-card in `privacy.html`
Section 1. Left the two other in-body references to "our Information
Officer" (Sections 7 and 9) pointing at Brandon specifically — the
info-card disclosure at the top already surfaces both contacts, no need
to duplicate the deputy's address in every mention.

## 2026-07-27 (later same day) — Contact email changed info → sales

User request, same change applied to `lrxtechgroup-website`. Every
`mailto:` link in `index.html` (Register Interest, Contact Sales, Talk
to Sales, footer links, the visible email text) and the one reference
in `terms.html`'s contact section now goes to `sales@lrxtechgroup.com`
instead of `info@lrxtechgroup.com`. Deliberately left `privacy.html`
untouched — its Information Officer contact is `brandon@lrxtechgroup.com`,
a different, specific address for POPIA purposes, not `info@`. Verified
both changed files still parse cleanly.

---

## 2026-07-27 (later same day) — Marked the whole site "Coming Soon"

Same treatment just applied to `lrxtechgroup-website`'s product cards
(user's explicit follow-up request), adapted to this site's single-
product layout — there's no card/badge system here, this whole site
*is* the one product's landing page, so "coming soon" had to be woven
through the nav, hero, pricing, CTA banner, and footer rather than
applied to one component.

- New `.hero-badge.coming-soon` pill ("Coming Soon") added above the
  existing hero eyebrow — same muted grey treatment
  (`background: var(--mid)`) as the sibling site's coming-soon badge.
- Nav: removed "Sign In" entirely (nothing to sign into yet) and changed
  "Get Started" to a single "Register Interest" `mailto:` CTA.
- Hero "Start Free →" → "Register Interest →", same `mailto:` pattern.
- Pricing cards' "Get Started Free"/"Start Business Trial" → "Register
  Interest" (kept Enterprise's "Contact Sales" — already `mailto:`,
  already consistent).
- CTA banner's "Start Free Today →" → "Register Interest →"; also fixed
  the banner copy itself ("Join the businesses using LRX One..." implied
  existing live customers — replaced with forward-looking copy that
  doesn't claim current availability).
- Footer's "Free Trial"/"Sign In" links replaced with a single
  "Register Interest" link.
- Left alone (correctly informational, not availability claims): the
  dashboard mockup's decorative `app.lrxone.com/dashboard` URL bar (not
  a real link), "See Platform"/`#features` and "Talk to Sales"/`mailto:`
  (already appropriately low-commitment).
- Removed the now-unused `.nav-signin` CSS rules along with the button.
- Verified `index.html` still parses cleanly.

---

## 2026-07-27 — Built real Privacy Policy and Terms of Service pages

Part of working through a rediscovered planning doc (`lrxtechgroup/lrxone`'s
`CLAUDE_TODO.md`, item 12). The earlier fix here (2026-07-25) deliberately
left the footer's Privacy Policy/Terms links pointed at `mailto:` rather
than fabricate legal content — the site owner has now supplied real
content (Information Officer, registered address, data processors,
retention periods, POPIA scope), so that block is lifted.

- New `privacy.html`: Information Officer (Brandon Le Roux), registered
  address, what's collected, the real data-processor list (AWS
  af-south-1, Anthropic, Stitch, PayFast, Stripe, Microsoft, Keycloak),
  retention (12 months post-cancellation, 7 years for financial records
  per FICA), and POPIA rights, matching the site's existing gold-on-black
  brand and typography.
- New `terms.html`: acceptable use, subscription/payment terms, service
  availability, data ownership (incorporating the Privacy Policy by
  reference), limitation of liability, governing law (Republic of South
  Africa).
- `index.html`'s footer links updated from the `mailto:` placeholders to
  the real pages.
- Verified both parse as well-formed HTML (`html.parser`), not just
  visually checked.

Also flagged and got fixed in `lrxtechgroup/lrxone`: the app frontend's
`RegisterPage.tsx` had its own "Terms of Service"/"Privacy Policy" links
pointing at `/terms`/`/privacy` *relative to `app.lrxone.com`* — routes
that don't exist in that app's router at all (confirmed - the catch-all
route there redirects to `/login`). Now points at the real pages built
here instead.

---

## 2026-07-25/26 — Fixed the two gaps from the first review, merged to main

Branch `fix/footer-links-favicon-robots` (off `main`), merged in:

- Footer `/privacy` and `/terms` links pointed at pages that don't exist
  — pointed both at a `mailto:` instead of fabricating legal content.
  Real Privacy Policy / Terms of Service pages still need to be written
  by the site owner and linked once they exist.
- Added an inline-SVG favicon, `robots.txt`, and `sitemap.xml` (linked
  from `robots.txt`).
- Footer copyright year was hardcoded — made it dynamic via a small
  inline script.

---

## 2026-07-25 — First review of this repo (no prior MEMORY/TODO existed)

This is the marketing/product landing page for LRX One, a single static
`index.html` (519 lines, no build process, no JS logic, nothing dynamic —
just HTML/CSS with inline SVG icons). Reviewed directly (not delegated to a
subagent — small enough for a direct read). No code changes made.

Nothing structurally wrong: no security issues (no scripts executing
untrusted data, no forms, no user input), clean semantic HTML, responsive
breakpoints present, `prefers-reduced-motion` respected. Two minor gaps
found, logged in `TODO.md`:
- Footer links to `/privacy` and `/terms` that don't exist anywhere in the
  repo (no routing/redirect layer either — this is a single static file).
- No favicon, robots.txt, or sitemap.xml.

This repo had never had a `MEMORY.md`/`TODO.md` before — this entry is the
first, done alongside similar first-pass reviews of the other
`lrxtechgroup` org repos.

---

## Repo orientation (for future sessions — not a changelog entry)

- Single-file static site: `index.html`. No build step, no dependencies,
  no CI/CD workflow of any kind.
- Sibling site: `lrxtechgroup/lrxtechgroup-website` (the parent company's
  own marketing page — same structure, same two gaps).
