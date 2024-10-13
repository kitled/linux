# `stress-ng` (stress next generation)

https://github.com/ColinIanKing/stress-ng

> This is the stress-ng upstream project git repository. stress-ng will stress test a computer system in various selectable ways. It was designed to exercise various physical subsystems of a computer as well as the various operating system kernel interfaces.




## Overview

Stress-ng is a stress testing tool designed to exercise various physical subsystems of a computer and operating system kernel interfaces. It’s an upstream project, and its primary goal is to measure a system’s capability to maintain efficiency under unfavorable conditions.

### Key Features

- Wide range of stress mechanisms (known as “stressors”) to load and stress various kernel interfaces
- Supports various operating systems, including Linux
- Can run stressors sequentially or in parallel
- Offers options for adjusting priority, timeout, and metrics reporting
- Includes a job file feature for specifying complex stress test scenarios

### Stress-ng Options

- `--cpu`: specifies the number of CPU stressors to run
- `--io`: specifies the number of I/O stressors to run
- `--vm`: specifies the number of virtual memory stressors to run
- `--vm-bytes`: sets the available memory for virtual memory stressors
- `--timeout`: sets the duration for each stressor
- `--metrics-brief`: shows brief metrics at the end of the run
- `--job`: runs stressors using a job file

### Example Stress Test Scenarios

Run sequential stressors with verbose output and brief metrics reporting

```sh
stress-ng --sequential --verbose --metrics-brief
```

Run parallel stressors with 4 CPU, 2 I/O, and 1 VM stressor, using 1GB of virtual memory, and running for 60 seconds: 

```sh
docker run -it --rm alexeiled/stress-ng --cpu 4 --io 2 --vm 1 --vm-bytes 1G --timeout 60s --metrics-brief
```


> [!Caution]
>
> Running stress-ng with root privileges can make stressors unkillable in low memory situations, so use with caution.  
> The tool can be used to test system performance changes between kernel versions or compiler versions, but it’s not intended as a scientifically accurate benchmarking metric.  
> Stress-ng can be used to test system resilience, but it may potentially break or lock up a machine, especially when running with root privileges or aggressive settings.


