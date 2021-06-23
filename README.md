# CSGO-1000Hz-Kernel

## Tested on

- Ubuntu 18.04 on AWS EC2 c5n.large (Intel Platinum 8124M @ 3.0Ghz)


## Steps to compile custom 1000Hz kernel

```
sudo apt-get update && sudo apt-get upgrade -y
```

```
sudo apt-get install kernel-package libncurses-dev wget bzip2 make build-essential flex bingo libssldev
```

```
mkdir /root/kernel/
cd /root/kernel/
```

Check the current kernel version
```
uname -r
```

Get the equivalent latest minor version from kernel.org
```
wget https://cdn.kernel.org/pub/linux/kernel/v5.x/linux-5.4.127.tar.xz
```

```
tar xfv linux-5.4.127.tar.xz
cd linux-5.4.127
```

```
make localmodconfig
```

```
make menuconfig
```

General setup -->
Timers subsystem -->

Timer tick handling (Full dynticks system (tickless)) -->
[*] Full dynticks system on all CPUs by default
[*] Detect full-system idle state for full dynticks system
[*] Old Idle dynticks config
[*] High Resolution Timer Support
-*- Enable the block layer --> IO Schedulers -->

<*> Deadline I/O scheduler
<*> CFQ I/O scheduler
[*] CFQ Group Scheduling support
Default I/O scheduler (Deadline) -->
Processor type and features -->

Processor family (Generic-x86-64 für AMD, Core 2/newer Xeon für Inetl)
Preemption Model (No Forced Preemption (Server)) -->
Timer frequency (100 HZ) -->
Default I/O scheduler (Deadline) -->
Wer möglichst viel auf einer Maschine laufen lassen möchte wählt bei Preemtion Model No Forced Preemption (Server) aus. Wer den genausten Server für z.B. wenige 128 Tick Warserver haben möchte, der nimmt Preemptible Kernel (Low-Latency Desktop). Das Gleiche gilt für Timer frequency. Je genauer der Kernel sein soll, desto mehr HZ sollten eingestellt werden. Große CSS Public Server werden sich hier über den geringeren Overhead von 100HZ freuen, 128 Tick CS:GO Warserver eher über die 1000HZ.

Power management and ACPI options -->
CPU Frequency scaling -->

[] CPU Frequency scaling
-*- Networking support -->
Networking options -->

[*] Network packet filtering framework (Netfilter)
[*] QoS and/or fair queueing

```
make-kpkg clean && make-kpkg -j4 --initrd kernel_image kernel_headers
```

```
dpkg -i linux-headers-*.deb
dpkg -i linux-image-*.deb
```

```
reboot
```

```
uname -a
```
