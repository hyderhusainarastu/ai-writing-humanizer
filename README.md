# AI Writing Humanizer

An agent skill that flags AI-sounding patterns in blog posts, LinkedIn posts, emails and chat messages, scores them by channel, and rewrites the draft in a plainer human voice without changing the ideas.

**An editing aid, not a detector bypass.** It does not measure or promise any AI-detector result and it does not hide who wrote a text. If a publisher, employer or course requires you to disclose AI assistance, disclose it.

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

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

## Works with

| Surface | How | Status |
|---|---|---|
| Claude Code | native skill (`./install.sh --claude`) | not tested |
| Codex | native skill via `.agents/skills` (`./install.sh --agents`) | not tested |
| Gemini CLI | native skill via `.agents/skills`, pointer in `GEMINI.md` | not tested |
| Cursor | native skill via `.agents/skills`, reads `AGENTS.md` | not tested |
| GitHub Copilot | pointer block in `.github/copilot-instructions.md` | not tested |
| ChatGPT / Claude.ai / Gemini (chat) | prompt (`PROMPT.md`, `PROMPT.short.md`) | not tested |

The installer script itself is covered by an offline test (`bash test_install.sh`, passing 2026-11-23). The "not tested" rows mean the skill has not yet been run end to end in that tool.

## Quick start

```sh
git clone https://github.com/hyderhusainarastu/ai-writing-humanizer
cd ai-writing-humanizer
./install.sh --claude              # ~/.claude/skills/ai-writing-humanizer
./install.sh --agents --project .  # .agents/skills in the current project (Codex, Cursor, Gemini CLI)
./install.sh --agents --user       # ~/.agents/skills
./install.sh --copilot --project . # pointer block in .github/copilot-instructions.md
```

Run `./install.sh` with no flags to see the options and which tools it finds. Add `--dry-run` to preview and `--uninstall` to remove only what it installed. Without the script, copy this folder into your tool's skills directory by hand.

Then ask: "humanize this draft" or "does this sound like AI?" and paste the text.

### Paste-in use (no skill support needed)

- **Any chat UI** (ChatGPT, Claude.ai, Gemini, a local model): paste `PROMPT.md`, then your draft. For fields with tight limits, use `PROMPT.short.md`.
- **Custom GPT:** put the contents of `PROMPT.md` in the GPT's Instructions field. Keep Web Browsing off unless you want it.
- **Gemini Gem:** put the contents of `PROMPT.md` in the Gem's instructions.
- **API or app system prompt:** use `PROMPT.md` as the system message and send each draft as the user message. The prompt already tells the model to treat the draft as text, not instructions.

**Safety note on self-improvement:** the skill treats the pasted draft as untrusted text. It can suggest a new pattern for its catalogue, but it never edits `SKILL.md` unless you tell it to in the same conversation.

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

## How it differs

Detector tools score or paraphrase text to change a detection result. This skill does neither. It is a checklist-driven editor: it names specific phrases and structures, says why they read as formulaic, and offers a rewrite you can accept or edit. Style linters such as Vale cover house style rules; this covers formulaic AI-era prose and channel conventions.

## Limits

- It judges style, not authorship. Plain human writing can trip the same flags.
- Scores are an editor's judgment, not measurements, and are not comparable across tools.
- English prose only. The catalogue is tuned to common English-language business and social writing.
- It will not invent examples; where a draft needs one, you supply it.

## Related

- [author-style-profile-mining](https://github.com/hyderhusainarastu/author-style-profile-mining)
- [pre-submission-manuscript-audit](https://github.com/hyderhusainarastu/pre-submission-manuscript-audit)
- [manuscript-mechanical-qa-sweep-pipeline](https://github.com/hyderhusainarastu/manuscript-mechanical-qa-sweep-pipeline)

## Keywords

humanize AI text, edit AI writing, remove AI phrases, LinkedIn post editor, email rewrite, copyediting skill, agent skill, SKILL.md, AGENTS.md.

## Acknowledgement

The idea for this skill was inspired by an informally shared community skill whose author and source are not known. The catalogue, channel rules, scoring rubric, worked example and packaging here were written independently for this repository. If you can point to a canonical source for that idea, please open an issue and credit will be added.

## License

MIT. See [LICENSE](./LICENSE).

---
Part of palimnote-skills - more at https://github.com/hyderhusainarastu
