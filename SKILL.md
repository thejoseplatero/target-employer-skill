---
name: target-employer
description: Build a world-class, company-branded landing page pitching a candidate for a specific job posting. Inputs are a full live scan of the company's site, the job description, the candidate's record, and the company's logo. Output is a standalone repo deployed to GitHub Pages (and optionally a custom domain via FTP), noindexed, markdown-editable, gated by its own automated QA suite. Use when someone is applying somewhere and wants a version "for them."
---

# Target Employer

One company, one role, one page that sells the candidate the way the company sells its own product. Not a template swap: a recruiter should open it and feel it was built specifically for their brand, using their own product's mechanics as the organizing metaphor.

Two full worked examples, built with this exact skill, are linked in the README. Read them before building a new one — their `design-notes.md` files and git logs are a record of every mistake already made once, so you don't repeat it.

## Before you start: fill in your own details

This skill is written for **`<CANDIDATE_NAME>`** applying with:
- Resume / CV source: `<RESUME_SOURCE>` (a file path, doc, or pasted text — whatever you'll actually keep current)
- Real testimonials / recommendation quotes source, if you have one: `<TESTIMONIALS_SOURCE>` (optional, but strongly recommended — see Phase 4)
- Personal site or portfolio to cross-promote, if you have one: `<PERSONAL_SITE_URL>` (optional)
- Any topics that must never be named publicly (e.g. an employer's confidential client list, an NDA'd project name): `<REDACTION_LIST>` (optional but check before every build)
- Your own GitHub account, for the standalone repo: `<GITHUB_USERNAME>`
- A domain you control, if you want a custom-domain deploy alongside GitHub Pages: `<CUSTOM_DOMAIN>` (optional — GitHub Pages alone is enough to ship)

Wherever this file says `<CANDIDATE_NAME>`, `<RESUME_SOURCE>`, etc., substitute your own answers. Do this once, then reuse the skill for every application.

## Inputs (ask, don't guess)

1. Company name + their real marketing site URL
2. The full job description text — paste the whole thing, not a summary
3. `<CANDIDATE_NAME>`'s record: `<RESUME_SOURCE>`, `<TESTIMONIALS_SOURCE>`, `<PERSONAL_SITE_URL>` content, plus anything said in-conversation. **Never invent a metric, a team size, or an outcome.** If a number isn't in the source material, don't cite it.
4. Fit concerns or specific hooks the candidate has already flagged for this application
5. **The company's logo, as an image.** Always ask for it if it hasn't been shared yet. Needed for Phase 0.5 below — never skip this, and never reuse a mark built for a different company's logo.

## Phase 0 — Full brand scan (browser, not curl)

Curl-scraping color tokens is the floor, not the scan. Open the company's real site in a browser and walk all of it:

- Scroll the entire homepage capturing every section: hero formula, scroll motion (sticky stacks, parallax, reveals), signature components (their checkout module, feed, search, cards — whatever their product's actual UI is built from)
- Open 1-2 secondary pages (a product or category page, a "how it works" page) and any mega-menu
- Extract measured tokens via JS in the page: computed colors, font-family, border-radius on buttons/panels, nav treatment. If browser tooling is blocked on a domain, fall back to `curl -A "Mozilla/5.0" <url>` plus grepping their live CSS bundles for hex colors, font-family, and border-radius — real measured values beat a guess every time.
- Capture their copy voice as a formula (benefit headline + one concrete sub + one verb-first CTA + numbers as proof — or whatever their actual pattern is)
- Write everything to `design-notes.md` in the new repo. This file is the contract for every later edit; "vibes" are not a reference, and neither is memory of what a *different* company's brand looked like.

**Pane gotcha:** an embedded browser preview pane can throttle background tabs, so screenshots can lag or return blank, and requestAnimationFrame can pause. When a screenshot looks wrong, verify via DOM state (`getBoundingClientRect`, computed styles, class lists) before assuming the page itself is broken.

## Phase 0.5 — The candidate's mark (always required, always custom)

Look at the actual logo image you were given and name its shape language out loud before drawing anything: an arc, a ring of dots, a geometric monogram, a specific icon. Build a small SVG mark for the candidate that echoes that exact shape, using their real initial and the company's real measured brand color — not the old mark's colors repainted, and not a generic circle-with-a-letter.

**Real mistake this rule exists because of:** the second worked example (Thomson Reuters) shipped with the first example's (Affirm's) arc-shaped mark copy-pasted in, unadapted, because it got reused from the reference implementation without checking the new company's actual logo. It was caught and fixed after the fact, but it shouldn't have shipped that way at all. Check the nav mark specifically, against the actual logo image, before calling any build done — every single time, even the tenth time you use this skill.

## Phase 1 — Concept: sell the candidate as the company's own product

The organizing idea comes from the company's own product mechanics, executed literally, not decoratively:

- Commerce company → the page IS a store: a product grid of shipped work (badge, illustration, category, a rating drawn from real accolades, an impact-stat "price", a working add-to-cart), a cart flyout drawer whose checkout mailto lists the cart contents, offers as a bottom-anchored dock that pops a bottom-sheet takeover
- Trust/verification/research company → the page IS an eval report or a cited research brief: every claim about fit gets scored (pass / adjacent / disclosed gap) and cited, mirroring how their own product presents verified, sourced answers
- Whatever the company's core mechanic actually is, that's the metaphor. Don't force a shopping cart onto a company that sells trust and verification, and don't force a citation system onto a company that sells impulse-buy commerce.
- The hero sells with the company's own copy formula, not a generic "clever" hook. A vague, too-cute headline gets cut every time it shows up — benefit headline, one dek of pure receipts, one primary CTA, a numbers proof strip.
- The job description's single biggest, most specific ask gets a working demo, not a paragraph about it. If the JD's central ask is "build an evals framework," ship a live, clickable eval-run demo on the page itself. If it's "agentic search," ship an agent that visibly reasons through a query on screen. Whatever they're hiring for, prove you already do it, on this page, live.

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
- Deploy target 1: GitHub Pages, enabled via the GitHub API or repo settings. Deploy target 2 (optional): FTP sync to a custom domain via a GitHub Actions workflow (e.g. `SamKirkland/FTP-Deploy-Action`). If you're setting this up for someone else (an AI assistant helping a human candidate), **never see or type the FTP password yourself** — have the human set that one secret themselves at a hidden terminal prompt; non-secret values like the FTP host/username can be set directly.
- Commit per logical phase, with commit messages that explain *why*, not just *what*. Push only once `qa.mjs` is fully green.

## Phase 6 — Verify like QA, not like hope

1. Browser pass at a narrow width (~390px) and a desktop width (~1280px): zero horizontal overflow (check via JS, `document.documentElement.scrollWidth - innerWidth`), no console errors, every interactive flow exercised end to end (add-to-cart through to a checkout link; a demo triggered through to its finished state; any dialog opened and closed).
2. Screenshot proof of each major section. If a preview pane is throttling and screenshots look stale or blank, fall back to asserting DOM state directly instead of trusting the pixels.
3. Full `qa.mjs` green locally before every push; `--live` green after every deploy.
4. Read the whole page top to bottom once, as the actual recruiter would. Anything that makes you want to re-read it gets rewritten before you call it done.

## Phase 7 — Log it

Keep one running ledger file (e.g. `job-applications.md`) with one line per page built: date, company, role, repo + live URL(s), status. Chat history is not a durable record — the ledger is.

## What this skill is not

Not a mass-application blast tool. One page, for one specific application the candidate has already decided to make. The "should I even apply here" judgment call happens *before* this skill runs, not as a side effect of running it.
