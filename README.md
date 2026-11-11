# smart-brevity

An agent skill that makes every agent reply **and every plan** short and easy to scan, and keeps every fact.

Agent output is often long and buried in filler. This skill puts the answer in
the first sentence, then lays out the rest in a shape that fits the content:
task report, how-to, explainer, options, findings or plan. No news-style labels.

## What's inside

- `smart-brevity/SKILL.md`: the rules (the first-sentence rule, the shape picker and shapes, formatting, keeping facts, word choice, plans, opt-out).
- `smart-brevity/references/examples.md`: before/after pairs for every shape.
- `smart-brevity/references/word-choice.md`: word swaps (fancy → plain, foggy → specific, passive → active).
- `smart-brevity/snippets/system-prompt.md`: a condensed version to paste into any agent's system prompt.

## Setup

There are three levels. Step 1 makes the skill available, step 2 uses it in one
session, and step 3 makes it always on. For the full effect, do steps 1 and 3.

### 1. Install the skill

Clone the repo, then link the skill folder into your skills directory. A symlink
keeps the skill in sync with the repo: `git pull` updates it.

```sh
git clone <repo-url> ~/Repositories/smart-brevity-skill
cd ~/Repositories/smart-brevity-skill

# For all your projects (user level)
mkdir -p ~/.claude/skills
ln -s "$PWD/smart-brevity" ~/.claude/skills/smart-brevity

# Or for one project only (commit it to share with your team)
mkdir -p /path/to/project/.claude/skills
ln -s "$PWD/smart-brevity" /path/to/project/.claude/skills/smart-brevity
```

To copy the folder instead of linking it, use `cp -r smart-brevity ~/.claude/skills/`.
A copy doesn't update when you pull.

Start a new Claude Code session. The skill loads at session start, and
`smart-brevity` appears in the skill list.

### 2. Use it in a session

Once installed, Claude loads the skill on its own when a reply fits its
description. To call it on demand:

- Type `/smart-brevity` in the prompt, or
- ask in plain words: "use smart brevity for this".

**Note:** a loaded skill shapes the replies that follow, but the model can drift
back to its default style in long sessions, and plan mode doesn't always load
skills. Step 3 fixes both.

### 3. Add it to the system prompt (always on)

`snippets/system-prompt.md` is a short version of the rules for the system
prompt, so every reply and every plan follows them from the first message.

**Claude Code: import it (recommended).** Add this line to `~/.claude/CLAUDE.md`
for all projects, or to a project's `CLAUDE.md` for one project:

```markdown
@~/Repositories/smart-brevity-skill/smart-brevity/snippets/system-prompt.md
```

The `@path` import reads the file at session start, so edits to the snippet
apply in the next new session without touching `CLAUDE.md`. Use the path where
you cloned the repo.

**Claude Code: paste it.** Or paste the contents of the snippet into
`CLAUDE.md`. This works without the repo, but you have to paste it again after
every update.

**Other agents and the API.** Paste the snippet into the agent's system prompt
or custom-instructions field. For the Claude API, add it to the `system`
parameter.

**Check:** in a new session, run `/memory` to see the loaded `CLAUDE.md` files,
or ask a question and check that the first sentence carries the answer.

## Opt out

Tell the agent "no smart brevity", "full detail", "long form" or "write it as prose".

## Credit

The method comes from *Smart Brevity* by Jim VandeHei, Mike Allen and Roy Schwartz
(Axios). The skill paraphrases its rules for agent output. This project is not
affiliated with Axios.
