# This branch is halted, and the reason matters more than the reports

**Ideas 03, 05 and 06 returned byte-identical results in all five metrics**:
494 transmissions, 3,963 receptions, 38.0% reach, 6,386 collisions, 527,183 ms
of airtime. Idea 04 shared the transmissions and the delivered set and differed
only in receptions.

Three different firmware builds cannot produce the same five numbers. The
builds are verifiably distinct: different checksums, both roles present in each
version directory, and each one carries a different diff against the control.

So the harness stopped applying the arm's firmware at some point during the
series, and every arm from idea 03 onward has been running the same build
against the control. The deltas on those pages are not measurements of the
ideas they name.

## What is and is not affected

| report | status |
|---|---|
| idea 01, cancel a queued relay | distinct numbers, and its Fife result agrees with its mechanism. Probably sound, not yet re-verified. |
| idea 02, retransmit delay cannot be zero | distinct numbers. Probably sound, not yet re-verified. |
| idea 03, cycle-aware loop detection | **suspect**: its numbers are the ones 05 and 06 duplicated. |
| idea 04, density-aware backoff | **suspect** |
| ideas 05 to 08 | **not written**, because the data behind them is known bad |

The control arm is *not* the problem. It produced 573 transmissions and 4,830
receptions in every sweep, which is exactly the reproducibility check working:
it is what made the duplicate arm values visible instead of plausible.

## What happens next

A discriminating run is queued: study-03 against study-05 directly, no control.
They are different binaries. If they return the same numbers, the harness is
confirmed as not applying arm firmware, and the cause gets fixed before any of
this is rerun.

Nothing here is published until that is settled. The four reports on this branch
stay so the working is visible, with this file next to them.
