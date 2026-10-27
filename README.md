# Hidden Terminal Simulation with ns-3

This project investigates how packet size affects throughput and packet
delivery ratio in a hidden terminal network, with and without RTS/CTS.
The simulation is based on the ns-3 `wifi-hidden-terminal` example by
Pavel Boyko. The original copyright and GPL license notice are retained
in the source file. The code was adapted for ns-3.47 so that it can be used in the correct way for the project.

## Requirements
- ns-3.47
- A C++ compiler and the dependencies required to build ns-3
- Ubuntu or another supported environment

## Run the simulation
Copy `hidden-terminal-project.cc` into the `scratch` directory of your
ns-3.47 installation. From the ns-3.47 directory you run:

```bash
./ns3 run "scratch/hidden-terminal-project --RngRun=1"
```

Each execution runs two experiments: first with RTS/CTS disabled, then
with RTS/CTS enabled. The output includes transmitted and received
packets and IP-level throughput for each flow.

## Reproduce the experiments

The source file initially uses a 1,400-byte application packet size.
To test another size, edit this line in the source file:

```cpp
onOffHelper.SetAttribute ("PacketSize", UintegerValue (1400));
```

Replace `1400` with `200`, `500`, `1000`, or `1400`. Keep the separate
10-byte probe packet unchanged.

For each packet size, run the program five times using `RngRun` values
from 1 through 5. For example:

```bash
for run in 1 2 3 4 5; do
  ./ns3 run "scratch/hidden-terminal-project --RngRun=$run" \
    | tee "result_run${run}.txt"
done
```

Save the files under different names or in separate directories for
each packet size so that results are not overwritten. Repeat the
procedure for all four packet sizes or more if you want too.

The simulation runs for 10 seconds. Throughput is calculated over
9 seconds using FlowMonitor IP-level byte counts. Packet delivery
ratio is the total received packet count divided by the total
transmitted packet count across the two data flows.
