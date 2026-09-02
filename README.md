<picture>
  <source media="(prefers-color-scheme: dark)" srcset="brand/meshbench-banner-1600x400.png">
  <source media="(prefers-color-scheme: light)" srcset="brand/meshbench-banner-1600x400-light.png">
  <img alt="MeshBench: an RF-accurate MeshCore network simulator" src="brand/meshbench-banner-1600x400-light.png">
</picture>

# MeshBench reports

Studies of how MeshCore actually behaves, run on real firmware against a
simulated radio and a simulated channel.

Published at <https://meshbench.github.io/meshbench-reports/>.

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

## Provenance

Every figure is measured output from MeshBench. The interpretation and the prose
are Claude Opus 5's.

Results are a best case throughout: no multipath, bare-earth terrain, no body
loss, no oscillator error, and no interference beyond the mesh's own traffic.
Every one of those omissions makes real links worse.

The faulty-radio builds used here come from
[meshcore-native](https://github.com/MeshBench/meshcore-native), which publishes
host builds of MeshCore alongside `-faultyirq` variants that misbehave the way
real radios do.
