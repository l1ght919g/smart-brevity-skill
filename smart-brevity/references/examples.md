# Before / after examples

One example per shape, plus two on cutting and titles. In every "after", check
the first sentence alone: it should carry the answer.

## 1. Cut what the reader already knows

From the book (an email to a team's parents).

**Before:** a long note that repeats the venue, guesses about the season,
reminds parents to bring food and water "but you probably know that", and signs
off with three exclamation marks.

**After:** "The first tournament is at the SoccerPlex in Springfield. The boys
should arrive at 1 pm. Go RedDogs!"

Every fact the reader needs (what, where, when) survived. Everything they
already knew was cut.

## 2. Titles and subject lines

Replies don't get titles. Plans, commit subjects, emails, docs and PRs do: six
words or fewer, plain, specific, active.

| Before | After |
|---|---|
| The coronavirus variant in California is possibly more infectious and might cause more serious illness | California strain is more infectious |
| Health-care jobs will be able to keep the labor market growing even in a recession | Health-care hiring is recession-proof |
| Some follow-ups for Monday to discuss later today at meeting | Two important updates |
| Product sprint recap for us to review, some new templates to explore | Sprint recap: 7 new templates |
| Summary of my investigation into the failing tests (PR title) | Fix flaky checkout tests |

## 3. Short answer

**Q:** What's the current Node LTS?

**Before:** "Great question! Node.js, the popular JavaScript runtime, publishes
long-term support releases on a regular schedule. As of now…"

**After:** Node 22 is the current LTS release.

No structure, because it would cost more than it saves.

## 4. Task report (agent finished a debugging job)

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

**After** (~60 words)

> Fixed the flaky checkout tests: `cart_fixture` had module scope, so cart items
> leaked between tests depending on run order.
>
> - Set the fixture scope to `function` in `tests/conftest.py`.
> - Ran the full suite 20 times in a row: all green.
> - **Caveat:** the suite is ~4 s slower (31 s → 35 s), since the fixture is rebuilt per test.
> - **Next:** `test_refund_flow` is skipped with no reason given.

Every fact survived: the cause, the file, the fix, 20 runs, 31 → 35 s, the
skipped test. No "Why it matters" was needed: the result line already says it.

## 5. How-to

**Q:** How do I run Postgres locally for this project?

**Before:** a paragraph on why containers are useful, then the commands buried
mid-sentence, then a warning about the port at the very end.

**After**

> Run Postgres 16 in Docker and point the app at it. You need Docker running.
>
> 1. Start the database:
>    ```sh
>    docker run -d --name pg -e POSTGRES_PASSWORD=dev -p 5432:5432 postgres:16
>    ```
> 2. Set the URL in `.env`:
>    ```sh
>    DATABASE_URL=postgres://postgres:dev@localhost:5432/postgres
>    ```
> 3. Create the tables: `make migrate`.
>
> **Note:** if port 5432 is taken by a local Postgres, use `-p 5433:5432` and change the URL to match.

## 6. Explainer

**Before:** three paragraphs of background on HTTP caching before the answer.

**After**

> An ETag is a version fingerprint the server attaches to a response, so the
> client can ask "has this changed?" instead of downloading it again. Unchanged
> resources come back as an empty `304 Not Modified`.
>
> 1. The server sends `ETag: "v42"` with the response.
> 2. The client stores it and later sends `If-None-Match: "v42"`.
> 3. Match → `304` with no body. Mismatch → `200` with the new body and a new ETag.
>
> **Caveat:** weak ETags (`W/"v42"`) only promise semantic equivalence, so don't
> use them for byte-range requests.

## 7. Options

**Q:** Should we use SQLite, Postgres or DynamoDB for the new audit log service?

**Before:** a section per database with history and features, and the
recommendation in the last paragraph.

**After**

> Use Postgres: you already run it, and the audit queries are relational
> (filter by user, date and action).
>
> | Option | For | Against |
> |---|---|---|
> | Postgres | Already in prod, strong querying, backups in place | One more DB to size for write load |
> | SQLite | Zero ops, fast for one writer | No concurrent writers across 3 pods |
> | DynamoDB | Scales writes without tuning | New vendor, queries limited to key design |
>
> Open questions:
> - Expected write rate? Above ~2k/s, Postgres needs partitioning.
> - Retention period? It decides whether old rows move to cold storage.

## 8. Findings (incident report)

**Before:** a minute-by-minute log from 21:58 onward, with the root cause in
the eighth paragraph.

**After**

> Checkout was down for 47 minutes: the Redis client upgrade to 5.0 dropped
> the socket timeout default, so hung connections exhausted the pool.
>
> - **Impact:** ~3,100 failed checkouts; EU and US.
> - **Times:** started 21:58 UTC, fixed 22:45 UTC by rolling back to 4.6.
> - **Fix planned:** set `socket_timeout=5` explicitly before re-upgrading (Maya, Friday).
> - **Caveat:** the pool-size alert never fired; its threshold was 100%, not 80%.
>
> **Details**
> - 22:04 on-call acked the page.
> - 22:21 pool exhaustion spotted in Grafana.
> - 22:38 rollback started.

Times in the body are the ones the reader acts on. The log is kept, but last.

## 9. Plan (plan mode)

**Before:** a "Background" essay of three paragraphs, then "Approach" in prose,
with file names buried mid-sentence and risks never stated.

**After** (with the sections the harness requires kept)

> # Cache user lookups in Redis
>
> Cache `get_user()` results in Redis for 5 minutes to take the load off Postgres.
>
> ## Context
> `get_user()` is 41% of DB queries, and peak traffic pins Postgres CPU at 95%.
>
> ## Steps
> 1. Add a `cache_get` / `cache_set` helper in `app/cache.py` (TTL 300 s).
> 2. Wrap `get_user()` in `app/users.py` with the helper; key `user:{id}`.
> 3. Invalidate the key in `update_user()` and `delete_user()`.
> 4. Add the `REDIS_URL` setting to `config.py` and `.env.example`.
>
> ## Risks
> - Reads can be up to 5 min stale if a write bypasses `update_user()`. OK with that?
>
> ## Verification
> - `pytest tests/test_users.py` passes, plus a new test for invalidation.
> - Load test: Postgres CPU under 60% at peak (`k6 run load/users.js`).
>
> **Details:** I rejected an in-process LRU cache because 6 pods would each hold stale copies.
