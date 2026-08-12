# This branch is paused, and the reason is under test

## What was seen

Three arms returned byte-identical numbers in all five metrics:

| arm | tx | rx | reach | collisions | airtime |
|---|---|---|---|---|---|
| control v1.17.0 | 573 | 4830 | 56.2% | 7293 | 540.3 s |
| idea 01 cancel a queued relay | 505 | 4407 | 47.5% | 6467 | 529.9 s |
| idea 02 retransmit delay not zero | 614 | 4833 | 58.7% | 7985 | 645.3 s |
| **idea 03 cycle-aware loop detect** | **494** | **3963** | **38.0%** | **6386** | **527.2 s** |
| idea 04 density-aware backoff | 494 | 4126 | 38.0% | 6401 | 527.2 s |
| **idea 05 region duty budget** | **494** | **3963** | **38.0%** | **6386** | **527.2 s** |
| **idea 06 congestion hop limits** | **494** | **3963** | **38.0%** | **6386** | **527.2 s** |
| idea 08 no forced transmit | 553 | 4618 | 52.3% | 7004 | 615.3 s |

## What I first concluded, and why it was wrong

My first reading was that the harness had stopped applying the arm firmware from
idea 03 onward. **Idea 08 refutes that**: it ran last, after all the suspect
arms, and produced numbers distinct from both the control and the cluster. A
harness that had stopped applying arm firmware would have given 08 the stuck
value too.

So the correct statement is narrower and less alarming: **three arms coincide
exactly and four do not**, and I do not yet know whether that is a real property
of those three changes or a fault that affects some arms and not others.

## Why coincidence is possible

All three suspect changes plausibly truncate a flood at the same depth, by
different routes: idea 03 rejects deep paths once a hash repeats, and ideas 05
and 06 both act through the transmit budget. On a fixed topology with a fixed
origin and deterministic firmware, three changes that stop relaying at the same
hop produce the same transmissions, the same receptions and the same collisions.
Identical is exactly what determinism looks like when behaviour matches.

Idea 04 supports this: it shares the transmission count and the delivered set
with idea 03 but differs in receptions, which is what a slightly different route
to the same truncation depth would do.

## The test

A discriminating run is queued: **study-03 against study-05 directly, no
control**. They are different binaries by checksum, with different diffs. If
they return the same numbers, something is wrong with how arms are applied. If
they differ, the coincidence above is real and the reports stand.

Nothing is published from this branch until that has run. The control arm is not
in question: it produced 573 and 4,830 in every one of seven sweeps, which is
what made the coincidence visible in the first place.
