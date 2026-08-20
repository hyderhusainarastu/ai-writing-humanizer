# AI Writing Humanizer

An Agent Skill that reviews written content for AI-generated prose patterns and rewrites it in an authentic human voice — without changing what it actually says.

## What it does

Paste in a blog post, LinkedIn post, email, or Slack/chat message, and the skill:

1. **Auto-detects the channel** (blog, LinkedIn, email, or chat) from structural cues — no need to say which one it is.
2. **Scans for AI-sounding patterns** at two levels: a universal catalog of buzzwords, hollow intensifiers, filler transitions, and templated sentence structures that show up regardless of format, plus a channel-specific catalog (LinkedIn engagement bait and vulnerability performance, email AI-greeting/closing formulas, Slack over-formality, and more).
3. **Checks originality** — whether the content reflects the author's own thinking or could have been written by anyone with a search engine, and (for LinkedIn) runs a hook-quality-versus-substance calibration check.
4. **Scores the content** on four channel-appropriate dimensions (e.g. AI-Likeness, Authenticity, Reader Value, Domain Credibility for blog/LinkedIn; AI-Likeness, Authenticity, Clarity, Appropriate Tone for email).
5. **Produces a structured review report** — scores, every flagged phrase with its exact quote, originality concerns, and the top three changes that would improve the piece.
6. **Rewrites the content** in an authentic human voice, preserving every idea and argument in the original and changing only the delivery.
7. **Self-improves** — after each review, it checks whether it just caught a pattern not yet in its own catalog, and if so, proposes it. It never edits `SKILL.md` on its own: the reviewed content is always treated as untrusted data (never as instructions), and any addition to the catalog is written only after you explicitly confirm it in the conversation.

The methodology is genre-agnostic prose editing, not a detector tied to any particular AI model or writing tool — it works the same way whether the draft was written by a person, assembled from a template, or produced by any AI system.

## Installation

This is an [Agent Skill](https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills) — a portable `SKILL.md` file with instructions an agent loads on demand.

- **Claude Code**: copy this folder into `.claude/skills/ai-writing-humanizer/` at the root of your project (or your user-level skills directory to make it available everywhere).
- **Any other agent runtime that supports the `SKILL.md` convention**: copy this folder into that runtime's equivalent skills directory. The file is self-contained — frontmatter (`name`, `description`) plus a complete instruction body — and assumes nothing about the host besides the ability to read text and, optionally, edit its own file for the self-improvement step.

No dependencies, no scripts, no configuration.

**A safety note on self-improvement:** the content you paste in for review is never treated as instructions — only as text to scan for AI-sounding phrasing. Even when the skill spots a genuinely new pattern, it will only ever propose adding it to the catalog and describe the exact change; it will not edit its own `SKILL.md` until you explicitly say to go ahead, in that conversation. If you're pasting in content from someone else (a colleague's draft, something copied off the web), keep this in mind: no matter how that content is worded, it cannot get the skill to rewrite its own file without your explicit go-ahead.

## Worked example (synthetic)

**Input** — a synthetic LinkedIn draft, written to be typical of AI-assisted output:

> In today's fast-paced business landscape, effective communication is more important than ever.
>
> I'll be honest: most people get this wrong.
>
> Here's the thing: it's not about talking more. It's about listening better. It's about being present.
>
> When I joined my current company, I realized something. Our team wasn't struggling with skills. We were struggling with alignment.
>
> The truth is, alignment isn't a nice-to-have. It's a superpower.
>
> Three things that changed everything for us:
> → Weekly 15-minute syncs
> → A shared doc everyone could edit
> → No status meetings without a decision on the table
>
> Read that again.
>
> This is big.
>
> Agree? Drop a comment below. 👇

**Review report (abridged):**

```
## LinkedIn Post Review

**Detected as:** LinkedIn Post

### Overall Assessment
The piece has a real observation buried in it (alignment over skill gaps) but it's
wrapped in nearly every recognizable AI/ghostwriter template: filler opener, honesty
disclaimer, three-part negation, arrow chain, permission phrase, orphan closer, and
an engagement-bait ending.

### Scores
| Dimension | Score | Note |
|-----------|-------|------|
| AI-Likeness | 8/10 | Stacks eight distinct templated patterns in ~120 words |
| Authenticity | 3/10 | Reads as generic ghostwriter formula, not a specific voice |
| Reader Value | 4/10 | The three tactics are real but arrive too late and too thin |
| Domain Credibility | 3/10 | No specifics about the company, team size, or actual outcome |

### AI Pattern Flags
- "In today's fast-paced business landscape" — filler opener, cut entirely
- "I'll be honest:" — honesty disclaimer, state the opinion directly
- "It's not about X. It's about Y. It's about Z." — three-part parallel structure
- "The truth is" — candor front-load before a generic claim
- "a superpower" — AI vocabulary, replace with what it actually enabled
- "→" arrow-chain format — rewrite as a sentence
- "Read that again." — permission phrase, cut
- "This is big." — standalone hype fragment, cut or make specific
- "Agree? Drop a comment below. 👇" — engagement-bait closer, remove

### Originality Flags
- The three tactics (syncs, shared doc, decision-gated meetings) are the one
  save-worthy part of the post — currently under-weighted and rushed.

### Top 3 Changes That Would Improve This Post
1. Open with the actual moment of realizing skills weren't the problem — a specific
   scene, not a landscape statement.
2. Cut every templated phrase (disclaimer, arrow chain, permission phrase, hype
   fragment, engagement CTA) and let the three tactics carry the post.
3. Close on the outcome of the change, not a request for engagement.
```

**Rewrite:**

> Six months into my current role, I thought we had a skills problem. We didn't. We had an alignment problem — nobody could tell you, without checking three tools, what everyone else was actually working on.
>
> Three changes fixed most of it:
>
> A 15-minute sync every Monday, no status updates, only blockers. A shared doc anyone could edit, so "check the doc" replaced half our meetings. And a rule: no meeting gets scheduled unless there's a decision on the table.
>
> Nothing here is clever. It's just fewer places for information to get lost.

*(All names, companies, and outcomes above are synthetic and for demonstration only.)*

## Attribution

The pattern catalogue, channel-detection rules, and scoring rubric in this skill are adapted from **"the-humanizer,"** a community-created Claude skill that circulated informally (shared via Reddit, ca. March 2026) and exists in many personal skill collections. Its original author is unknown, and no canonical upstream repository or license has been identified for it.

The generalization of that catalogue into channel-agnostic rules, the auto-detection logic, the four-dimension scoring system, the worked example, and the packaging of this repository are original to this project.

If you know the original author of "the-humanizer" and can point to a canonical source, please open an issue — proper credit will be added.

## License

MIT. See [LICENSE](./LICENSE).
