# How this series was run, and how the arms were verified

Eight protocol ideas, each on its own branch of a local MeshCore checkout, each
run against an unmodified control on the same network with the same seeds.

## The network

The 311 node Scotland and Ireland import, built from live CoreScope with the
transport regions the real nodes hold. Four originators, spread rather than
adjacent so they contend with the mesh rather than with each other. Ninety
seconds of simulated time, two seeds per arm.

## The control

Every sweep runs a control arm built from unmodified MeshCore at the commit
`repeater-v1.17.0` points at. Across the whole series it produced **573
transmissions and 4,830 receptions every time**. A difference between an arm and
the control is therefore attributable to the firmware rather than to the run.

## Why four arms return the same numbers

Ideas 03, 05, 06 and 07 return identical figures. That is not one binary running
four times, and the evidence is recorded rather than asserted: **every run
carries the SHA-256 prefix of each binary it attached**, and the four are
different files.

```
control  simple_repeater@study-control=3efa691b5e74
idea 01  simple_repeater@study-01=313396c42403
idea 02  simple_repeater@study-02=3151a4c01c1a
```

The explanation is in each report. In summary: ideas 05, 06 and 07 depend on
conditions this workload never creates — a transmit budget running low, and
request and response traffic — so their modified code paths do not execute, and
the binaries behave as the control does in every respect the measurement can
see. Idea 03's change does execute and truncates the flood.

That per-run provenance did not exist when the series was first run. Its absence
cost several hours of process listing to answer "which build produced this
number", and it was added to the simulator as a result.

## What is not established

Two seeds is a thin basis for a small delta. Anything under a few per cent in
these reports should be read as unproven.

The ±20% figure quoted elsewhere for this simulator belongs to a different
measurement, reach under contention from around eight simultaneous senders. It
is not quoted here as though it were universal; where a control's own spread is
known, that is used instead.

The MeshCore branches carrying these changes are local and are not pushed
anywhere.
