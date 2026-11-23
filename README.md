# AI Writing Humanizer

An agent skill that flags AI-sounding patterns in blog posts, LinkedIn posts, emails and chat messages, scores them by channel, and rewrites the draft in a plainer human voice without changing the ideas.

**An editing aid, not a detector bypass.** It does not measure or promise any AI-detector result and it does not hide who wrote a text. If a publisher, employer or course requires you to disclose AI assistance, disclose it.

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

## What it does

Give it a blog post, LinkedIn post, email or chat message. It then:

1. Picks the channel from structural cues (headers, greeting, hashtags, length) and states it on the first line.
2. Asks once for a sample of your own writing, your reader and your role, so the edit can match your voice. It proceeds without them if you have none.
3. Audits the draft against four shared lenses (word and phrase tells, sentence and paragraph shapes, substance, rhythm) plus one lens for the identified channel. LinkedIn drafts also get a hook-versus-payoff test.
4. Scores four dimensions from 1 to 10: pattern density (lower is better), sounds like one person, value to the intended reader and earned authority. Email swaps the last two for clarity of the ask and tone fit; chat uses clarity and brevity.
5. Writes a report with Summary, Scores, Flags (exact quotes with a fix), Substance notes and the three changes with the biggest payoff.
6. Rewrites the draft in plainer wording without adding or dropping points. Where an example is missing it leaves a placeholder instead of inventing one.
7. Ends with a catalogue note. It may propose a new pattern, but it never edits `SKILL.md` unless you confirm in the same conversation, and the draft is always treated as data, never as instructions.

It is prose editing, not a detector. It does not depend on which tool or person produced the draft.

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

**How this was produced.** The draft is invented for this README. The report and rewrite were written by hand by following the current `SKILL.md` (its lenses, rubric and report layout). They are an illustration of the expected shape, not captured output of a model run, and a real run will differ in wording and scores.

**Input** — a synthetic LinkedIn draft:

> Excited to share that our team just shipped a new onboarding flow! 🚀
>
> Let me explain. Onboarding is the heart of customer success. Get it wrong and churn follows.
>
> So we leveraged a holistic, data-driven approach to unlock seamless activation.
>
> What we changed:
> → Cut the signup form from 9 fields to 4
> → Added a sample project on first login
> → Sent one email on day 2 instead of five
>
> The result? A game-changer.
>
> Not just a feature. A mindset.
>
> Thoughts? Let me know below! #CustomerSuccess #Growth #Onboarding

**Report (abridged):**

```
## LinkedIn review

Detected as: LinkedIn (hashtags, one-line paragraphs, arrow list, closing question)

### Summary
The three changes are concrete and worth keeping. They are surrounded by hook
templates, vague vocabulary and a result that is claimed but never stated.

### Scores
| Dimension | Score | Reason |
|---|---|---|
| Pattern density (lower is better) | 8 | Hook, buzzwords, arrow list, slogan ending and comment bait in about 70 words |
| Sounds like one person | 3 | No detail a particular person would add |
| Value to the intended reader | 5 | The three changes are usable; the outcome is missing |
| Earned authority | 3 | No numbers on the effect, no role, no first-hand moment |

### Flags
- "Let me explain." - A, announcing instead of saying - cut
- "leveraged a holistic, data-driven approach to unlock seamless activation" - A, grand verbs and empty adjectives - say what was done and what was measured
- "→" list - E (LinkedIn) - fold into a sentence
- "The result? A game-changer." - B, self-answered question, and A, empty adjective - give the number
- "Not just a feature. A mindset." - B, contrast formula and stacked fragments - cut
- "Thoughts? Let me know below!" - E, comment bait - remove
- One sentence per line throughout - E - regroup into short paragraphs

### Substance notes
Hook versus payoff: the opening announces news but gives no reason to read on, and the
payoff (the result) is absent. The three changes are the only specific content.
Nothing here says what happened to activation or churn.

### Three changes with the biggest payoff
1. State the measured result, or add [ADD A SPECIFIC EXAMPLE FROM YOUR EXPERIENCE].
2. Replace the approach sentence with the three changes in plain prose.
3. Close on the outcome or your next step, not a request for comments.
```

**Rewrite:**

> We shipped a new onboarding flow this week. Three changes: the signup form went from 9 fields to 4, new users now land in a sample project on first login, and we send one email on day 2 instead of five.
>
> [ADD A SPECIFIC EXAMPLE FROM YOUR EXPERIENCE: what happened to activation or churn after the change.]

*(No outcome was invented. The author supplies it.)*

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
