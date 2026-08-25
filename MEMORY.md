# MEMORY — lrxone-website

Running log of what's been done on this repo across sessions, so work can be picked up
without re-deriving context. Newest entries at the top. Update this file every time you
finish a unit of work here — don't just leave it to the next session to reconstruct.

---

## 2026-08-13 (WhatsApp icon added to the social row, all 6 pages) — "lets remove this tile and put the whatsapp with the rest of the social media"

Same WhatsApp relocation just done on `lrxtechgroup-website` (see that
repo's MEMORY.md for the full back-and-forth: started as "use the real
WhatsApp logo," redirected mid-turn to "remove the tile, put it in the
social row instead"). This site never had a standalone WhatsApp contact
tile to remove — it only ever had the Facebook/Instagram `.social-row`
built in the entry below — so the only change here is adding the
official WhatsApp glyph (brand green `#25D366`, inline SVG, no external
request) as the first icon in `.social-row`, before Facebook, across all
6 pages (`index.html`, `terms.html`, `privacy.html`,
`refund-policy.html`, `cancellation-policy.html`, `faq.html`). Confirmed
via `grep -c "wa.me/27620498603"` = 1 per file.

## 2026-08-13 (Facebook/Instagram row above the footer, all 6 pages) — "do the same for lrxone website"

Same feature just built on `lrxtechgroup-website` (see that repo's
MEMORY.md for the full "wrong place" back-and-forth that led to this
design): a centered `.social-row` strip — Facebook (official blue "f")
and Instagram (official gradient) icons, 22px, inline SVG, own
`border-top` divider — sitting between each page's main content and
`<footer>`.

Applied to all 6 pages. `index.html` has the same footer-logo/copy/
links structure as the sibling site; the other 5
(terms/privacy/refund-policy/cancellation-policy/faq) use this site's
simpler single-line centered footer (`footer { text-align: center }`,
no `footer-links` list) — the CSS anchor was `footer a:hover` on those
instead of `.footer-links a:hover`, but the `.social-row` block itself
is identical everywhere. Same Facebook/Instagram URLs and gradient id
(`igGradientSocialRow`) as the sibling site, since these are the same
LRX Tech Group accounts. Verified via Playwright on `terms.html` (one
of the simple-footer pages) — renders correctly above the footer.

## 2026-08-13 — Footer link list rewritten to match lrxtechgroup-website's wording

User asked to remove "LRX One Hive" and "LRX One Billing" from the
footer link list (they duplicated the page's own "learn more" links
just above the footer), then followed up asking to match the footer on
this site to `one.html`'s footer on lrxtechgroup-website — same
wording, but with "Home" swapped for "LRX Tech Group" (this site has no
home page of its own to link to).

Started this edit before pulling latest `main` and only discovered on
push that another session had, in the meantime, already rebuilt this
same footer from the old two-row `footer-top`/`footer-bottom` layout to
the current single-row `footer-logo`/`footer-copy`/`footer-links`
structure (see the 2026-08-05 "index.html footer matches
lrxtechgroup-website" entry below) and renamed the product to "LRX One
Hive" — so my first draft of this change was built against a stale
base. Resolved via `git merge origin/main`: kept the current single-row
structure (my old two-row markup no longer has matching CSS - those
rules were removed in the 08-05 rebuild) and re-applied the link-list
simplification on top of it.

`.footer-links` in `index.html` went from `LRX One Hive / LRX One
Billing / LRX Tech Group / Contact / Privacy Policy / Terms of Service
/ Refund Policy / Cancellation Policy / FAQ` to `LRX Tech Group / Terms
/ Privacy / Refund Policy / Cancellation Policy / FAQ / Contact` -
matching `one.html`'s exact wording ("Terms" not "Terms of Service",
"Privacy" not "Privacy Policy") and item order. `Terms`, `Privacy`,
`Refund Policy`, `Cancellation Policy`, and `FAQ` all keep linking to
this site's own local copies of those pages; `Contact` keeps the
existing `https://lrxtechgroup.com/contact.html` link (this site has no
local contact page, and that's the convention already established here
- see the 2026-08-05 "FAQ contact links" entry, which deliberately
moved away from a `mailto:` link toward this same URL). The
footer-brand paragraph above the link list (still mentioning "LRX One
Core and LRX One Billing" - itself now stale wording predating the
Hive rename, out of scope here) and the two `pillar-item` links
elsewhere on the page were left as-is - only `.footer-links` was in
scope. Verified via Playwright screenshot + a DOM check of every link's
text and href, then re-verified after the merge that the resolved
markup matched.

## 2026-08-05 (hero height hard-capped on touch devices) — "still like this on request desktop view"

User re-tested after the `--vh` fix and sent fresh real-device
screenshots showing the gap unchanged, still specifically under
Request Desktop Site. Investigation:

- Confirmed via `git log`/`git show origin/main:index.html` that the
  `--vh` fix genuinely was on `main` for both repos — not a failed
  push.
- Tried to fetch the live site directly (curl and WebFetch) to check
  whether it had actually deployed; both got HTTP 403 from the site's
  own bot/WAF protection, so deployment status couldn't be confirmed
  from this environment either way. Flagged to the user that a stale
  cached copy is plausible: toggling Request Desktop Site changes the
  User-Agent header, but most HTTP caches key on URL only (no `Vary:
  User-Agent`), so a browser or CDN cache populated before the fix
  can keep being served afterward.
- Independent of the caching question, hardened the fix itself so it
  no longer depends on getting viewport-height measurement right at
  all in this mode. Added a `@media (pointer: coarse)` rule — matches
  real touchscreens specifically (phones/tablets), which stays true
  even under Request Desktop Site (that mode fakes the reported
  viewport width and UA string, but not the actual input hardware) —
  that caps `.hero`'s `min-height` to
  `min(calc(var(--vh, 1vh) * 100), 820px)`. Real desktop (mouse/
  trackpad, `pointer: fine`) is completely unaffected by this rule and
  keeps the full-bleed 100dvh/--vh behavior confirmed working earlier
  today; any touch device is now hard-bounded to 820px regardless of
  what value vh/dvh/innerHeight report in whatever rendering mode.

Verified via Playwright with real device/pointer emulation (not just
viewport size, which was the gap in earlier verification passes):
- `pointer:fine` desktop context, 1440×1080 → hero height 1080
  (uncapped, matches viewport, full-bleed intact).
- `pointer:coarse` touch context at 980×700 (a plausible real
  Request-Desktop-Site geometry) → hero height ~700-747, well under
  the 820px cap, no gap.
- `pointer:coarse` touch context at 980×2200 (deliberately extreme,
  simulating whatever inflated value the real bug might be producing)
  → hero height capped to exactly 820px; screenshot confirms content
  fills the section and flows straight into the pillars band and
  footer with no black void, regardless of how wrong the underlying
  vh measurement is.

Same fix applied to lrxtechgroup-website — see that repo's MEMORY.md.
Still unresolved: whether the live site the user is testing has
actually picked up any of today's commits yet, since it couldn't be
verified from this environment. Asked the user to hard-refresh / test
in a private tab to rule out a stale cache.

## 2026-08-05 (hero viewport-height fix upgraded to JS-measured `--vh`) — "it is on the desktop view on mobile"

User confirmed the empty-gap bug reported earlier today only shows up
under Chrome's "Request Desktop Site" on a phone, not in normal
mobile view — matching cause #1 from the earlier investigation. The
`100dvh` fix added right before this wasn't guaranteed to cover that
case: `dvh`'s live-tracking behavior is a mobile-viewport-adaptive
feature, and there's no guarantee a browser applies the same dynamic
recalculation once it's rendering in a desktop-site emulation
context — CSS viewport units alone can't be trusted to correctly
reflect the visible screen in that mode.

Replaced the CSS-only approach with the standard, more bulletproof
technique: measure the real `window.innerHeight` with a small inline
script in `<head>` (runs before first paint) and store it as a
`--vh` custom property (`1% of innerHeight`, re-measured on resize/
orientation change), then set `.hero`'s `min-height` to
`calc(var(--vh, 1vh) * 100)` as the final (highest-priority)
declaration — `100vh` and `100dvh` stay as earlier declarations so
anything without JS or before the script fires still gets a sane
fallback. Because this measures the browser's actual current
`innerHeight` via JS rather than relying on how any given browser
mode computes CSS viewport units, it holds regardless of normal
mobile rendering, desktop-site emulation, or any future quirk in
between. Verified via Playwright across mobile (390×844) and two
desktop-site-style viewports (980×844, 980×700) — `--vh` matched
`window.innerHeight` exactly in all three, and the 700px-tall
desktop-site case (closest match to what a real phone would report)
showed no gap, hero content filling the section correctly.

Same fix applied to lrxtechgroup-website's homepage hero — see that
repo's MEMORY.md.

## 2026-08-05 (hero 100vh mobile-empty-space bug fixed) — "is there that much empty black space on normal website view or only desktop view on mobile"

Diagnosed root cause of a real-device screenshot showing a huge black
gap between the nav and the "One Account. Every LRX One Product."
hero content on `index.html`. Two things were compounding:

1. The screenshot's nav showed "ABOUT / REGISTER / SIGN IN" all
   inline plus the two-column product mockup — both are desktop-only
   (`.nav-link { display:none }` below 768px, `.hero-right { display:
   none }` below 900px), so the browser was rendering at a desktop-
   width viewport on a phone (almost certainly Chrome's "Request
   Desktop Site", which reports ~980px width).
2. Independently of that, `.hero { min-height: 100vh; ... align-items:
   center }` — confirmed via Playwright at a real 390×844 mobile
   viewport that this alone is fine (content fits with normal
   spacing). But mobile browsers compute `100vh` against the
   *largest* possible viewport (chrome collapsed), which is taller
   than what's actually visible when the address bar is showing —
   reproduced the exact reported gap shape by rendering at an
   artificially tall viewport height, confirming `.hero`'s content
   gets vertically centered inside a section taller than the visible
   screen, pushing it down off-screen with empty black space above.

Fixed by adding `min-height: 100dvh;` after the existing `min-height:
100vh;` (dvh = dynamic viewport height, tracks the *currently*-visible
viewport rather than the collapsed-chrome maximum; kept as a second
declaration so older browsers without `dvh` support silently fall
back to the existing `100vh` value — no regression). Same
`min-height: 100vh` pattern existed only on this one hero section
(grepped the whole site, one match). Verified via Playwright at
390×844 (real mobile) — renders identically, no layout change, before
and after.

Same bug (and same fix) also existed on the sibling
`lrxtechgroup-website`'s homepage hero — see that repo's MEMORY.md.

## 2026-08-05 (index.html footer rebuilt to match lrxtechgroup-website) — "let's make lrx one the same as lrx tech group"

User sent side-by-side mobile screenshots of `lrxtechgroup.com/faq`'s
footer (simple single row: wordmark, copyright, link list, all inline
and wrapping together) versus `lrxone.com`'s homepage footer (a
two-part `footer-top`/`footer-bottom` layout with a brand paragraph
above a separate links block above a separate copyright row) — visibly
inconsistent between the two sites.

Rebuilt `index.html`'s footer to use the exact same structure, classes,
and CSS as `lrxtechgroup-website`'s footer (`footer-logo` / `footer-copy`
/ `footer-links`, single flex row that wraps as needed): dropped the old
`footer-top`/`footer-brand`/`footer-bottom` two-row layout and the
descriptive brand paragraph (lrxtechgroup-website's footer doesn't carry
one either). Kept all 9 existing footer-links entries and the
copyright's "· lrxtechgroup.com" link (already an established pattern
on this site's other pages). `footer-logo` uses the same gold/white
two-tone "LRX One" as the nav brand. Verified via Playwright at 1280px
and 360px — link row wraps cleanly at both widths, matches the sibling
site's rhythm.

Note: this only touched `index.html` — lrxone-website's legal
subpages (`terms.html`, `privacy.html`, `refund-policy.html`,
`cancellation-policy.html`, `faq.html`) already use a simple centered
single-line footer of their own (no `footer-links` list at all), which
was untouched since it wasn't part of what the screenshots flagged.

## 2026-08-05 (FAQ "get in touch" links point to Contact page) — "the get in touch should take person to contact page"

All three "get in touch"/"Contact Us" links in `faq.html` (intro
paragraph, no-results message, bottom CTA) were pointing to
`mailto:sales@lrxtechgroup.com`. Changed all three to
`https://lrxtechgroup.com/contact.html`, matching the pattern this
site already uses elsewhere — `index.html`'s footer links "Contact" to
the same URL, since this site has no contact page of its own and
lrxtechgroup.com's is the shared one for the whole product suite.

## 2026-08-05 (mobile nav-back wrap fixed; FAQ intro trimmed) — "check the search on mobile too", real-device screenshot from lrxone.com/faq

User checked the search feature on an actual phone (not just a
simulated viewport) and sent two screenshots. Found two real bugs the
1280px/390px checks so far hadn't caught:

**Nav-back wrapping onto the logo.** On narrower real phones (~360px
CSS width — common on budget/mid Android, versus the 390-412px range
tested earlier) `.nav-back`'s full text, "← Back to lrxone.com", didn't
fit next to the mobile-sized `.nav-brand` and wrapped to a second line
that visually overlapped the "LRX One" wordmark. Reproduced by testing
at a real 360px viewport (the earlier 390px checks happened to just
barely fit, which is why this was missed). Fixed by splitting the link
text into `.nav-back-full`/`.nav-back-short` spans and swapping which
is visible inside the existing `@media (max-width: 600px)` block —
mobile now shows a short "← Back" that always fits on one line,
desktop is unaffected (still shows the full text). Applied identically
across all 5 pages that carry this nav pattern: `faq.html`, `terms.html`,
`privacy.html`, `refund-policy.html`, `cancellation-policy.html`.

**FAQ intro too wordy.** User also flagged "too much writing under
faq" — `faq.html`'s intro paragraph was 3 sentences / ~40 words,
pushing the search box and first question further down the page than
necessary, especially on mobile where every line costs more scroll.
Trimmed to two short sentences ("Answers to the questions we hear most
often about LRX One. Can't find what you're looking for? Get in touch
with our team."), dropping the redundant middle clause pointing to
lrxtechgroup.com (the search box now handles discovery) while keeping
a contact link.

Verified via Playwright at a real 360px viewport (all 5 pages, closed
state) and at 1280px desktop (faq.html, to confirm no regression on
the full-text nav-back).

## 2026-08-05 (FAQ keyword search added) — "let's have a key word search function in faq", same fix as lrxtechgroup-website

Same feature added to `faq.html` here, identical implementation to the
sibling repo: a live `#faqSearch` input above the accordion groups,
vanilla JS substring-matching each `.faq-item`'s text on every
keystroke, hiding non-matching items/groups, auto-opening matches, and
showing a no-results message (linking to
`mailto:sales@lrxtechgroup.com` since this site has no contact page)
when nothing matches. Verified via Playwright screenshots (match,
no-match, cleared states) before pushing.

## 2026-08-05 (new FAQ page added; hosting copy updated to South Africa + EU) — "do the same for lrxone-website"

Same two changes just completed on the sibling repo (`lrxtechgroup-
website`), applied here to match.

**FAQ page.** Added `faq.html`, reusing this site's legal-doc chrome
(`.nav-brand`/`.nav-back`, simple centered single-line footer — this
site has no `footer-links` list on its subpages, confirmed by reading
`terms.html`). Three `<details>` accordion groups (Products & account /
Pricing & billing / Data & compliance), content adapted for the
lrxone.com context: registration/sign-in links point to
`app.lrxone.com/register` and `app.lrxone.com/login`, pricing detail
links out to `lrxtechgroup.com/one.html#pricing` and
`.../billing.html#pricing` (pricing lives on the sibling site, not
here), and the closing CTA uses `mailto:sales@lrxtechgroup.com` since
this site has no `/contact.html`. All content pulled from existing site
copy (`terms.html`, `privacy.html`, `refund-policy.html`,
`cancellation-policy.html`) — nothing speculative. Verified via
Playwright screenshots (desktop closed/open state, mobile 390px) before
pushing: nav, accordion open/close, and footer all render correctly.
Linked from `index.html`'s `footer-links` list (the only page on this
site that has one) — inserted after "Cancellation Policy".

**Hosting copy.** `privacy.html` was the only file with an exclusivity
claim (`index.html` has none). Processor table's AWS row and the
infrastructure paragraph both changed from "AWS's af-south-1 (Cape
Town) region" / South-Africa-only framing to "AWS South Africa and EU
regions", matching the sibling site's wording exactly.

Known pre-existing inconsistency, deliberately NOT touched this pass
(out of the requested scope): `privacy.html`'s processor table still
lists `Stitch` and `Stripe` as payment processors, which was already
found inaccurate on the sibling site earlier this session (only PayFast
is actually implemented in lrxone's billing-service) and removed there.
Would need explicit confirmation before fixing here too.

## 2026-08-04 (nav wordmark scaled back down on mobile) — same fix as lrxtechgroup-website, "lrxone.com is the same"

User sent a phone screenshot showing this site had the identical
issue just fixed on `lrxtechgroup-website`: the desktop nav sizing
(33px text, 52px icon, fixed px, no responsive scaling) looks
oversized next to the logo on phone screens. Added the same mobile
override pattern: icon 52px→38px, `.lrx`/`.one` 33px→24px, `.nav-
brand` gap 18px→12px, `.nav-brand-text`'s internal LRX/One gap
8px→4px. `index.html` already had a `@media (max-width: 768px)` block
(for hiding `.nav-link`); the 4 legal pages use `@media (max-width:
600px)` instead, matched that breakpoint there too, same as the
sibling site. Verified via 412px-wide screenshots on `index.html` and
`terms.html`.

## 2026-08-04 (LRX/One word spacing tightened, decoupled from icon-text gap) — new .nav-brand-text wrapper

Widening `.nav-brand`'s gap to 18px in the previous entry had an
unintended side effect: "LRX" and "One" are direct flex siblings of
the icon under `.nav-brand`, so that one `gap` value applied uniformly
to *both* the icon-to-text gap *and* the LRX-One word gap — pushing
the two words apart just as much as the logo moved from the text.
User asked to tighten the LRX/One spacing specifically.

Wrapped `<span class="lrx">`/`<span class="one">` in a new
`<span class="nav-brand-text">` with its own `gap: 8px`, so it's
independent of `.nav-brand`'s 18px icon-to-text gap now. Applied
across all 5 pages.

## 2026-08-04 (nav logo/text gap widened) — 8px → 18px, matching lrxtechgroup-website

Same request as `lrxtechgroup-website`'s matching entry today, applied
identically for consistency: `.nav-brand` gap 8px → 18px across all 5
pages.

## 2026-08-04 (nav logo + wordmark sized to exactly match lrxtechgroup-website) — 38px icon → 52px, 20px text → 33px

User flagged that this site's nav logo/text was visibly smaller than
`lrxtechgroup-website`'s and asked for them to match exactly, not just
proportionally. Previously each site's icon/text had been scaled up
independently by its own ratio (this site's icon 26px→38px vs.
lrxtechgroup's 36px→52px earlier in the session), so they were
never actually the same absolute size.

Set `.nav-brand-icon` to `height: 52px` (was 38px) and both
`.nav-brand .lrx`/`.one` to `font-size: 33px` (was 20px, plus the
earlier 54px single-line attempt that got reverted) — both values now
identical to `lrxtechgroup-website`'s `.nav-logo-icon`/`.nav-logo-text
.lrx`. Removed the old `transform: translateY(9px)` optical-correction
hack (was tuned for the smaller 20px text against a 38px icon; at the
new sizes flexbox's default `align-items: center` reads as
well-centered without it — confirmed via screenshot rather than
assumed). Kept single-line (the stacked variant was explicitly
rejected earlier). Applied across all 5 pages.

## 2026-08-04 (nav wordmark text size reverted) — "leave the wording where it was"

Same revert as `lrxtechgroup-website`'s matching entry today: "LRX
One" text next to the logo goes back to its original 20px, letter-
spacing 0.05em, and the `transform: translateY(9px)` optical-
centering approach — undoing the earlier "match logo height" 54px
scale-up. The nav padding cap from the entry below is unrelated and
was kept. Applied across all 5 pages, verified via screenshot.

## 2026-08-04 (nav horizontal padding capped) — same fix as lrxtechgroup-website, logo was drifting too far from the left edge on large monitors

Same root cause and fix as `lrxtechgroup-website`'s matching entry
today: `nav { padding: 0 5%; }` grows unbounded on wide viewports.
Changed to `padding: 0 clamp(20px, 3vw, 56px);` across all 5 pages.
Verified: logo's left edge at a 1920px viewport went from 96px (old
5%) to 56px (capped), matching the sibling site exactly.

## 2026-08-04 (nav wordmark scaled to match logo height) — "LRX One" text now spans the 38px icon height, kept as a single line

Same request as `lrxtechgroup-website`'s matching entry today: nav text
should match the logo's height instead of looking small beside it.
Since "LRX One" here is a single line (not a two-line stack like
lrxtechgroup's "LRX TECH / GROUP"), used the same measured-cap-height
method from earlier in the session (render at a known font-size,
pixel-scan the actual ink height, scale to the target) rather than
guessing: at 20px, "LRX One"'s cap-height measured 14px; scaling to hit
the 38px icon height gave **54px** font-size (verified: renders at
~37.75px ink height, effectively exact). `.nav-brand .lrx`/`.one`
20px → 54px, letter-spacing eased 0.05em → 0.02em, and the old
`translateY(9px)` optical-correction hack removed (no longer needed at
matching size). Applied across all 5 pages.

**Explored and reverted**: also tried stacking "LRX" over "One" (two
lines, mirroring lrxtechgroup's TECH/GROUP layout) at a measured 22px
each (ink height ~37px) — implemented and screenshotted across all 5
pages, but the user decided against it ("let's not do the stacking")
and asked to go back to the single-line 54px version, which is what
shipped. Mentioning this so a future session doesn't re-propose the
stacked variant as if it's new.

## 2026-08-04 (footer wordmark "One" recolored to white too) — same gold/white split now applied at the bottom of the page

Follow-up to the nav fix above — user asked for the same treatment on
"the One at the bottom," i.e. the footer's `.brand-mark` ("LRX One"),
which was a single `<span class="gold">LRX One</span>` (all gold, no
split, since it predates the nav-brand pattern and was never wired to
it). Split into `<span class="gold">LRX</span>` +
`<span style="color:var(--white)">One</span>`, matching the nav.
Confirmed via grep this is the only `.brand-mark` on the site (the
legal pages use a simpler footer with no brand-mark). Verified via
screenshot.

## 2026-08-04 (nav wordmark "One" recolored to white on index.html) — fixes an inconsistency, breaks up the all-gold nav block

User flagged the logo + "LRX One" nav looking "too yellow." Explored
recoloring the logo itself first (mocked up 4 muted/bronze/amber
alternatives), but the user clarified they want to *keep* the logo's
gold and instead break up the monochrome gold block in the top-left
corner by varying the wordmark's own coloring.

Turned out this was already inconsistent across the site: `index.html`
had `.nav-brand .one { color: var(--gold); }` (both "LRX" and "One"
solid gold, matching the icon — hence the "all yellow" read), while
`cancellation-policy.html`, `privacy.html`, `refund-policy.html`, and
`terms.html` already had `.one` set to `var(--white)`. Fixed
`index.html` to match the other four pages: "LRX" stays gold (echoing
the icon), "One" is now white — same two-tone treatment used
everywhere else on the site (e.g. "LRX One Hive" gold+white
pattern on lrxtechgroup-website). Verified via screenshot.

## 2026-08-04 (nav heading optically re-centered against the logo) — text nudged +9px down from bounding-box center

Same fix as `lrxtechgroup-website`'s matching entry today, same root
cause: `.nav-brand`'s `align-items: center` already puts the icon and
"LRX One" text at the same geometric bounding-box center, but the
logo's ink is visually weighter lower in its box than the single-line
text is, so it still read as off-center. Measured ink centroids (icon
≈39.1 vs text ≈33.4, a smaller gap than the two-line lrxtechgroup-website
case since this is single-line text) via the same screenshot-and-analyze
method, mocked up 0px/+3px/+6px variants, got sign-off on the +6px
variant, then two more explicit nudges (+2px, +1px) to land on
`transform: translateY(9px)` applied to both `.nav-brand .lrx` and
`.nav-brand .one` across all 5 pages.

## 2026-08-04 (nav logo enlarged) — icon bumped 26px → 38px, heading kept vertically centered beside it

Same request as `lrxtechgroup-website`'s matching entry today: nav logo
bigger, heading text ("LRX One") centered to its right. `.nav-brand`
already used `display: flex; align-items: center`, so bumping
`.nav-brand-icon`'s height doesn't disturb the centering — it
re-derives against the new (taller) icon automatically. Applied
`height: 26px` → `38px` across all 5 pages (identical rule on each).
Nav bar is 70px tall, so 38px leaves plenty of room, no clipping.
Verified via headless-Chromium screenshot before committing.

## 2026-08-04 (logo recolored to match brand gold, all 5 LRX repos)

Same fix as `lrxtechgroup-website` — full details and the exact colour
values/method are in that repo's MEMORY.md (shared source asset,
identical fix applied everywhere). Short version: the extracted logo's
own gradient (mean `~#C7974A`) was measurably warmer than this site's
actual `--gold`/`--gold-dark` (`#D4AF37`/`#B8922E`, confirmed identical
to the sibling sites before recoloring). Recolored the icon's gradient
in place (luminosity-driven interpolation across the brand palette,
keeping the original highlight/shadow shading), regenerated the
favicon set and `logo-mark.png`, redeployed over the previous
versions. Verified visually against this site's own nav (icon now
matches the "SIGN IN" button gold and "LRX One" text gold).

---

## 2026-08-02 (real logo added, both sites) — favicon added, icon mark added to the previously text-only nav

User uploaded the real LRX Tech Group logo artwork and asked to use
just the icon mark (not the full "LRX TECH GROUP" + tagline lockup) as
the favicon and nav logo, on this site and `lrxtechgroup-website`. Full
extraction/processing details are in `lrxtechgroup-website`'s
MEMORY.md (same source image, same generated asset set, shared across
both repos) — this entry covers what's specific to this site.

**Difference from `lrxtechgroup-website`**: this site's nav
(`.nav-brand`) never had an icon graphic at all — just the "LRX"/"One"
text spans, `align-items: baseline`. So this was an addition, not a
swap: added `<img class="nav-brand-icon" src="/images/logo-mark.png">`
before the text spans, changed `.nav-brand`'s `align-items` from
`baseline` to `center` (baseline alignment doesn't make sense once
there's an image in the flex row), and added a new `.nav-brand-icon`
rule (`height: 26px; width: auto` — smaller than
`lrxtechgroup-website`'s 36px nav icon since this nav is visually
lighter/more compact to begin with).

Applied identically across all 5 pages (`index.html`, `privacy.html`,
`terms.html`, `refund-policy.html`, `cancellation-policy.html`) —
confirmed byte-identical favicon `<link>` and `.nav-brand` markup/CSS
across all 5 before scripting the replacement.

**Verified**: same method as the sibling repo — local `http.server` +
Playwright screenshot of the nav bar, `curl` 200-check on every new
asset URL.

---

## 2026-07-31 (org-wide rename) — "LRX One Core" → "LRX One Hive" across all 5 pages

Same org-wide rename requested across all `lrxtechgroup` repos: "Core"
as part of the product name "LRX One Core" becomes "LRX One Hive".

Updated `index.html`, `terms.html`, `privacy.html`, `refund-policy.html`,
`cancellation-policy.html` — plain-text mentions plus the split gold/
white span styling (`<span style="color:var(--gold)">LRX One</span>
<span style="color:var(--white)">Core</span>`) used in the hero/product-
picker copy, which a plain "LRX One Core" string search would have
missed since the word "Core" sits in its own span.

Left `MEMORY.md`/`TODO.md` history untouched, same reasoning as always
— it's a changelog of what was true at the time, not live copy.

**Verified**: `grep -rn '\bCore\b\|\bCORE\b' *.html` returns nothing
after the change.

---

## 2026-07-30 (nav wordmark weight matched to footer) — "One" in the nav logo is now bold, matching the footer's "LRX One" mark

Follow-up to the same-day separator removal below: once the pipe was
gone, the nav wordmark's "One" was left at `font-weight: 300` with
`letter-spacing: 0.1em` — a leftover from when the thin weight helped
visually separate it from "LRX" without the pipe. The user compared it
directly against `index.html`'s own footer `brand-mark` (`LRX One` at
`font-weight: 900`, no extra letter-spacing) and asked for the nav to
match that design.

Changed `.nav-brand .one` from `font-weight: 300; letter-spacing: 0.1em`
to `font-weight: 900` (letter-spacing removed) on all 5 pages — same
scope as the separator fix, since all 5 share the identical nav markup.
Color was left untouched: `index.html`'s nav "One" is gold (matching its
gold footer mark), the 4 legal pages' nav "One" stays white (their own
pre-existing, intentional gold-`LRX`/white-`One` split) — this request
was about weight/spacing consistency with the footer design, not a
color change.

---

## 2026-07-30 (branding + footer cleanup) — Separator removed from "LRX | ONE" wordmark, arrow dropped, sales email removed, Contact re-pointed

User flagged four issues from a live screenshot of `index.html` (the
`app.lrxone.com` sign-in page):

- **"LRX | ONE" separator removed sitewide**: the product's actual name
  is "LRX One" (two words, no pipe) — the `|` divider was a leftover
  from an earlier wordmark iteration (see 2026-07-28 entries below,
  where it replaced "LRX ONE | CORE"). Removed the `<span class="pipe">`
  element and its CSS rule from all 5 pages (`index.html`, `terms.html`,
  `privacy.html`, `refund-policy.html`, `cancellation-policy.html` — all
  five shared byte-identical nav markup), changed `ONE` → `One` to match
  the product name's actual casing everywhere else on these sites, and
  added `gap: 6px` to `.nav-brand` so "LRX" and "One" keep a visible gap
  now that the pipe (which supplied the spacing via its margin) is gone.
  `index.html`'s footer `brand-mark` had the same "LRX | ONE" text —
  fixed to "LRX One" there too.
- **Arrow removed from the "learn more" strip**: `index.html`'s
  "Looking for features, pricing, and the full product tour? ... on
  lrxtechgroup.com →" line had a trailing arrow the user wanted gone.
  Removed the `→` character only, left the rest of the sentence and its
  links unchanged.
- **Sales email removed from the footer**: `index.html`'s `footer-bottom`
  had a bare `sales@lrxtechgroup.com` line sitting under the copyright
  line, with no context (no "Contact:" label, not a mailto). Removed
  that `<p>` entirely — the same address is still reachable properly
  through the footer's "Contact" link (see next item).
- **Footer "Contact" link re-pointed to a real contact page**: it was a
  bare `mailto:sales@lrxtechgroup.com`, which only ever offered one of
  the several ways to reach LRX Tech Group. Changed to
  `https://lrxtechgroup.com/contact.html` — the corporate site's actual
  Contact page (Sales/Support/Billing emails, WhatsApp, phone), matching
  what the user wanted: "all relevant contact methods as is on
  lrxtechgroup website."

**Scope note**: the pipe/separator fix applies to all 5 pages (user
confirmed sitewide rather than `index.html`-only after being asked,
since the nav wordmark markup turned out to be identical across all of
them). The arrow, sales-email, and Contact-link fixes only existed on
`index.html` to begin with — the other 4 pages have a much simpler
single-link nav/footer with no equivalent elements.

`terms.html`'s legal-content mention of `sales@lrxtechgroup.com` (in its
Terms body copy, not a footer/nav element) was deliberately left
untouched — out of scope for a footer/nav cosmetic fix.

---

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
