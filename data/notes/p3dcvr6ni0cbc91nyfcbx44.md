
A control group (cgroup) is a Linux kernel feature that limits, accounts for, and **isolates the resource usage** (CPU, memory, disk I/O, network, and so on) of a collection of processes.

Cgroups provide the following features:

- Resource limits – You can configure a cgroup to limit how much of a particular resource (memory or CPU, for example) **a process can use**.
- Prioritization – You can control how much of a resource (CPU, disk, or network) a process can use compared to processes in another cgroup when there is resource contention.
- Accounting – Resource limits are monitored and reported at the cgroup level.
- Control – You can change the status (frozen, stopped, or restarted) of all processes in a cgroup with a single command.

# how to configure

The following command creates a v1 cgroup (you can tell by pathname format) called foo and sets the memory limit for it to 50,000,000 bytes (50 MB).

```
root # mkdir -p /sys/fs/cgroup/memory/foo
root # echo 50000000 > /sys/fs/cgroup/memory/foo/memory.limit_in_bytes
```

I start test.sh in the background and its PID is reported as 2428. The script produces its output and then I assign the process to the cgroup by piping its PID into the cgroup file /sys/fs/cgroup/memory/foo/cgroup.procs.
```
root # ./test.sh &
[1] 2428
root # cgroup testing tool
root # echo 2428 > /sys/fs/cgroup/memory/foo/cgroup.procs
```