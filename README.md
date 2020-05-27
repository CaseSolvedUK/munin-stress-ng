# munin-stress-ng
Munin plugin to provide throughput statistics for cpu memory and disk, written in bash

On load testing a web server running in a cloud VPS on shared hardware, I noticed huge variation in latency but couldn't find any real cause, so wrote this to check the VPS throughput.

The idea is:
 - Run stress-ng to exercise the cpu, memory and disk to 100% for a short period. Output sequential measurements to a log over an extended period of time
 - Every munin collection period, calculate the minimum, average, maximum and standard deviation of the measurements, display on a graph, and delete the log

Developed and tested on Ubuntu 18.04 only

## Dependencies
 - munin
 - bash
 - stress-ng
 - stat
 - date
 - awk
 - lshw

## How to use

1. Clone the repo

2. Link the `stress-ng` file to your plugins, for example:

`ln -s /path/to/stress-ng /etc/munin/plugins/stress_ng`

3. Configure the plugin to run as root:

`nano /etc/munin/plugin-conf.d/munin-node`

    [stress_ng]
    user root

4. `service munin-node restart`

5. Start the stress-ng process: `./stress-ng start`

6. Check the process status: `./stress-ng status`

The plugin uses the /tmp folder to store three files with munin-stress-ng prefix: .yml, .log and .pid

### To stop stress testing

`./stress-ng stop`

### To remove the plugin

1. Unlink the plugin: `rm /etc/munin/plugins/stress_ng`

2. `service munin-node restart`

Once munin stops reading data, the stress-ng task will terminate
