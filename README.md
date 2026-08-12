# MeshBench reports

Studies of how MeshCore actually behaves, run on real firmware against a
simulated radio and a simulated channel.

Published at <https://a13xb0.github.io/meshbench-reports/>.

MeshBench compiles the real MeshCore application natively, one operating-system
process per node, and runs it against a model of an SX1262 and a model of the
air. The firmware and its radio driver are unmodified, so the decisions in these
reports are the same decisions the code makes on hardware. What is simulated is
everything the radio would have told it.

## Reports

- [Testing 1.17's listen-before-talk on a radio that lies](1-17-listen-before-talk/) —
  MeshCore 1.17 rewrote listen-before-talk to survive LoRa interrupt flags that
  get stuck. On a well-behaved radio the change is invisible, so we built a radio
  that sticks. The two versions then separated by a factor of 43.

### Eight protocol ideas

Read from MeshCore at the commit `repeater-v1.17.0` points at, each proposed
from a specific line of the firmware, pre-registered before running, and run
against an unmodified control on a 311 node import of ScotMesh and Ireland.
See [METHOD.md](METHOD.md) for how the arms were verified.

| | idea | outcome |
|---|---|---|
| 1 | [Cancelling a relay somebody else already sent](study-01-cancel-queued-relay/) | 12% fewer transmissions, nine points less reach |
| 2 | [A retransmit delay that cannot be zero](study-02-min-retransmit-delay/) | 19% more airtime for 2.5 points of reach |
| 3 | [The negative control that failed](study-03-cycle-aware-loop-detect/) | eighteen points of reach, because path hashes are one byte |
| 4 | [A backoff that knows how crowded it is](study-04-density-aware-backoff/) | right mechanism, wrong constant |
| 5 | [A duty budget the band would recognise](study-05-region-duty-budget/) | no change: the ceiling was never reached |
| 6 | [Hop limits that know how busy the node is](study-06-congestion-hop-limit/) | condition never fired on this workload |
| 7 | [A second chance for an acknowledgement](study-07-ack-redundancy/) | not exercised: nothing here sends a request |
| 8 | [Not forcing a transmit through a busy channel](study-08-no-forced-tx/) | fewer collisions, at the cost of latency |

Three of the eight are worth adopting, two on evidence gathered here and one on
reasoning the evidence could not reach. Three changed nothing measurable, and
the reasons are more useful than the numbers.

## Provenance

Every figure is measured output from MeshBench. The interpretation and the prose
are Claude Opus 5's.

Results are a best case throughout: no multipath, bare-earth terrain, no body
loss, no oscillator error, and no interference beyond the mesh's own traffic.
Every one of those omissions makes real links worse.

The faulty-radio builds used here come from
[meshcore-native](https://github.com/A13xB0/meshcore-native), which publishes
host builds of MeshCore alongside `-faultyirq` variants that misbehave the way
real radios do.
