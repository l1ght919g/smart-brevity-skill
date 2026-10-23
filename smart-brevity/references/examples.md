# Before / after examples

## 1. Cut what the reader already knows

From the book (an email to a team's parents).

**Before:** a long note that repeats the venue, guesses about the season,
reminds parents to bring food and water "but you probably know that", and signs
off with three exclamation marks.

**After:** "The first tournament is at the SoccerPlex in Springfield. The boys
should arrive at 1 pm. Go RedDogs!"

Every fact the reader needs (what, where, when) survived. Everything they
already knew was cut.

## 2. Teases: headlines and subject lines

Six words or fewer, plain, specific, active.

| Before | After |
|---|---|
| The coronavirus variant in California is possibly more infectious and might cause more serious illness | California strain is more infectious |
| Health-care jobs will be able to keep the labor market growing even in a recession | Health-care hiring is recession-proof |
| Some follow-ups for Monday to discuss later today at meeting | Two important updates |
| Product sprint recap for us to review, some new templates to explore | Sprint recap: 7 new templates |

For agent output: "Summary of my investigation into the failing tests" becomes
"**Flaky tests: shared fixture state**".

## 3. Task report (agent finished a debugging job)

**Before** (182 words)

> Great question! I took a look into the issue you mentioned with the checkout
> tests failing intermittently. After investigating for a while, I was able to
> determine that the problem seems to have been related to the way the test
> fixtures were being set up. Specifically, it appears that the `cart_fixture`
> was sharing state between tests because it was defined with module scope
> rather than function scope, which meant that items added in one test were
> sometimes leaking into the next one depending on the order the tests ran in.
> I went ahead and changed the scope to function in `tests/conftest.py`, and
> after that I ran the whole suite 20 times in a row and all 20 runs passed. One
> thing to note is that the suite is now about 4 seconds slower (from 31s to
> 35s) because the fixture is rebuilt each time. I also noticed that
> `test_refund_flow` is marked as skipped with no reason given, which you may
> want to look into at some point. Let me know if you have any other questions!

**After** (~85 words)

> **Flaky checkout tests fixed**
>
> `cart_fixture` had module scope, so cart items leaked between tests depending on run order.
>
> **Why it matters:** the checkout failures in CI were noise, not real bugs. They should stop now.
>
> - **Fix:** fixture scope set to `function` in `tests/conftest.py`.
> - **Verified:** 20 full-suite runs in a row, all green.
>
> **Yes, but:** the suite is ~4 s slower (31 s → 35 s) because the fixture is rebuilt per test.
>
> **What's next:** `test_refund_flow` is skipped with no reason given. Worth a look.

Every fact survived: the cause, the file, the fix, 20 runs, 31 → 35 s, the
skipped test. Why it matters says something the lede doesn't.

## 4. Concept explanation

**Before:** three paragraphs of background on HTTP caching before the answer.

**After**

> An ETag is a version fingerprint the server attaches to a response, so the
> client can ask "has this changed?" instead of downloading it again.
>
> **Why it matters:** unchanged resources come back as an empty `304 Not Modified`,
> which saves bandwidth and speeds up repeat loads.
>
> **How it works:**
> - The server sends `ETag: "v42"` with the response.
> - The client stores it and later sends `If-None-Match: "v42"`.
> - Match → `304` with no body. Mismatch → `200` with the new body and a new ETag.
>
> **Yes, but:** weak ETags (`W/"v42"`) only promise semantic equivalence, so don't
> use them for byte-range requests.

## 5. Plan (plan mode)

**Before:** a "Background" essay of three paragraphs, then "Approach" in prose,
with file names buried mid-sentence and risks never stated.

**After** (with the sections the harness requires kept)

> # Cache user lookups in Redis
>
> Cache `get_user()` results in Redis for 5 minutes to take the load off Postgres.
>
> ## Context
> **Why it matters:** `get_user()` is 41% of DB queries, and peak traffic pins Postgres CPU at 95%.
>
> ## Steps
> 1. Add a `cache_get` / `cache_set` helper in `app/cache.py` (TTL 300 s).
> 2. Wrap `get_user()` in `app/users.py` with the helper; key `user:{id}`.
> 3. Invalidate the key in `update_user()` and `delete_user()`.
> 4. Add the `REDIS_URL` setting to `config.py` and `.env.example`.
>
> **Yes, but:** reads can be up to 5 min stale if a write bypasses `update_user()`. OK with that?
>
> ## Verification
> - `pytest tests/test_users.py` passes, plus a new test for invalidation.
> - Load test: Postgres CPU under 60% at peak (`k6 run load/users.js`).
>
> **Go deeper:** I rejected an in-process LRU cache because 6 pods would each hold stale copies.

## 6. Trivial question

**Q:** What's the current Node LTS?

**Before:** "Great question! Node.js, the popular JavaScript runtime, publishes
long-term support releases on a regular schedule. As of now…"

**After:** Node 22 is the current LTS release.

No tease and no signposts, because the structure would cost more than it saves.
