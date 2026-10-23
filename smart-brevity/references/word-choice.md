# Word choice

The rule behind every row: short, concrete and spoken beats long, abstract and
written. If you wouldn't say it out loud to a colleague, swap it.

## Fancy → plain

| Instead of | Say |
|---|---|
| utilize, leverage | use |
| elucidate | explain |
| facilitate | help, run |
| commence, initiate | start |
| terminate | end, stop, kill |
| ascertain | find out, check |
| demonstrate | show |
| subsequently | then, later |
| approximately | about, ~ |
| sufficient | enough |
| numerous | many |
| in order to | to |
| at this point in time | now |
| due to the fact that | because |
| prior to | before |
| a dearth / paucity of | a lack of, few |
| challenge (meaning a problem) | problem |
| discourse | talk |
| disseminate | spread, share |
| ubiquitous | everywhere |
| obfuscate | hide |
| vicissitude | change |
| salient | key |
| conundrum | puzzle, jam |
| posit | assume |
| optimal | best |
| functionality | feature |
| remediate | fix |

## Foggy → specific

*Could*, *may* and *might* tell the reader nothing. Say what you actually know.

| Foggy | Specific |
|---|---|
| This could cause issues. | This **breaks** the login page on Safari. |
| The fix may improve performance. | The fix is **expected** to cut load time ~30%; not yet measured. |
| We might ship Friday. | Friday is the **target**; it depends on the canary passing. |
| There may be other callers. | I **didn't check** for other callers; `grep` before merging. |

## Passive → active ("who did what")

| Passive | Active |
|---|---|
| The bug was identified in the parser. | I found the bug in the parser. |
| It was decided that the launch is delayed. | Sam delayed the launch. |
| Tests are being run. | CI is running the tests. |
| Concerns have been raised about latency. | Ops flagged the latency. |

## Abstract → concrete

| Abstract | Concrete |
|---|---|
| improved performance | p95 dropped from 820 ms to 140 ms |
| some files were affected | 3 files: `a.py`, `b.py`, `c.py` |
| a significant number of users | ~12% of checkout requests |
| in the near future | by Nov 7 |
