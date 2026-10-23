# smart-brevity

An agent skill that makes every agent reply **and every plan** short and easy to scan, and keeps every fact.

**Why it matters:** agent output is often long and buried in filler. This skill
puts the answer first, says why it matters, then gives the details as bullets.

## What's inside

- `smart-brevity/SKILL.md`: the rules (the Core 4, formatting, word choice, plans, opt-out).
- `smart-brevity/references/examples.md`: before/after pairs, including a plan.
- `smart-brevity/references/word-choice.md`: word swaps (fancy → plain, foggy → specific, passive → active).
- `smart-brevity/snippets/system-prompt.md`: a condensed version to paste into any agent's system prompt.
- `original-book-tips-and-trics.txt`: source notes from the book. Not part of the skill.

## Install

- **Claude Code:** `ln -s "$PWD/smart-brevity" ~/.claude/skills/smart-brevity`
- **Always on, plan mode included:** add the contents of `snippets/system-prompt.md` to `~/.claude/CLAUDE.md` or to your agent's system prompt.

## Opt out

Tell the agent "no smart brevity", "full detail", "long form" or "write it as prose".

## Credit

The method comes from *Smart Brevity* by Jim VandeHei, Mike Allen and Roy Schwartz
(Axios). The skill paraphrases its rules for agent output. This project is not
affiliated with Axios.
