# munin-stress-ng
Munin plugin to provide throughput statistics for cpu memory and disk, written in bash

On load testing a web server running in a cloud VPS on shared hardware, I noticed huge variation in latency but couldn't find any real cause, so wrote this to check the VPS throughput.

The idea is:
 - Run stress-ng to exercise the cpu, memory and disk to 100% for 1 second each. Output sequential measurements to a log over an extended period of time
 - Every munin collection period, calculate the minimum, average, maximum and standard deviation of the measurements, display on a graph, and delete the log

## Dependencies
 - munin
 - bash
 - stress-ng
 - stat
 - date
 - awk

## How to use
1. Clone the repo

2. Link the `stress-ng` file to your plugins, for example:

`ln -s /path/to/stress-ng /etc/munin/plugins/stress_ng`

3. Configure the plugin to run as root:

`nano /etc/munin/plugin-conf.d/munin-node`

    [stress_ng]
    user root

5. `service munin-node restart`

The plugin uses the /tmp folder to store three files with munin-stress-ng prefix: .yml, .log and .pid
