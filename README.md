# target-employer

A [Claude Code](https://claude.com/claude-code) skill that turns a job posting into a standalone, company-branded landing page pitching you for the role — built in the target company's own visual language, using their own product as the metaphor for why you fit.

Not a resume template. Not a form letter. The page is built fresh for one specific company and one specific role, scanned from their real live site, and it ships as its own tested, deployed, shareable link — something you send instead of (or alongside) a cover letter.

## See it before you build one

Two real examples, built with this exact skill, are live right now:

- **[joseplatero.com/affirm](https://joseplatero.com/affirm/)** — applying to Affirm (a commerce/BNPL company) as a shopping experience: a real product grid of shipped work, a working cart, an Affirm-style checkout module, an Uber-Eats-style offer takeover.
- **[joseplatero.com/thomsonreuters](https://joseplatero.com/thomsonreuters/)** — applying to Thomson Reuters (a trust/verification company) as a live eval report: every fit claim scored pass/adjacent/disclosed-gap with citations, a clickable "run the eval suite" demo, Thomson Reuters' real measured brand colors and typography.

Same skill, two completely different pages, because the concept is always pulled from *that specific company's own product*, never from a template.

## What you get

- A standalone repo (yours, on your own GitHub account) containing the page, a markdown-based content-editing pipeline, and a full automated QA suite
- Deployed to GitHub Pages automatically, and optionally to a custom domain of yours via FTP
- `noindex, nofollow` on the page itself — built for direct sharing (email, LinkedIn message, application form), deliberately not meant to show up in search
- A visual identity built around your real logo-vs-their-logo, so it looks intentional, not templated
- An honest fit breakdown (direct fit / adjacent / disclosed gap) instead of a highlight reel — recruiters trust a page more when it admits something, not less

## Quick start

1. **Install the skill.** Copy `SKILL.md` from this repo into `.claude/skills/target-employer/SKILL.md` inside whatever project or "second brain" folder you use with Claude Code. (Any tool built on the Claude Agent SDK that supports the Skills format works the same way — this isn't Claude-Code-specific.)
2. **Fill in your own details.** Open the copied `SKILL.md` and fill in the placeholders in the "Before you start" section at the top: your name, where your resume/CV lives, where any testimonials live, your GitHub username, and so on. Do this once.
3. **Gather your inputs** (see the checklist below).
4. **Invoke it.** In a Claude Code session, either type `/target-employer` (if your tool exposes skills as slash commands) or just describe the task in plain language — see example prompts below. Claude will read the skill and follow its phases.
5. **Review, then send the link.** The skill verifies its own work (QA suite, live checks), but you're still the one deciding whether the honesty and tone are right before you send it to anyone.

## What to prepare, for the best outcome

The single biggest lever on quality is **what you hand Claude before it starts**. Vague inputs produce a vague page. Specific inputs produce a page that reads like it actually knows you.

| Input | Why it matters |
|---|---|
| **The company's real name and marketing site URL** | The skill scans their live site for real colors, fonts, and voice — it does not guess a "generic tech company" look. |
| **The full job description, pasted in whole** | Not a summary. The skill mines exact phrases and requirements from the JD's own wording — summarizing it first throws away the material it works from. |
| **Your resume or CV, as a file it can read** | This is the only source of truth for your numbers, titles, and dates. The skill is instructed never to invent a metric — if it's not in your source material, it won't appear on the page. |
| **Real testimonials or recommendation quotes, if you have any** | Two real, attributed quotes are worth more than five more bullet points about yourself. Optional, but noticeably strengthens the page when included. |
| **The company's logo, as an image** | The skill builds you a small personal mark that echoes the shape of their real logo (their arc becomes your arc, their dot-ring becomes your dot-ring). Without the real logo, this step can't happen properly. |
| **Any honest fit concerns, said out loud** | Tell Claude directly if you're worried about a gap — years of experience, company size, a skill you're weaker on. The skill is built to disclose exactly one real gap, undefended, because a page with zero gaps reads as less trustworthy, not more. Naming your own concern up front makes this section sharper. |
| **Anything that must stay confidential** | If a past employer, client, or project can't be named publicly, say so before the build starts, not after you spot it on the page. |

### Example prompt — good

> I want to apply to **Acme Health** for their **Senior Product Manager, Care Navigation** role. Here's the full job posting: [paste the entire JD].
>
> My resume is at `resume/cv.pdf`. I don't have testimonials handy, skip that section.
> My honest concern: they want 5+ years in regulated healthcare and I have 2, mostly adjacent in fintech compliance work — be upfront about that as the disclosed gap, don't try to talk around it.
> Here's their logo: [attach image].
> Build the landing page using the target-employer skill.

### Example prompt — too thin (avoid this)

> Make me a landing page for a PM job at Acme Health.

This gives Claude nothing to scan, nothing to cite, and nothing honest to disclose — it will either stall to ask you the missing questions (correct behavior) or, worse, fill the gaps with generic filler if you push it to "just build something."

## Customizing after the first build

The generated repo has a markdown content pipeline, so you don't need to touch HTML to fix a typo or reword a line:

```bash
# edit any file in content/*.md, then:
node scripts/build.mjs   # rebuilds index.html from your edits
node scripts/qa.mjs      # re-runs the full automated check suite
```

Commit and push only once `qa.mjs` reports everything green.

## FAQ

**Does this work with tools other than Claude Code?**
Yes — anything built on the Claude Agent SDK that supports the Skills format (a `SKILL.md` file with YAML frontmatter) can load and run this the same way.

**Will this page show up if someone Googles me?**
No, by design. It ships with `noindex, nofollow` and no social preview tags — built for you to share the link directly, not for search discovery.

**Can I make the repo private instead of public?**
Yes. The skill defaults to asking rather than assuming, since a public-vs-private choice made for one application shouldn't silently carry over to the next one.

**What if the company doesn't have an obvious "product mechanic" to build the page around (e.g. a law firm, a nonprofit)?**
The skill's Phase 1 explicitly says: pull the metaphor from whatever the company's *real* core mechanic actually is — a research/verification company gets a citation-and-evals framing (see the Thomson Reuters example), a commerce company gets a shopping framing (see the Affirm example). Look at what they actually do and sell, not at a generic industry stereotype.

## Credit

Built by [Jose Platero](https://joseplatero.com) as a personal tool, shared publicly in case it's useful to someone else running their own job search. Not affiliated with Affirm, Thomson Reuters, Anthropic, or any company referenced in the linked examples.

## License

MIT — see [LICENSE](LICENSE).
