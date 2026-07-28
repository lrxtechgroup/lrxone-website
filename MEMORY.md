# MEMORY — lrxone-website

Running log of what's been done on this repo across sessions, so work can be picked up
without re-deriving context. Newest entries at the top. Update this file every time you
finish a unit of work here — don't just leave it to the next session to reconstruct.

---

## 2026-07-28 (final one today) — Reconciled legal pages with lrxtechgroup-website

User asked to make sure the policies on lrxone.com and lrxtechgroup.com
are consistent — reasonable, since `lrxtechgroup-website` had just been
through three passes of legal-page work (Terms/Refund/Cancellation, then
Privacy, then a billing@ email-routing change) that this site never
picked up, having built its own versions of these pages earlier and not
been touched since.

Diffed both sites' `terms.html`/`refund-policy.html`/
`cancellation-policy.html`/`privacy.html` against each other and found
two real, substantive drifts (not just branding, which is expected to
differ — see below):

1. **Stale free-tier reference.** `cancellation-policy.html` Section 3
   still said "Switching to a lower-tier paid plan, or to the free
   Starter tier, is a plan change..." — the STARTER-pricing pass
   (2026-07-28, R0 → R199/mo) only updated `terms.html`'s payment clause
   and missed this file. Fixed, and added an explicit "we don't offer a
   free tier" line matching the one just added to
   `lrxtechgroup-website`'s cancellation-policy.html, so a reader can't
   assume otherwise from context.
2. **Contact routing.** All three pages sent every contact point —
   general questions, refund requests, cancellation requests — to
   `sales@lrxtechgroup.com`. `lrxtechgroup-website`'s pages now split
   billing/finance-related contact to `billing@lrxtechgroup.com` (refund
   requests, cancellation requests, each policy's own "Contact us"
   section) while leaving general/legal questions on `sales@`. Made the
   same split here: `terms.html` Section 11 now explicitly names both
   addresses and what each is for; `refund-policy.html` and
   `cancellation-policy.html` moved their operative contact points to
   `billing@`.

**Deliberately not changed**: this site's pages stay LRX One
Core-specific — product name, `app.lrxone.com` references, the "LRX |
ONE" nav branding — rather than being generalised to also cover LRX One
Billing the way `lrxtechgroup-website`'s versions now are. That's the
correct scope for this domain (a single-product sign-in site, not the
corporate umbrella that hosts both products); consistency was about the
underlying rules (no free tier, the refund window, when cancellation
takes effect, who gets contacted about what), not about making the copy
byte-for-byte identical across two sites with different jobs.

- Bumped `terms.html`'s "Last updated" date to 28 July 2026 to match the
  other three pages, since its content changed.
- Verified all HTML files on the site still parse cleanly.

---

## 2026-07-28 (last one today) — STARTER tier no longer free (R199/mo)

User decision, confirmed as an exact number (R199/mo) rather than left
vague — see `lrxone`'s own MEMORY.md for the full billing-config change.
`terms.html`'s payment clause updated: "including a free Starter tier" →
"including a low-cost Starter tier", so this page doesn't contradict the
real pricing. Verified the file still parses cleanly.

---

## 2026-07-28 (last one today) — Built Refund Policy + Cancellation Policy for PayFast merchant verification

User relayed a real request from PayFast (confirmed mid-conversation:
"this is from payfast"), phrased as a prerequisite for completing
merchant account verification: dedicated Terms and Conditions, Refund
Policy, and Cancellation Policy pages. Checked what already existed
first — `terms.html` already covers subscriptions/payment with a brief
non-refundable + cancel-anytime clause, but there was no standalone
Refund Policy or Cancellation Policy page, which is very likely
specifically what a payment processor's verification checklist looks
for (a clause buried in a general ToS usually doesn't satisfy this kind
of check).

Deliberately did NOT invent the actual refund/cancellation terms — this
is a real legal/financial document going to a payment processor's
underwriting team, not ordinary marketing copy, so asked the user
directly (via structured questions) rather than guessing: which product
(LRX One Core), what the refund stance should be, and whether to reuse
the existing Terms page. Confirmed: **no refunds, except when a customer
has cancelled and is still billed afterward** (a billing-error case) —
that's the one policy decision that actually mattered here, and it came
from the user, not from me.

- New `refund-policy.html`: states the no-refunds-by-default policy,
  the billed-after-cancellation exception (full refund, proactively),
  how to request one (contact sales@, 5 business day response — a
  standard operational timeframe, not a negotiated term), and how
  refunds are processed (original payment method, via PayFast, up to
  10 business days to reflect).
- New `cancellation-policy.html`: how to cancel (workspace settings or
  email, no reason required, no fee), when it takes effect (end of
  current billing period — matches the existing terms.html clause
  exactly, not contradicting it), the distinction between downgrading
  and cancelling, and what happens to Customer Data afterward
  (cross-referencing the existing Privacy Policy's retention periods
  rather than restating them).
- Both pages match `terms.html`/`privacy.html`'s existing legal-page
  template exactly (same nav, same CSS, same footer) — not a new visual
  treatment.
- `terms.html`'s payment section now cross-references both new pages
  ("incorporated into these Terms by reference") rather than leaving
  its own brief clause to stand alone and potentially read as the only
  word on the subject.
- `index.html`'s footer links to both new pages alongside the existing
  Privacy Policy / Terms of Service.
- Verified all four touched/created files parse cleanly.

Not verified: whether these two pages plus the existing terms.html
actually satisfy PayFast's specific verification checklist — built to a
reasonable, standard interpretation of what a payment processor checks
for, since PayFast's own written requirements weren't available in this
session. Flagged in TODO.md.

---

## 2026-07-28 (yet later same day) — Renamed to "LRX One Core"

Same rename applied on `lrxtechgroup-website` (see that repo's own
MEMORY.md for the naming rationale): "LRX One" → "LRX One Core", so
"LRX One" is a pure house mark and every product — including the
flagship — carries its own suffix.

- Nav-brand wordmark: `LRX | ONE` → `LRX ONE | CORE` (reused the
  existing `.lrx`/`.pipe`/`.one` CSS classes, just changed which text
  goes in which span — gold "LRX ONE", plain "CORE").
- Footer brand-mark: same swap.
- `<title>`, meta description, hero desc paragraph, the learn-more
  strip's "See LRX One on lrxtechgroup.com" link text, and the footer's
  mailto subject line.
- `privacy.html` and `terms.html`: every occurrence of "LRX One" as the
  defined product name in the legal text (title, meta, and body —
  roughly 10 and 15 occurrences respectively) updated via a scripted
  find/replace that protected "LRX One Billing" from being touched
  (temporarily placeholder-swapped it out, replaced bare "LRX One",
  swapped it back), then hand-verified no stray "LRX One" without
  "Core" or "Billing" remained.
- Also renamed the small set of literal "LRX One" UI strings in the app
  frontend itself (`lrxone/frontend`) for the same reason — see that
  repo's own commit/MEMORY.md.
- Verified `index.html`, `privacy.html`, and `terms.html` all still
  parse cleanly.

---

## 2026-07-28 (later same day) — Nav/hero "Register Interest" → real "Register" link

User request: at the top of the page, replace "Register Interest"
(the `mailto:` low-commitment CTA) with a real "Register" that goes
straight to the app's sign-up page, sitting alongside "Sign In" the
same way the two already work in the app frontend itself.

Checked `lrxone/frontend/src/App.tsx` first rather than guessing the
URL — confirmed `/register` is a real, already-wired route
(`RegisterPage.tsx`), the same way `/login` already was when Sign In
was made real during yesterday's restructure. Same reasoning applies to
both now: the link is structurally correct and will just start working
once the app is actually deployed to `app.lrxone.com`, no code change
needed later.

- Nav: `Register Interest` (`mailto:`) → `Register` (`https://app.lrxone.com/register`).
- Hero actions: same swap, kept "Sign In" as the primary (`btn-gold`)
  button and "Register" as secondary (`btn-outline`) — unchanged
  visual hierarchy, since sign-in is still this page's stated primary
  purpose.
- Hero note copy adjusted ("New here? Register above, or see the full
  pitch first.") since "register your interest" no longer describes
  what happens — it's a real form now, not an email.
- Deliberately left the footer's "Register Interest" `mailto:` link
  untouched — the user scoped this to "the top" of the page. It's now
  inconsistent with the real Register link above it; noted in TODO.md
  rather than changed unprompted.
- Verified `index.html` still parses cleanly.

---

## 2026-07-28 — Fixed outbound links to point at the real LRX One product page

Follow-up to the restructure below: this site's "full pitch" links
(hero note, footer "Features & Pricing") pointed at
`lrxtechgroup.com/#products` — the summary card, not the actual full
content. `lrxtechgroup-website` now has a real dedicated `one.html`
page (built there today, mirroring `billing.html`'s pattern, with the
features grid/pricing/dashboard mockup that used to live on this page
before the restructure). Both links here updated to
`lrxtechgroup.com/one.html`. See that repo's own MEMORY.md for the
full page build.

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
