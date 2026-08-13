# MEMORY — lrxone-website

Running log of what's been done on this repo across sessions, so work can be picked up
without re-deriving context. Newest entries at the top. Update this file every time you
finish a unit of work here — don't just leave it to the next session to reconstruct.

---

## 2026-08-13 — Footer link list rewritten to match lrxtechgroup-website's wording

User asked to remove "LRX One Core" and "LRX One Billing" from the
footer link list (they duplicated the page's own "learn more" links
just above the footer), then followed up asking to match the footer on
this site to `one.html`'s footer on lrxtechgroup-website — same
wording, but with "Home" swapped for "LRX Tech Group" (this site has no
home page of its own to link to).

`.footer-links` in `index.html` went from `LRX One Core / LRX One
Billing / LRX Tech Group / Contact / Privacy Policy / Terms of Service
/ Refund Policy / Cancellation Policy` to `LRX Tech Group / Terms /
Privacy / Refund Policy / Cancellation Policy / FAQ / Contact` -
matching `one.html`'s exact wording ("Terms" not "Terms of Service",
"Privacy" not "Privacy Policy") and item order. `Terms`, `Privacy`,
`Refund Policy`, and `Cancellation Policy` keep linking to this site's
own local copies of those pages (`/terms.html` etc., already present
per the 2026-XX-XX legal-pages reconciliation work); `FAQ` has no local
equivalent here so it points to `https://lrxtechgroup.com/faq.html`;
`Contact` keeps the existing `mailto:sales@lrxtechgroup.com` link
(functionally equivalent to a contact page, and avoids a dead
cross-domain link since this site has none). The footer-brand paragraph
above the link list (still mentioning "LRX One Core and LRX One
Billing") and the two `pillar-item` links elsewhere on the page were
left as-is - only the `.footer-links` list itself was in scope. Only
`index.html` has this link-list footer; `privacy.html`, `terms.html`,
`refund-policy.html`, and `cancellation-policy.html` each have a
simpler one-line footer with no link list, so nothing else needed
changing. Verified via Playwright screenshot + a DOM check of every
link's text and href.

## 2026-07-29 (footer links trimmed) — "Sign In" and "Register Interest" removed from footer link list

User asked to remove "Sign In" and "Register Interest" from the footer.
Removed both `<li>` entries from `.footer-links` in `index.html` - the
nav bar's own "Sign In" button (top right, unrelated element) was left
untouched since only the footer was in scope. No other page on this
site repeats this footer link list (it's a lean single-page site; the
legal pages have their own simpler footer).

---

## 2026-07-29 (brand/copy pass) — Logo colour, hero copy, colour scheme, dashes, and arrow fixed

User caught six issues from a live screenshot of the reworked lrxone.com
hero:

- **Nav/footer logo**: `.nav-brand .one` was white against a gold `LRX`
  - inconsistent with the umbrella "LRX One" always being gold sitewide.
  Changed to gold. Footer's `brand-mark` had the same split (`LRX` gold,
  `| ONE` unstyled/white) - wrapped the whole mark in one gold span.
- **"Product Sign-In" eyebrow removed**: this sat next to the "Coming
  Soon" badge with its own little gold line (`.hero-eyebrow::before`).
  Removed the element entirely (text + line together, since the line was
  a pseudo-element on the eyebrow itself, not a separate thing).
- **Headline copy fix**: "Every LRX Product." was missing "One" - now
  reads "Every LRX One Product.", matching the product suite's actual
  name.
- **Colour scheme matched to lrxtechgroup.com**: applied the sitewide
  gold-"LRX One"/white-product-name split to the hero subtitle and
  hero description's "LRX One Core"/"LRX One Billing" mentions, and to
  the footer brand paragraph. Left the pillar-band labels, dashboard
  mockup's picker card names, and footer nav links as plain uniform
  text - those are UI chrome/navigation, not body copy, matching how
  lrxtechgroup-website itself treats its own nav and footer links.
- **Em dashes removed, grammar fixed as a result**: the hero description
  ("product suite — LRX One Core, ... and LRX One Billing, ... —
  accessible") became "...product suite, comprising LRX One Core (...)
  and LRX One Billing (...), accessible..." - matches the wording
  already used in the page's own meta description. Also fixed the
  `<title>` tag ("Sign In — LRX One" → "Sign In - LRX One") and the
  footer brand paragraph's dash. Left the "on lrxtechgroup.com →" arrow
  in the learn-more strip alone - user only asked about the Sign In
  arrow, and that's a different, unrelated element.
- **Arrow removed from Sign In**: `Sign In →` in the hero actions is now
  just `Sign In`, matching the nav's own arrow-free Sign In button.

**Verified**: local `http.server` + Playwright screenshots, desktop and
mobile, full page - zero horizontal overflow, all six fixes visually
confirmed.

---

## 2026-07-28 (mobile audit) — Fixed nav crowding on the reworked hero

Part of the same cross-repo mobile audit as `lrxtechgroup-website`'s
footer-overflow fix (see that repo's own MEMORY.md for the sibling bug
found there). Ran the same Playwright overflow sweep — 375px and 768px,
`scrollWidth` vs `clientWidth` — across all 5 pages here.

`index.html` didn't technically overflow (no horizontal scrollbar
triggered), but a closer look at the nav specifically (bounding-box
measurements of `.nav-brand`, `.nav-link`, `.nav-start`) showed it was
right at the edge of breaking: at 375px the "LRX | ONE" logo's right
edge and "About"'s left edge were both at 134px — zero gap, touching
directly — and the "Sign In" button's right edge sat at 376px against a
375px viewport, a 1px clip. The math: `.nav-brand` (134px) +
`.nav-right`'s total width (242px) sums to exactly 376px, leaving no
room for the flex `justify-content: space-between` gap to do anything.

This nav (`About` / `Register` / `Sign In`) never had responsive
handling — `lrxtechgroup-website`'s equivalent nav pattern already hides
secondary links under 768px via `.nav-links { display: none; }`, relying
on the hero's own CTAs instead, but that treatment was never applied
here. Added the matching rule: `@media (max-width: 768px) { .nav-link {
display: none; } }`. Only the gold Sign In button remains in the nav at
phone widths now; About and Register are still reachable via the hero
buttons and footer.

Also re-checked the pillars band (the two-product-tile rework from
earlier today) at 375px specifically, since it was a brand-new component
— renders cleanly, no issues. The four legal pages were already clean
(their nav is a single "back" link, never had room to crowd).

---

## 2026-07-28 (second correction) — LRX One is the umbrella product, not just a login

Second correction in the same thread of work. The first correction fixed
a real product-scope bug (this site's legal pages exclusively describing
LRX One Core when the login serves Billing too) by introducing "LRX One"
as a name for "the shared account system." That got the scope right but
the framing wrong: the user clarified LRX One isn't just plumbing behind
the two products — it's LRX Tech Group's actual umbrella product/brand,
a suite, with LRX One Core and LRX One Billing as the two products
within it. Signing in once is a consequence of that structure, not the
definition of it.

Every place that said "LRX One is the shared account system..." or
"...your single sign-in for..." rewritten to "LRX One is LRX Tech
Group's product suite, comprising...":
- `index.html`: meta description, hero-desc, footer tagline, and the
  `.mock-picker` CSS comment (not user-facing, but was written with the
  same wrong framing and worth keeping accurate).
- `terms.html`: meta description, the doc-intro paragraph, and Section 1
  ("The service").
- `privacy.html`: the doc-intro paragraph.
- `refund-policy.html` and `cancellation-policy.html` needed no changes
  — neither ever asserted what LRX One *is*, only used the name.

**Flagged, not acted on**: `lrxtechgroup-website`'s own `index.html` —
the actual corporate homepage — shows LRX One Core and LRX One Billing
as two independent, equal product cards with zero "LRX One" umbrella
mentions anywhere (checked via grep before flagging, not assumed). If
the umbrella-product framing is real, that site's product section might
warrant restructuring, not just new copy — a materially bigger,
more visible change to the primary marketing site than fixing wording on
the sign-in page. Asked the user how far to take it rather than
redesigning that page's information architecture without confirming
intent first.

---

## 2026-07-28 (design follow-up) — Reworked hero and nav brand for both products

Follow-up to the correction below, which fixed the legal pages and left
the hero pitch, dashboard mockup, and nav/footer brand mark alone as an
open question ("visible design/positioning choice, not a compliance
fix"). User asked for it directly: rework the hero and nav brand to
cover both products.

**Nav/footer brand mark**: "LRX ONE | CORE" → "LRX | ONE". Incidentally
fixes a standalone inconsistency this surfaced — the site's own legal
pages (`terms.html` etc.) already used "LRX | ONE" in their nav, so
`index.html` had been visually inconsistent with its own sibling pages
even before the dual-product question came up.

**Hero copy**, rewritten end to end from Core's specific pitch to a
shared-account framing:
- Eyebrow: "Enterprise Operating System" → "Product Sign-In"
- Headline: "One Platform. Endless Possibilities." (Core's exact
  tagline, reused verbatim from `lrxtechgroup-website`'s one.html) →
  "One Account. Every LRX Product." — deliberately a different line, not
  a genericised version of Core's, so it can't be mistaken for
  representing Core specifically.
- Sub: "Connect · Automate · Analyze · Scale · Secure" (Core's 5
  pillars) → "LRX One Core · LRX One Billing"
- Desc: rewritten to name both products and what each is, instead of
  describing only Core.

**Dashboard mockup**: this was the biggest actual dishonesty in the old
page — it depicted Core's own internal dashboard (a Workflows sidebar
item, an "AI Coach" nav entry, a workflow status table with "Invoice
Processing" / "Lead Scoring" rows) as if that's what any shared-login
user lands on. Replaced with a "Choose a product" picker showing two
tiles — LRX One Core and LRX One Billing, each with an icon and a
one-line feature summary — which is what a real shared login for two
products should show after authentication. `mock-url` changed from
"app.lrxone.com/dashboard" to "app.lrxone.com" to match (there's no
longer a single product's dashboard being implied). The old markup's
CSS classes (`.mock-body`, `.mock-sidebar`, `.mock-nav-item`,
`.mock-content`, `.mock-row`, `.mock-card`, `.mock-chart`, `.mock-bar`,
`.mock-table`, `.mock-tr`, `.mock-status`) became fully unused once the
markup changed — grepped to confirm before deleting rather than leaving
dead CSS, replaced with a smaller `.mock-picker*` set.

**Pillars band**: Core's five-pillar framework (Connect / Automate /
Analyze / Scale / Secure — the same specific value-prop pillars used on
Core's own marketing page) replaced with two clickable tiles, one per
product, linking out to each one's page on lrxtechgroup.com. Changed the
markup from `<div class="pillar-item">` to `<a class="pillar-item">` to
make them real links, which meant adding `text-decoration: none` to the
`.pillar-item` rule (wasn't needed on a div, is needed on an anchor).

**Footer**: brand mark and tagline updated to match the nav; the single
"Features & Pricing" link (pointed only at Core's page) split into
separate "LRX One Core" and "LRX One Billing" links; "Register Interest"
mailto subject line de-Core'd.

**Verification**: ran this through an actual browser screenshot
(Playwright + the pre-installed Chromium at `/opt/pw-browsers/chromium`,
via Node since no Python `playwright` package was installed) rather than
relying on HTML-validity checks alone, since this was a real visual
change. First attempt used a 500ms wait and caught the page mid
entrance-animation — several elements looked missing that were actually
just still at `opacity: 0` partway through their `fadeUp` animation
delay/duration. Recalculated the longest delay+duration in the page
(hero-note: 0.9s delay + 0.7s duration = 1.6s) and re-ran with a 2.5s
wait, which rendered correctly. Confirmed the full page — hero, product
picker mockup, pillars band, learn-more strip, and footer — all read
coherently as a dual-product page with no leftover Core-exclusive
framing.

---

## 2026-07-28 (correction) — app.lrxone.com is the shared login for both products

Direct correction to the entry immediately below, from the same
session's earlier legal-pages reconciliation pass. That pass explicitly
reasoned: "this site's pages stay LRX One Core-specific... that's a
legitimate difference given this domain is the single-product sign-in
site, not the corporate umbrella." The user corrected that: `lrxone.com`
/ `app.lrxone.com` is actually the shared login for **both** LRX One
Core and LRX One Billing, not a Core-exclusive surface. That flips the
premise the earlier "deliberately not changed" call was built on.

This isn't just a copy-scope nit — it's a real compliance gap. A LRX One
Billing customer signing in through `app.lrxone.com` would have been
bound by Terms of Service, a Privacy Policy, and a Refund/Cancellation
Policy that described themselves as governing "LRX One Core" only, a
different product than the one they're actually using.

**Fixed across all four legal pages** — titles, meta descriptions, and
every substantive section (the service/who-we-are, acceptable use,
subscriptions, service availability, data ownership, IP, liability,
termination, governing law, information collected, how it's used, who
it's shared with, rights requests) now name both LRX One Core and LRX
One Billing explicitly, or use the umbrella name "LRX One" for the
shared account system itself, instead of exclusively naming Core. The
privacy.html processor table's Anthropic row is the one deliberately
Core-specific line left in place — the AI Assistant genuinely is a Core
feature only, so scoping that one row correctly means naming Core
specifically, not generalising it.

**Also lightly corrected `index.html`** (the actual sign-in page):
`<title>` and meta description no longer claim Core-exclusivity, and the
"Looking for features, pricing, and the full product tour?" line now
links to both products' pages on lrxtechgroup.com instead of only Core's.

**Deliberately left alone**: the hero pitch ("One Platform. Endless
Possibilities," the Connect/Automate/Analyze/Scale/Secure pillars), the
dashboard mockup (which visibly shows Core's own UI — Workflows,
AI Coach, Documents), and the "LRX ONE | CORE" nav/footer brand mark.
Those are load-bearing design/product-positioning choices, not metadata
— reworking them to represent both products (or neutralising them) is a
bigger call than this fix, and is flagged as an open question in TODO.md
rather than decided unilaterally.

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
