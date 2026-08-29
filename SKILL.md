---
name: target-employer
description: Build a world-class, company-branded landing page pitching a candidate for a specific job posting. Inputs are a full live scan of the company's site, the job description, the candidate's record, and the company's logo. Output is a standalone repo deployed to GitHub Pages (and optionally a custom domain via FTP), noindexed, markdown-editable, gated by its own automated QA suite. Use when someone is applying somewhere and wants a version "for them."
---

# Target Employer

One company, one role, one page that sells the candidate the way the company sells its own product. Not a template swap: a recruiter should open it and feel it was built specifically for their brand, using their own product's mechanics as the organizing metaphor.

Two full worked examples, built with this exact skill, are linked in the README. Read them before building a new one — their `design-notes.md` files and git logs are a record of every mistake already made once, so you don't repeat it.

## First run: Claude interviews you. Nobody edits this file.

The first time this skill runs, check `memory/pitch-profile.md`. If it does not
exist, FIRST read `memory/profile.md` (the fuller Job OS writes it at /setup)
and prefill anything it already holds, name and resume especially, so the
interview shrinks. Then open with one line of expectation setting, something
like: "First time running Pitch. A few quick questions, then a draft page in
about fifteen minutes." Then
collect whatever is still missing, one question at a time, and write the
answers to `memory/pitch-profile.md` so they are never asked again. "No" is a
fine answer to every optional question; the page ships well without
testimonials, without a personal site, and without GitHub:

- Your name as it should appear on the page.
- Your resume: a file path or pasted text. This is the only source of claims.
- "Has anyone written something good about your work? A recommendation, a quote from a manager?" (optional, worth having).
- "Do you have a personal website or portfolio?" (optional).
- "Do you have photos, videos, or work samples you want on the page? Photos of
  you speaking or working, a short video, screenshots of things you built?
  Give me the files or a folder." (optional; record what exists and where.
  A page ships fine with none.)
- "Is there anything from your work we must not name in public? A secret
  project, a client name?" (optional, but always ask).
- A GitHub username if they have one (optional; see the deployment ladder).

On every later run, read `memory/pitch-profile.md`, confirm the resume is still
current in one line, and go. Wherever this file says `<CANDIDATE_NAME>`, `<RESUME_SOURCE>`,
`<TESTIMONIALS_SOURCE>`, `<PERSONAL_SITE_URL>`, `<REDACTION_LIST>`, or
`<GITHUB_USERNAME>`, it means the values from that profile. These bracketed
names never appear in anything the candidate sees.

## Two speeds. Say which one is running.

How the person chooses: saying "build my pitch page" with a posting runs the
DRAFT, always, and you announce it ("Running the draft, about fifteen
minutes"). Saying "full build", before or after seeing a draft, runs the FULL
BUILD, and a draft's scan and copy carry forward into it. Nobody asks for the
pro layer by name; it is offered only when they ask how to maintain, keep
updated, or host the page properly.

**Draft (the default, and the workshop mode): about fifteen minutes.** A fast
scan of the company's site for tokens and language, a single-file page in
their look with the candidate mapped to the posting and the honest gap named,
opened locally in the browser. Label it a draft. It is real enough to see the
idea and light enough for a classroom.

**The full build: for when a real application is on the line, 30 to 45
minutes.** The browser brand scan with signature moves, the custom mark, the
working interactive element, and the side-by-side design QA. One page, one
folder, opens locally; publish via the deployment ladder when they want a
link. Never start it without saying so.

**The pro layer: optional, and not for a first page.** The bespoke QA script
(every design rule encoded as a failing test), the standalone repo, live
parity checks, custom-domain FTP. This is maintenance infrastructure for
pages that live for months. Offer it only if they ask how to keep the page
maintained, and say plainly it roughly doubles the time.

A draft upgrades to a full build, and a full build to the pro layer;
nothing is wasted.

**Both speeds share the same floor.** The draft cuts deployment, QA depth,
and asset breadth. It never cuts the scan, the concept, or the delight
budget: a draft with generic reveals and a color match is a failed draft,
not a fast one. At draft speed the page still applies the full delight
budget with inventory citations in the CSS comments, still steals one
signature move whole and working, and still passes the kill question. If
time forces a cut, cut a section, never the fidelity of the sections that
remain.


## Inputs (ask, don't guess)

1. Company name. Derive their real marketing site URL from it and confirm it with the user in one line before scanning; never scan a guessed domain silently.
2. The full job description. A posting URL counts as the natural way in:
   fetch it, then confirm the role and company back in one line. If the fetch
   fails, ask them to paste the whole thing, not a summary.
3. `<CANDIDATE_NAME>`'s record: `<RESUME_SOURCE>`, `<TESTIMONIALS_SOURCE>`, `<PERSONAL_SITE_URL>` content, plus anything said in-conversation. **Never invent a metric, a team size, or an outcome.** If a number isn't in the source material, don't cite it.
3b. **Media is optional, especially on a first page.** A first pitch page ships
fine with only text and the candidate's mark; never block on photos or video.
If the candidate DOES keep a media library, treat it as one central registry:
a single folder deployed to a public URL with a manifest listing each asset's
verbatim label, live URL, and dimensions. Read the manifest before writing
markup, stream the public URLs instead of copying files, take labels and alt
text verbatim, and never invent a caption. A registry entry nobody uploaded
ships a broken element on a page you already sent.
4. Fit concerns or specific hooks the candidate has already flagged for this application
5. **The company's logo, as an image.** Get it yourself during the Phase 0
   scan: it is sitting in their site's nav or press page, so save it from
   there. Ask the candidate for it only if the scan cannot produce a clean
   copy. Needed for Phase 0.5 below — never skip having it, and never reuse a
   mark built for a different company's logo.

**The candidate record comes from the real resume, never from memory.** Look
before asking: the project's `memory/profile.md` and any resume file it names,
or a career/resume content source if the environment has one. If found, confirm
it is current and use it as the only source of claims. If nothing is found, stop
and ask for the resume before building anything; a pitch page written from
recollection fabricates by accident. Every number, title, and claim on the page
must be traceable to the resume or to an answer the candidate gave when asked.

## Phase 0 — Full brand scan (browser, not curl)

**This is the core of the skill and it is not optional.** If you are about to skip the browser and "just grab the colors," stop: you are no longer running this skill. Open the company's real site in a real browser, walk it, and MEASURE computed styles. Scan the pages closest to the product the role touches (for a loyalty role, the loyalty pages; for checkout, the store), not just the homepage.

**Real case for why:** a major airline's page built from a curl scan came out with red, square primary buttons. The browser scan of the same site showed a black nav, blue pill CTAs (24px radius), and the real heading face with its declared fallback. Curl reads the brand's accent; the browser reads how the brand actually behaves. If no browser tool is available, say so plainly and treat any curl fallback as a draft that must be re-verified, never as done.

Walk all of it:

- Scroll the entire homepage capturing every section: hero formula, scroll motion (sticky stacks, parallax, reveals), signature components (their checkout module, feed, search, cards — whatever their product's actual UI is built from)
- Open 1-2 secondary pages (a product or category page, a "how it works" page) and any mega-menu
- Extract measured tokens via JS in the page: computed colors, font-family, border-radius on buttons/panels, nav treatment. If browser tooling is blocked on a domain, fall back to `curl -A "Mozilla/5.0" <url>` plus grepping their live CSS bundles for hex colors, font-family, and border-radius — real measured values beat a guess every time.
- Capture their copy voice as a formula (benefit headline + one concrete sub + one verb-first CTA + numbers as proof — or whatever their actual pattern is)
- Write everything to `design-notes.md` in the new repo. This file is the contract for every later edit; "vibes" are not a reference, and neither is memory of what a *different* company's brand looked like.

### The deep scan protocol. Six stages, instruments named, run in order.

A single screenshot is one frozen frame of a moving site, and CSS keyframes
miss every JS-driven animation. Both blindnesses produced weak pages until
this protocol replaced them. Run the stages in order and write what each one
found into the inventory.

**Stage 1, pick the strongest instrument available.** The ladder: real
browser automation first, headless Chrome screenshots second, curl last.
Check for the better instrument before settling; do not default to the
weakest one out of habit. Declare which rung you ran on.

Rung one needs nothing preinstalled on the person's machine except Chrome
itself: `npm i playwright-core` in a scratch folder (seconds, ~3MB, no
browser download), then launch with `channel: 'chrome'` so it drives the
Chrome they already have. NEVER install the full `playwright` package; it
downloads entire browsers and the person did not sign up for that. If npm
or Chrome is missing, drop a rung without drama; the protocol's other
stages still run on rung two, just with the motion numbers recovered from
frame diffs instead of getAnimations().

**Stage 2, multi-page census.** Minimum four pages: the homepage, the
product page closest to the ROLE, one content page (blog, research, docs),
and one more from the main nav. Different pages reveal different systems;
the homepage alone is marketing, not the brand.

**Stage 3, motion capture.** With automation: run
`document.getAnimations({subtree:true})` and record names, durations,
easings, iteration counts, and targets; step the scroll in five increments
and count running animations at each stop (bursts reveal stagger
choreography); screenshot at 250ms, 900ms, 2s, and 4s after load to catch
the entrance sequence. Without automation: multiple headless screenshots at
increasing virtual-time budgets, diffed. Record the true entrance numbers
(a site whose reveals are 500ms linear must not be rebuilt at 1s ease-out).

**Stage 4, ground truth over inference.** Fonts: read `document.fonts` for
what actually loaded, then ZOOM a rendered screenshot on the hero
letterforms and look, because family names and fallback chains lie (a face
named like a sans can render as a slab serif). Colors: read the `:root` CSS
variables; brands name their real palette in code (volcanic, marble, coral)
and those named tokens outrank any frequency count of hex values. Type
scale: measure the rendered h1 (size, spacing, line-height), don't infer it.

**Stage 5, asset harvest.** Pull the image and video URLs from the DOM,
download two or three hero assets, and describe the illustration language in
words: materials, lighting, texture, motion. "Gradient" is not a
description; "luminous chromatic ink-wash fields with visible photographic
grain" is. If the imagery system has grain, glow, or texture, the page must
carry the same texture or it will read flat next to the original.

**Stage 6, the uniqueness filter.** The inventory still needs its twelve
entries, and now at least FIVE must pass this test: "would this sentence be
false on a random SaaS site?" Sticky navs, pill buttons, and fadeInUp fail
the test and do not count toward the five. The five that pass go in a
**Fingerprint** section at the top of delight-inventory.md, and the finished
page must visibly reproduce at least three of them. A quota filled with
generic entries is the failure mode this stage exists to kill: the minimum
is a floor for looking, not a ceiling for finding.

### Never scan only the posting page. It is the plainest page they own.

Careers and job-detail pages are templated and stripped: reduced type scale, no
motion, no media. Scanning one and building from it produces a page that is
technically on-brand and visibly dead. **Always walk the homepage, the work or
case-study index, one case-study detail, and the about or capabilities page before
writing any markup.**

**Real case:** a digital consultancy's job page measured a 48px H1, flat text
columns, and zero imagery. A page built from it shipped text-only and the candidate
called it bare on sight. The same company's homepage was over 10,000px tall with an
autoplaying full-bleed video hero, a 60-second infinite client-logo marquee,
hover-video swaps on every case card, live world clocks in the nav, and a 140px
display heading on the work index. Same brand. The posting page showed none of it.

### The scan produces artifacts, or it did not happen.

Prose impressions get skipped under time pressure; files with minimums cannot
be. A full build's Phase 0 is not complete until these exist in the repo:

1. **`design-notes.md`** — the measured tokens (already required below).
2. **`delight-inventory.md`** — MINIMUM 12 entries across all 6 categories,
   no category empty:
   - **Micro-interactions**: hover and press states. Which property moves,
     how far, which curve, how many ms. Measured, not guessed.
   - **Scroll behavior**: reveals, stickies, parallax, statement pacing
     (one giant thought per screen is a scroll behavior).
   - **Motion and ambient**: autoplaying video, loops, animated counters,
     number tickers, background drift.
   - **Imagery system**: illustration style, photo treatment, device frames,
     icon weight. What KIND of pictures make it feel like them.
   - **Component furniture**: cards, nav, inputs. Radii, shadows, borders,
     density.
   - **Copy delight**: microcopy, empty states, jokes, the way buttons speak.
   Every entry has three fields: WHERE (page + selector or screenshot),
   WHAT (the measured values), HOW TO STEAL (one implementation sentence).
3. **The evidence pack**: screenshots at four scroll depths minimum, plus
   hover states where the tooling can capture them.

**Blindness is declared, never silently absorbed.** Screenshots cannot see
motion. When the browser tooling is limited or blocked, pull the site's real
shipped CSS bundles and read them: extract every `@keyframes`, `transition`,
`animation`, and easing function, plus video and Lottie URLs from the HTML.
The animation system is fully written down in files the site already serves;
a scan that never opens them has chosen to be blind. State in design-notes
exactly what the scan could and could not observe.

### Always measure the delight level, then match it.

Brand is not only color and type. It is how much the page moves and how much it
shows. Capture in the browser and record in `design-notes.md`:

- **Easing and duration census.** Which curve and durations actually dominate.
  One dominant curve is the signature; use it everywhere instead of mixing. In the
  case above: `cubic-bezier(0.22, 1, 0.36, 1)` at 0.12s for hovers, 0.28s to 0.6s
  for reveals.
- **Hero treatment.** Static, video, or generative. Autoplay, muted, looping, and
  what aspect ratio.
- **Hover behavior on cards.** Image scale, filter shift, idle-to-hover video swap,
  cursor changes.
- **Loops and ambient motion.** Marquees, tickers, live clocks, counters. These
  read as craft and are cheap to reproduce.
- **Scroll reveals.** Translate distance, stagger, whether they replay.
- **Image and media treatment.** Aspect ratio, `object-fit`, corner radius, and
  whether radius differs between media and controls. One real system used 0px
  radius on every image while every button was a 100px pill; flattening that
  tension loses the brand.
- **Display type ceiling.** The largest heading anywhere on the site, not on the
  posting page.

Then build to that level. A restrained brand still earns motion; restraint means
one curve and few effects, not zero. If the scan finds video and the page ships
with none, the scan was decoration.

### Name the signature moves, then steal one whole.

After the census, write down the two or three **signature moves**: the specific
components or interactions that make the site unmistakably theirs. A live product
widget in the hero. An inline receipt that appears as an agent buys a tool call.
A card that flips to a case study. Whatever it is, name it in `design-notes.md`.

The page must **reproduce at least one signature move as a working interaction**,
rebuilt in their tokens with the candidate as the content. If their signature is a
transaction widget, the page contains a working transaction widget whose product
is the candidate. A static nod is not reproduction, and tokens without a signature
move produce a template wearing their colors.

### Match their structural depth, not just their surface.

During the scan, write down the target site's **section inventory**: every
H2-level section in order. Their hero, stats strip, feature grid, how-it-works
steps, comparison, pricing, install, closing CTA, whatever their actual arc is.
The pitch page must carry a comparable arc: for each of their signature section
types, build the candidate equivalent. Their how-it-works becomes how-I-would-
work. Their comparison becomes the two-playbooks section. Their stats strip
becomes the candidate's numbers. A brand-correct page with three short sections
against their ten-thousand-pixel product story reads as a brochure and fails the
kill question on depth alone. Cut a section type only when their own site does
not have it.

### The delight budget. The build spends it, and cites its sources.

The built page must implement, at minimum, from the inventory:
- The site's dominant easing curve and duration range, globally.
- Three micro-interactions (hover lifts, press states, focus moves).
- One scroll behavior (their reveal pattern, their statement pacing).
- One signature move, working, with the candidate as the content.
- The imagery treatment (their device frames, their illustration weight, or
  an honest equivalent; a page of bare text blocks fails this line).
Each implemented item carries a comment naming its inventory entry
(`<!-- delight #7: card hover lift, 4px, 200ms, their ease -->`). A page
that cannot cite its inventory is a template with their colors on.

**Real case:** a Qatom pitch page built from the press kit alone came out with the
right ink, the right ground, the right words, and none of the character. Their
homepage's entire identity is a live agent conversation buying a tool call with an
inline receipt. The page had no widget, so the brand was absent no matter how
correct the hexes were.

**Respect `prefers-reduced-motion`** for every effect added this way, and give any
autoplaying video a static poster so a blocked or slow load never leaves a hole.

**Pane gotcha:** an embedded browser preview pane can throttle background tabs, so screenshots can lag or return blank, and requestAnimationFrame can pause. When a screenshot looks wrong, verify via DOM state (`getBoundingClientRect`, computed styles, class lists) before assuming the page itself is broken.

## The candidate's asset harvest (before any markup)

The deep scan harvests the company's assets; this harvests the person's.
Inventory their asset folder completely: every photo, video, logo, and
document, with dimensions and one line on what it actually shows (open
them and look; a filename is not a description). Then two binding rules:

- When the brand's imagery system is PHOTOGRAPHY, the person's real photos
  ARE the page's imagery. Never build a gradient stand-in for a photo you
  were given. A brand that puts its product in real scenes gets the
  candidate in real scenes.
- When the brand ships product VIDEO LOOPS, the person's videos are the
  loops: muted, looping, in the brand's own frame treatment.

Every asset left unused appears in the parity table with a reason ("too
dark for their palette" is a reason; silence is not).

## The exemplar anatomy (measured from the Affirm and PointClickCare builds)

The two reference pages were deep-scanned like any brand, and five things
separate them from every page that came back "too short." All five are
requirements:

1. **The ambient layer.** At least three subtle infinite loops running at
   all times (measured on Affirm: seven, at 5-40s periods: a floating
   phone, a glowing chip, breathing arcs, marquee lanes, a nudging dock).
   A page that only animates on reveal is dead the moment the person stops
   scrolling. Slow, quiet, always moving.
2. **Full-JD coverage.** One section walks the ENTIRE posting, line by
   line, each line flagged with the receipt that answers it or the honest
   miss ("Every JD line, flagged"). This is where length comes from
   honestly. A speculative pitch with no posting flags the seat's implied
   mandate instead.
3. **Proof genres, plural.** Numbers, video ("The work, on tape"), quotes,
   and evidence-source ("Where the evidence comes from") are four
   different sections, not one list. Each proof type the candidate has
   gets its own room.
4. **Concept furniture in the chrome.** The concept lives in the nav and
   the floating elements, not only in sections: Affirm's cart icon in the
   nav, the offers dock, the payment-plan card. If the chrome could sit on
   any other concept's page, it is not furniture yet.
5. **Genre-named sections.** Every heading speaks the concept's language:
   "Shop the record.", "The full terms", "Numbers you can check." A
   generic heading ("What people say") is a drift flag; rename it inside
   the metaphor.

## The build is two passes, never one

A single writing pass converges to one visual idea per section and stops
around six sections; that is an economics problem in the writer, not a
design decision, and it produced every "too short" page this skill has
shipped. So the build is TWO passes by mandate:

**Pass one, the skeleton.** Concept, structure, tokens, the signature move.
Render it. Do not judge it; it is scaffolding.

**Pass two, the expansion.** Count sections against the depth number, then
expand section by section, each in its own edit: add the section's furniture
(sub-components, secondary rows, captions, chips, small illustrations, a
stat, a second beat of copy), and give EVERY section at least one designed
moment of its own: a micro-interaction, a reveal choreography, a working
element, or a photographic beat. The delight budget is per-section now, not
per-page: a page where seven sections carry nothing is a failed budget even
if three sections are excellent. Stop expanding only when the depth number
is met and no section is bare.

## The depth check has a number

Count the sections on the scanned product page nearest the role. The full
build ships at least two-thirds that many sections; the draft at least
half. A five-section page against a twelve-section brand fails the depth
check no matter how good the five are. Long pages are how these brands
build belief; a short page reads as a leaflet wearing their logo.

## Phase 0.5 — The candidate's mark (always required, always custom)

Look at the actual logo image you were given and name its shape language out loud before drawing anything: an arc, a ring of dots, a geometric monogram, a specific icon. Build a small SVG mark for the candidate that echoes that exact shape, using their real initial and the company's real measured brand color — not the old mark's colors repainted, and not a generic circle-with-a-letter.

**Real mistake this rule exists because of:** the second worked example (Thomson Reuters) shipped with the first example's (Affirm's) arc-shaped mark copy-pasted in, unadapted, because it got reused from the reference implementation without checking the new company's actual logo. It was caught and fixed after the fact, but it shouldn't have shipped that way at all. Check the nav mark specifically, against the actual logo image, before calling any build done — every single time, even the tenth time you use this skill.

## Phase 1 — Concept: the page IS their product, with the candidate inside

**This phase produces `concept.md`, before any markup. No concept file, no build.**

The finished page is not a resume in their colors. It is the company's own
product experience, delivering the candidate as the content. The format
adapts three ways at once: to the company's product, to the job's domain,
and to this person's actual material.

Worked examples of the move, from real builds:
- **Affirm** (buy-now-pay-later shopping): the page became a shopping
  experience. The candidate was the product in the cart, capabilities were
  line items, the offer was a payment plan.
- **PointClickCare** (care-coordination software): the page became a
  candidate advisor, the way their product advises on care.
- **Wealthsimple** (investing app): the page becomes a portfolio. The career
  is the performance chart, skills are holdings with returns, receipts are
  the activity feed, and the JD's own named features (WealthRank, Portfolio
  Pulse) appear with the candidate's data inside them.

`concept.md` must contain:
1. The company's core product experience in one sentence (what the user
   actually does in their product).
2. THREE candidate-as-product concepts. For each: the metaphor, what every
   section becomes under it, and what the signature interaction becomes.
3. The pick, with one line of why, biased toward the concept that uses
   products the JOB DESCRIPTION itself names. If the JD names features,
   those features are the page's furniture.

**The kill rule for concepts:** if the finished page can be described as
"hero, capability cards, honest gap, ninety-day plan," it has no concept,
no matter how good the brand match is. The metaphor must change the FORMAT:
sections carry the product's own names, navigation behaves like the product,
data is displayed the way the product displays data. The honest gap and the
plan still exist, but they arrive dressed as the product too (a gap can be a
"risk disclosure" on an investing page; a plan can be "upcoming orders" on a
shopping page).

**Concept meets fidelity:** the delight budget (Phase 0) gains one mandatory
line item here: at least ONE of the company's actual product surfaces,
rebuilt as a working component with the candidate's data inside it. Not a
picture of their UI: a functioning rebuild. A chart that draws, a feed that
updates, a cart that adds. This single component is where concept, brand,
and motion meet, and it is the thing a hiring manager screenshots.

## The words live in content.md

Every build writes `content.md` next to `index.html`, BEFORE the markup, and
the page is assembled from it. One block per page section, in page order,
holding the EXACT words that appear on the page: heading, body, labels,
numbers, button text. Under each block, a one-line source note saying where
each claim came from (resume line, interview answer, or the posting). Not a
copy of the resume; the page's own script.

Why one file and not two: people edit words they can see. A file of source
material makes them hunt for where a sentence came from; a file of exact page
words lets them fix a word in ten seconds. The source notes carry the
claims-register job inside the same file.

The round-trip is a feature of the kit: tell the person, at handoff, "the
words are in content.md. Edit any line, then tell me to sync your page, and
I will apply your words to the page without touching the design." When asked
to sync, apply content.md as the single source of truth for copy, change no
layout or motion, and never overwrite a human edit with your own phrasing.

## Phase 2 — Honesty framework (load-bearing, never cut)

- Three buckets, scored against specific lines in the actual JD: **Direct fit** (answers a specific JD requirement from the candidate's real record), **Adjacent, not identical** (real but not the same thing, named plainly, never blurred into a false equivalence), **The gap** (at least one real, undefended gap). A page with zero acknowledged gaps reads as unaware or dishonest — build one in, every time.
- The gap should also surface inside whatever live demo you built (Phase 1), with a line explaining *why* it's shown instead of hidden: trust is the actual product being sold here.
- Soften a gap only by reframing it honestly ("I understand the model and have used it, but my daily reps have been X, not Y" — a rep/frequency gap, not a competence denial). Never hedge it into non-existence, and never apologize for it at length.

## Phase 3 — Design system rules (each one learned by breaking it once)

- Match the company's actual lightness/darkness end to end. A page that's dark at the top and light at the bottom (or vice versa) reads as two different websites stitched together.
- One focal object per viewport, especially in the hero. Satellite cards, floating chips, and toasts fighting the headline for attention get deleted, not rearranged into a smaller version of the same clutter.
- No sticky-stack scrolling for content-dense sections; the trick only works when each panel is genuinely sparse enough to fit one viewport. If your content needs real reading time, let it scroll normally.
- Overlays (offer takeovers, promo banners) should never push page layout around. Anchor them to a fixed edge, and open them on a real click or a real scroll-trigger — never as an unsolicited auto-popup that fires before the user has context.
- In any "numbers you can check" section, the numbers must be the visually loudest element on the page, full stop. Watch CSS selector scoping here specifically — a selector meant for a caption can accidentally also match the big numeral inside it, silently shrinking the number you most wanted to be big. Verify computed font sizes of key numerals directly in the browser, don't trust the stylesheet by eye.
- Decorative brand motifs (an arc, a glow, a texture) belong in padding/whitespace zones, never laid behind live text.
- If the concept calls for light-colored cards on an otherwise dark page (product tiles, a cart drawer), that's a deliberate, sparing pop of light, not an accident of inconsistent theming.

## Phase 3.5 — The delight bar (how a standard gets enforced, not just stated)

Prose advice gets skimmed. A standard holds only when it is a failing test. Three
rules make that work:

**1. The scan is the spec. Assert measured values, not the existence of a value.**
Never write a test that asks "is there an easing curve." Write the number the scan
actually measured into the test: the exact cubic-bezier, the exact hover duration,
the exact card aspect ratio, the exact display size. Then "match their level"
becomes mechanical, and drifting off their brand fails the build instead of
shipping. Every row in the `design-notes.md` token table should have a
corresponding check in `qa.mjs`.

**2. Audit by surface, not by feature.** A page can pass every motion check and
still be dead across half its area. One rebuild passed nine separate motion tests
while its footer was two lines of 13px grey text. Enumerate every surface a
visitor can land on or touch, and require a state for each:

> nav and mark · primary and secondary buttons · section headings on reveal ·
> photographs · media cards and their overlay labels · quote or testimonial cards ·
> inline and list links · the hero · **the footer** · back to top

The footer is the reliable dead zone. Hold it to the hero's standard: a real
closing statement at display size, structured columns, its own call to action, and
at least one live or moving element. If the footer is the smallest type on the page
and nothing in it responds to a cursor, the page is not done.

**3. Ratchet: every miss caught by eye becomes a permanent test.** When a human
spots something the suite missed, that is two commits, never one. Fix the page,
then encode the miss so it cannot return, and prove the new test fails by
reintroducing the bug. The QA file grows monotonically. That is the entire
mechanism by which quality rises across builds instead of resetting each time.

Known dead zones worth checking every time, each found the hard way: the footer,
empty and loading states, the space between sections, link hovers in body copy,
the back-to-top affordance, and anything below the last call to action.

**Look at every cropped image at its real rendered size.** A landscape photo in a
portrait well loses about half its width, and the default centre crop cuts the
subject out. On one build a panel photo rendered as a shoulder and a microphone
with the candidate's face off the edge, and it passed every automated check
because CSS cannot see a face. State `object-position` explicitly on every cropped
photo, including when centre is correct, so it reads as a decision rather than a
default. Record where the subject sits in the asset manifest.

## Phase 4 — Copy rules

- Benefit + mechanism + number, in that order, everywhere it's possible. Verb-first CTAs. Quote the company's own real taglines only if they were actually found during the Phase 0 scan, and quote them honestly, not paraphrased into something they didn't say.
- Banned by default: "surface(s)" as a noun, "leverage" as a verb, vague "experiences/products/platforms" with no concrete referent, em dashes, emoji, and generic AI-assistant-speak.
- No years-of-experience bragging as a hero stat ("15 years!"). Tenure reads as age; outcomes read as value. Lead with awards, users/members reached, growth multipliers, and named, checkable accomplishments instead.
- Respect `<REDACTION_LIST>` — never name anything the candidate has flagged as confidential or off-limits, even when it would strengthen a claim. Describe the work anonymously instead ("a global strategy consultancy," not the firm's name) if the substance is worth keeping and the name isn't.
- If `<TESTIMONIALS_SOURCE>` has real quotes, include two of them as styled quote cards with real name, title, and context. Never trim a quote into implying something it didn't actually say.
- If `<PERSONAL_SITE_URL>` exists, promote it as its own panel on the page ("This page is the [store/eval report/whatever the concept is]. [Domain] is the full [thing]."), not buried as a single footer link.
- Footer disclaimer, always: not affiliated with or endorsed by the target company, a personal unsolicited pitch for one specific application, built in their visual style as a gesture of care, not a claim of association. Any live demo on the page is illustrative.

## Phase 5 — Engineering skeleton

- Own standalone public repo, `<GITHUB_USERNAME>/<company-slug>` — **not** buried inside another project's repo. Public repo visibility is fine (repo visibility and search-engine indexability are two different axes — see the noindex point below); if there's ever doubt about whether to make a specific repo public or private, ask, don't assume the last answer still applies to a new company.
- Add `<meta name="robots" content="noindex, nofollow, noarchive">` to the page. **Do not** also add a root `robots.txt` `Disallow` for this path — a `Disallow` blocks crawlers from ever fetching the page at all, which means they never get to read the `noindex` tag either, and that combination can paradoxically produce a bare, snippet-less search result if the link ever leaks elsewhere. `noindex` alone, with crawling allowed, is the correct combination. No Open Graph tags, so no rich link-preview card encourages wider sharing.
- Content pipeline: prose lives in `content/*.md` files (raw HTML fragments, not literal markdown prose), structure lives in `template.html` with `<!--content:token-->` markers, and a small zero-dependency `scripts/build.mjs` stitches them into `index.html`. Verify a byte-identical round trip before the first commit.
- `scripts/qa.mjs`, zero dependencies, checking at minimum: noindex present (critical section, checked first), document integrity (balanced tags, unique ids), CSS integrity (brace balance, no orphaned top-level declarations, load-bearing selectors present), the page's own structural/design rules from Phase 3 (encode every one as a failing test, so a regression is caught automatically instead of relying on someone noticing), accessibility (`rel=noopener` on external links, `prefers-reduced-motion` respected), responsive breakpoints present, `node --check` on all inline JS, brand/copy rules from Phase 4, and a `--live` mode that fetches the deployed URL(s) and checks for 200 status, byte-for-byte parity with the local build, and continued `noindex` presence.
- Any count-up animation driven by `requestAnimationFrame` needs a `setTimeout` fallback to its final value — rAF silently pauses in backgrounded/occluded browser tabs, which can leave a number stuck at 0 for anyone who isn't looking at the tab the instant it entered view.
- **Deployment is a ladder, and the build never blocks on it.** Ask what they
  have before assuming anything:
  1. **Nothing (no GitHub, no domain) — the normal case for a first-time
     student.** Build the page fully local and self-contained, open it in
     their browser so they see it working, and say plainly: the page is done,
     it lives in this folder, and publishing is a separate ten-minute step
     whenever they want a link to send. Do not treat local-only as failure.
  2. **A GitHub account.** A free account is all it takes: create the repo,
     push, enable GitHub Pages, and they have a real URL. Offer to walk them
     through creating the account if they do not have one; it is free and
     this is the recommended path to a sendable link.
  3. **A domain they control (optional).** FTP sync via a GitHub Actions
     workflow (e.g. `SamKirkland/FTP-Deploy-Action`). If you're setting this
     up for someone else, **never see or type the FTP password yourself** —
     have the human set that one secret at a hidden terminal prompt;
     non-secret values like host/username can be set directly.
  The page itself must be built so every rung works: one folder, relative
  paths, no build step, so the same files open from disk and serve from
  Pages identically.
- Commit per logical phase, with commit messages that explain *why*, not just *what*. Push only once `qa.mjs` is fully green.

## Phase 6 — Verify like QA, not like hope

1. Browser pass at a narrow width (~390px) and a desktop width (~1280px): zero horizontal overflow (check via JS, `document.documentElement.scrollWidth - innerWidth`), no console errors, every interactive flow exercised end to end (add-to-cart through to a checkout link; a demo triggered through to its finished state; any dialog opened and closed).
2. Screenshot proof of each major section. If a preview pane is throttling and screenshots look stale or blank, fall back to asserting DOM state directly instead of trusting the pixels.
3. Full `qa.mjs` green locally before every push; `--live` green after every deploy.
4. Read the whole page top to bottom once, as the actual recruiter would. Anything that makes you want to re-read it gets rewritten before you call it done.

## Phase 6.5 — Design QA: side by side or it did not happen

Before anything ships, screenshot the built page at desktop (1440) and mobile
(390), full height, and put them next to the Phase 0 screenshots of the company's
real site. Judge them as a pair:

- Type scale and display ceiling within range of theirs.
- Spacing rhythm and section density match: their whitespace, not yours.
- Color roles used the way they use them, not just the same hex values.
- The signature move present and actually working.
- Motion at their measured level, in their dominant curve.
- No dead zones: no section that is plain text where they would put media or a
  component.

Then the parity table: every delight-inventory entry gets a verdict,
**reproduced, adapted, or skipped with a reason**. More than half skipped
means the build failed regardless of how it looks. Then the kill question,
out loud: **could this page be mistaken for a template
with their colors?** If yes, it fails, no matter how many checks passed. Fix,
re-shoot, compare again. The comparison happens against screenshots, never
against memory. Deployment happens only after this gate.

## The page ships clean. Provenance lives in the repo, never on the page.

The finished page carries nothing about how it was made: no kit credit, no
course mention, no "brand measured from" notes, no scan dates, no AI mention.
The candidate sends this to a hiring manager; every visible word must serve
that reader. Where the record lives instead: design-notes.md and
delight-inventory.md in the repo, and the git log. The only exception is a
demo page built for teaching, which carries a visible demo banner precisely
because it is NOT a real application.

## Phase 7 — Log it

Keep one running ledger file (e.g. `job-applications.md`) with one line per page built: date, company, role, repo + live URL(s), status. Chat history is not a durable record — the ledger is.

## What this skill is not

Not a mass-application blast tool. One page, for one specific application the candidate has already decided to make. The "should I even apply here" judgment call happens *before* this skill runs, not as a side effect of running it.
