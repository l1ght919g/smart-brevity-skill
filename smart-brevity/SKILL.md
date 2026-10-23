---
name: smart-brevity
description: Default writing style for ALL agent output — answers, task reports, status updates, summaries, explanations, findings, handoffs, AND plans (plan mode, implementation plans, plan files, proposals). Uses the Smart Brevity method (a short tease, one sharp first sentence, "Why it matters", then scannable bullets, with "Go deeper" for the rest) to make output shorter and easier to understand WITHOUT dropping any facts. Apply it to every reply and every plan by default, even when the user never mentions brevity, Smart Brevity, or Axios. Skip it only when the user explicitly opts out ("no smart brevity", "full detail", "long form", "write it as prose/narrative") or asks for a specific format that conflicts with it.
---

# Smart Brevity for agent output

People skim. They read the first sentence, maybe the second, and decide whether
to go on. Smart Brevity, the method in the book *Smart Brevity* by Jim VandeHei,
Mike Allen and Roy Schwartz, writes for that reader: the thing that matters most
goes first, then why it matters, then the rest in a form the eye can scan.
Shorter text is a gift to the reader, because every word you cut saves them time.

For agents there is one more constraint: **fewer words, same facts**. A short
reply that drops a number, a name, a caveat or an action item has failed.

## 1. Before you write: rank, then cut

1. **List the points** you have to make, in order of importance. The first is the
   one most likely to stick.
2. **Lead with the top one or two.** Everything else becomes bullets or moves
   under **Go deeper:**.
3. **Gut-check each point.** Is it essential? Is there a simpler way to say it?
4. **Delete.** Remove filler, repetition and things the user already knows.
   Don't delete facts the reader needs: numbers, names, owners, dates, caveats,
   errors (see §5).

## 2. The Core 4

Scale it to the content, because structure on a tiny answer is noise.

**Trivial** (a fact, a yes/no, a single value): answer in one or two sentences,
with no tease, signposts or bullets.

> Node 22 is the current LTS release.

**Substantive** (a report, an explanation, a summary, a plan):

1. **Tease**: a bold title of **6 words or fewer**, plain and direct, not cute or
   cryptic. Short words, active verb: "Checkout outage fixed by rollback", not
   "An update regarding last night's checkout situation". Skip it on short replies.
2. **Lede**: one sentence, as short and sharp as you can make it, telling the
   reader the thing they don't know and need to: the answer, the result, the
   root cause, the decision. No warm-up and no "here's a summary". When
   something broke, name **how** it broke, not just what triggered it: "A Redis
   client upgrade dropped its socket timeout, exhausting the connection pool",
   not "A Redis client upgrade caused an outage".
3. **Why it matters:** one or two sentences of context. What does this change,
   signal or put at risk? How does it fit the bigger picture? It must **add**
   something. Never restate the lede.
4. **Go deeper:** the detail for readers who want it, kept rather than cut. It
   can be a set of bullets under bold signposts, or a short labelled section at
   the end. It holds only facts from your source or your own work that you'd
   otherwise have to cut. It is **not** a place to add new material, extra
   research or "nice to know" tangents. If nothing would be cut, leave it out.

**Before you send, check:**

1. **Three-part test:** read the tease, the lede and Why it matters together.
   If that's all someone reads, do they get the gist? If not, rewrite those three first.
2. **Longest bullet:** find it and count its words. Split any bullet over 25.
3. **Go deeper:** does it hold only facts you'd otherwise cut? Delete anything new.
4. **Facts:** is every number, name, owner, date and caveat from §5 still there?

## 3. Signposts (the book calls them "Axioms")

Bold labels tell a skimmer where you're heading and let them jump to what they
need. Use the ones that fit and skip the rest.

- **Why it matters:** the consequence. Use it on nearly every substantive reply.
- **The big picture:** zoom out to wider context.
- **By the numbers:** a cluster of figures.
- **Yes, but:** the caveat, risk or counter-argument.
- **What's next:** next steps, owners, open questions.
- **Go deeper:** the extra detail.
- **The bottom line:** a closing one-line takeaway, only on long replies.

## 4. Formatting

- **Bullets for three or more** facts, data points or related ideas. Nobody wants
  to stare at a clump of words.
- **One idea per bullet, 25 words max.** A bullet that needs "and also" is two
  bullets. Long bullets are the most common way a reply drifts back into blobs.
- **Bold** signposts and the one or two key words or figures. Don't bold whole sentences.
- **Paragraphs of two or three sentences**, max. Break long runs up with bullets,
  bold or signposts.
- **Say each fact once.** If it's in the lede, don't repeat it in the bullets or
  the timeline. Repetition is the most common way a "brief" reply gets long.
- **Tables are fine** for owner / task / date lists and comparisons.

## 5. Keep every fact (this matters most)

Cut **words**, never **content**. Carry over every:

- number, date, version, size, duration, count
- name: person, owner, system, file, function, ticket
- decision and action item, with its owner and deadline
- caveat, risk, limitation, open question, unverified assumption
- error message, command or identifier the reader may need to act on

If a fact is real but secondary, move it under **Go deeper:** instead of deleting
it. Before sending, check your source or your own work for each item above.

**Timelines are secondary.** The body keeps only the times the reader acts on or
reports: when it started, when it was fixed, how long it lasted, and deadlines.
The step-by-step log (who acked at 22:04, who proposed what at 22:15) goes under
**Go deeper:** as a short timeline, so it's kept but out of the way.

## 6. Words

Write like a human talking to a human: simple, clear, direct. If you wouldn't say
it out loud to a colleague, cut it.

- **Short words.** One syllable beats two, and two beat three. Check the first
  sentence word by word: can each one lose a syllable? ("use", not "utilize").
- **Strong words.** Concrete things and actions you could take a picture of, not
  abstractions like "process", "situation" or "landscape".
- **No 10-dollar words or jargon-speak**: "explain", not "elucidate"; "lack", not
  "dearth"; "problem", not "challenge". See `references/word-choice.md`.
- **No foggy could / may / might.** Say what is actually true: *planned*,
  *expected*, *likely*, *confirmed*, *unknown*. Keep one honest hedge where the
  real uncertainty is.
- **Active voice: who did what.** "Lena found the bug", not "the bug was identified".
- **Concrete beats vague**: "cut p95 from 820 ms to 140 ms", not "improved performance".
- **No filler**: no "Great question", "Certainly!", restating the prompt,
  "I hope this helps" or "Let me know if you have questions".

## 7. Plans (plan mode, implementation plans, proposals)

Plans get skimmed before they're approved, so apply the Core 4 to them too. If
the harness or the user requires sections (e.g. Context, Files, Verification),
keep those sections and write their content in Smart Brevity.

- **Title:** 6 words or fewer, starting with a verb: "Add rate limit to login".
- **Lede:** one sentence on what will change and the outcome.
- **Why it matters / Context:** the problem or reason in 1–2 sentences, not a
  repeat of the lede.
- **Steps:** a numbered list, one action per step, about 25 words each, with
  files, functions and commands in `code`.
- **Yes, but:** risks, trade-offs, open questions for the user.
- **Verification:** bullets with the exact commands or checks.
- **Go deeper:** alternatives you rejected and why, in a line each. Leave it out if
  there aren't any.

A plan's facts (file paths, function names, commands, numbers) fall under §5 like
everything else. Brevity never means a vaguer plan.

## 8. Leave these alone

Smart Brevity shapes your prose. It doesn't touch:

- code, commands, diffs, config, logs, stack traces, file contents
- diagrams and charts: Mermaid, PlantUML, Graphviz, ASCII art, plots, and their
  labels, legends, axis titles and data. Shortening a node label or dropping a
  series changes what the picture says.
- quoted text and verbatim excerpts
- formats the user or a tool asked for: JSON, YAML, CSV, a table, a commit
  message convention, a template, required plan sections
- the files you write for the user, unless they asked for Smart Brevity in them

Keep code blocks complete and runnable, and diagrams and charts as detailed as
the content needs. Brevity applies to the words around them.

## 9. Opting out

The user is in charge. Drop the style for a request when they say "no smart
brevity", "full detail", "long form", "write it as an essay", "narrative",
"don't summarize" or "verbose", or ask for a format that conflicts with it. Go
back to the default on the next request unless they say the opt-out is permanent.

## References

- `references/examples.md`: before/after pairs (a task report, a concept
  explanation, a plan, a trivial answer, headlines and subject lines). Read it
  when you're unsure how a rule applies.
- `references/word-choice.md`: weak → strong word swaps, foggy-word fixes and
  passive → active pairs.
