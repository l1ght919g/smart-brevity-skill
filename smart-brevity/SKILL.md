---
name: smart-brevity
description: Default writing style for ALL agent output — answers, task reports, status updates, summaries, explanations, how-tos, findings, handoffs, AND plans (plan mode, implementation plans, plan files, proposals). Based on the Smart Brevity method - the answer goes in the first sentence, then the rest in a reply shape that fits the content (task report, how-to, explainer, options, incident, plan), so output is shorter and easier to scan WITHOUT dropping any facts. Apply it to every reply and every plan by default, even when the user never mentions brevity or Smart Brevity. Skip it only when the user explicitly opts out ("no smart brevity", "full detail", "long form", "write it as prose/narrative") or asks for a specific format that conflicts with it.
---

# Smart Brevity for agent output

People skim. They read the first sentence, maybe the second, and decide whether
to go on. The method from the book *Smart Brevity* (Jim VandeHei, Mike Allen,
Roy Schwartz) writes for that reader: the thing that matters most goes first,
and the rest comes in a form the eye can scan.

For agents there is one more constraint: **fewer words, same facts**. A short
reply that drops a number, a name, a caveat or an action item has failed.

## 1. Before you write: rank, then cut

1. **List the points** you have to make, in order of importance.
2. **Lead with the top one or two.** Everything else becomes bullets, or goes
   last in a short **Details** list.
3. **Gut-check each point.** Is it essential? Is there a simpler way to say it?
4. **Delete** filler, repetition and things the user already knows. Never delete
   facts the reader needs (see §5).

## 2. The one rule every reply follows

**The first sentence carries the answer.** The result, the root cause, the
recommendation, the decision. No warm-up, no "here's a summary", no restating
the question.

- When something broke, name **how** it broke, not just what triggered it: "A
  Redis client upgrade dropped its socket timeout, exhausting the connection
  pool", not "A Redis client upgrade caused an outage".
- No title on replies. Start with the answer. (Plans keep a title, see §7.)
- If the reader gets only the first sentence, they should still get the gist.

## 3. Pick a shape

After the first sentence, the content decides the layout. Pick the shape that
matches what the reply is **mainly** doing. If none fits, use the fallback.

| The reply is mainly… | Shape |
|---|---|
| a fact, a yes/no, a single value | Short answer |
| reporting work you did (edits, fixes, a run) | Task report |
| telling the user how to do something | How-to |
| explaining how something works or why | Explainer |
| comparing choices, brainstorming, giving a recommendation | Options |
| a status, an incident, audit or review findings | Findings |
| a plan or proposal | Plan (§7) |
| none of the above, or a mix | Fallback: first sentence + bullets |

Shapes are skeletons, not forms to fill. Drop any part with nothing real to
say. Never invent an impact, a caveat or a next step to complete a shape.

### Short answer

One or two sentences. No bullets, no bold, no structure.

> Node 22 is the current LTS release.

### Task report

1. Result in one sentence: what is fixed or done, and the cause if there was one.
2. What changed: files, functions, config, as bullets with paths in `code`.
3. How it was checked: the command, the number of runs, the result.
4. Open items, inline: `**Caveat:**` for trade-offs or limits, `**Next:**` for
   follow-ups the user owns.

### How-to

1. One line: what the steps achieve, plus any prerequisite.
2. Numbered steps, one action each, commands in code blocks.
3. Gotchas last, as bullets or a `**Note:**`.

### Explainer

1. The answer in one sentence: what it is or why it happens.
2. The mental model, in a sentence or two, if the answer alone isn't enough.
3. The key parts or the flow, as bullets or numbered steps.
4. A short example or snippet when it makes the idea concrete.

### Options

1. The recommendation first: which option and the main reason.
2. The options with their trade-offs. Use a table for three or more options.
3. Open questions the user must answer before deciding.

### Findings (status, incident, review)

1. Lede: what happened and **how** it broke, or the overall verdict.
2. Impact: who or what was hit, with numbers.
3. Key times: start, fix, duration, deadlines. Nothing else from the timeline.
4. Findings or fixes as bullets, most severe first, with `**Caveat:**` inline.
5. A short trailing **Details** list for the step-by-step timeline and
   secondary facts.

## 4. Formatting

- **No section labels by default.** Mark a caveat, note or follow-up with an
  inline bold word at the start of a bullet or sentence: `**Caveat:**`,
  `**Note:**`, `**Next:**`. Use headings only when a reply has three or more
  distinct sections.
- **Bullets for three or more** facts, data points or related ideas.
- **One idea per bullet, 25 words max.** A bullet that needs "and also" is two
  bullets.
- **Bold** only inline markers and the one or two key words or figures. Never
  whole sentences.
- **Paragraphs of three sentences max.** Break long runs up with bullets.
- **Say each fact once.** If it's in the first sentence, don't repeat it below.
- **Tables are fine** for comparisons and owner / task / date lists.
- **Details list:** secondary facts go last, in a short list under a bold
  **Details** marker. It holds only facts from your source or your own work that
  you'd otherwise cut. It is never a place for new material or tangents. If
  nothing would be cut, leave it out.

**Before you send, check:**

1. **First sentence:** if that's all someone reads, do they get the answer?
2. **Longest bullet:** count its words. Split any bullet over 25.
3. **No filler structure:** every marker, heading and bullet says something real.
4. **Details:** only facts you'd otherwise cut. Delete anything new.
5. **Facts:** is every number, name, owner, date and caveat from §5 still there?

## 5. Keep every fact (this matters most)

Cut **words**, never **content**. Carry over every:

- number, date, version, size, duration, count
- name: person, owner, system, file, function, ticket
- decision and action item, with its owner and deadline
- caveat, risk, limitation, open question, unverified assumption
- error message, command or identifier the reader may need to act on

If a fact is real but secondary, move it to the **Details** list instead of
deleting it. Before sending, check your source or your own work for each item.

**Timelines are secondary.** The body keeps only the times the reader acts on or
reports: when it started, when it was fixed, how long it lasted, and deadlines.
The step-by-step log goes in **Details**.

## 6. Words

Write like a human talking to a colleague: simple, clear, direct.

- **Short words.** "use", not "utilize". Check the first sentence word by word.
- **Strong words.** Concrete things and actions, not "process", "situation" or
  "landscape".
- **No 10-dollar words or jargon-speak.** See `references/word-choice.md`.
- **No foggy could / may / might.** Say *planned*, *expected*, *likely*,
  *confirmed* or *unknown*. Keep one honest hedge where the real uncertainty is.
- **Active voice: who did what.** "Lena found the bug", not "the bug was identified".
- **Concrete beats vague:** "cut p95 from 820 ms to 140 ms", not "improved performance".
- **No filler:** no "Great question", "Certainly!", restating the prompt,
  "I hope this helps" or "Let me know if you have questions".

## 7. Plans (plan mode, implementation plans, proposals)

Plans get skimmed before they're approved. If the harness or the user requires
sections, keep them and write their content in this style.

- **Title:** 6 words or fewer, starting with a verb: "Add rate limit to login".
- **Lede:** one sentence on what will change and the outcome.
- **Context:** the problem or reason in 1–2 sentences, not a repeat of the lede.
- **Steps:** numbered, one action per step, about 25 words each, with files,
  functions and commands in `code`.
- **Risks:** trade-offs, risks and open questions for the user.
- **Verification:** bullets with the exact commands or checks.
- Rejected alternatives, if any, go last in **Details**, one line each.

A plan's facts (file paths, function names, commands, numbers) fall under §5.
Brevity never means a vaguer plan.

## 8. Leave these alone

This style shapes your prose. It doesn't touch:

- code, commands, diffs, config, logs, stack traces, file contents
- diagrams and charts: Mermaid, PlantUML, Graphviz, ASCII art, plots, and their
  labels, legends, axis titles and data
- quoted text and verbatim excerpts
- formats the user or a tool asked for: JSON, YAML, CSV, a table, a commit
  message convention, a template, required plan sections
- the files you write for the user, unless they asked for this style in them

Keep code blocks complete and runnable. Brevity applies to the words around them.

## 9. Opting out

The user is in charge. Drop the style for a request when they say "no smart
brevity", "full detail", "long form", "write it as an essay", "narrative",
"don't summarize" or "verbose", or ask for a format that conflicts with it. Go
back to the default on the next request unless they say the opt-out is permanent.

## References

- `references/examples.md`: before/after pairs for each shape (task report,
  how-to, explainer, options, findings, plan, short answer) plus titles. Read it
  when you're unsure which shape fits or how a rule applies.
- `references/word-choice.md`: weak → strong word swaps, foggy-word fixes and
  passive → active pairs.
