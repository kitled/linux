# `stress-ng` (stress next generation)

https://github.com/ColinIanKing/stress-ng

> `stress-ng` will stress test a computer system in various selectable ways. It was designed to exercise various physical subsystems of a computer as well as the various operating system kernel interfaces.


> [!Caution]
>
> Running stress-ng with root privileges can make stressors unkillable in low memory situations, so use with caution.  
> The tool can be used to test system performance changes, but it’s not intended as a scientifically accurate benchmarking metric.  
> Stress-ng can be used to test system resilience, but it may potentially break or lock up a machine, especially when running with root privileges or aggressive settings.




## Overview

Stress-ng is a stress testing tool designed to exercise various physical subsystems of a computer and operating system kernel interfaces. It’s an upstream project, and its primary goal is to measure a system’s capability to maintain efficiency under unfavorable conditions.

### Features

- Wide range of stress mechanisms (known as “stressors”) to load and stress various kernel interfaces
- Supports various operating systems, including Linux
- Can run stressors sequentially or in parallel
- Offers options for adjusting priority, timeout, and metrics reporting
- Includes a job file feature for specifying complex stress test scenarios

### Options

- `--sequential N`: run all stressors for a given device class
- `--class CLASS`: specify a device CLASS, for instance `cpu`
- `--cpu`: specifies the number of CPU stressors to run
- `--io`: specifies the number of I/O stressors to run
- `--vm`: specifies the number of virtual memory stressors to run
- `--vm-bytes`: sets the available memory for virtual memory stressors
- `--timeout`: sets the duration for each stressor
- `--metrics-brief`: shows brief metrics at the end of the run
- `--job`: runs stressors using a job file

### Examples

Stress CPU (all cores) for 1 minute, using matrix computations, showing result metrics.

```sh
stress-ng --matrix -1 --tz -t 60 --metrics-brief -v --thermalstat 6
```

> https://askubuntu.com/a/937274
>
> As the author of stress-ng, I'd personally recommend it as it has a wide range of stress tests that can be used to exercise the CPU in many different ways.
> 
> There are many ways to exercise the CPU, with `stress-ng` one can tell it to run all the stressors that can stress a CPU using:
> 
> ```sh
> stress-ng --class cpu --sequential 0 -t 1m -v
> ```
> 
> ..and to check the temperatures from the thermal zone information, one can use:
> 
> ```sh
> stress-ng --class cpu --sequential 4 -t 60 -v --tz
> ```
> 
> The above will run the cpu class of stressors one by one (sequentially) for 60 seconds with verbose mode enabled and with thermal zone stats on 4 CPUs.
> 
> Or one can run specific stressors, such as:
> 
> ```sh
> stress-ng --matrix 0 -t 2m
> ```
> 
> ..this will run the matrix stressor for 2 minutes on all the CPUs (0 = all CPUs).
> 
> The manual with `stress-ng` details all the options. There are over 170 stressors to play with. To see all the stressors, use:
> 
> ```sh
> stress-ng --help | less
> ```


hottest temp: `--vecwide`


<!--

Run in a Docker image; parallel stressors with 4 CPU, 2 I/O, and 1 VM stressor, using 1GB of virtual memory; for 60 seconds: 

```sh
docker run -it --rm alexeiled/stress-ng --cpu 4 --io 2 --vm 1 --vm-bytes 1G --timeout 60s --metrics-brief
```

-->




