# commands for network

### basics: bandwidth, latency, throughput
```bash

bandwidth: bits per second (Mbps)
throughput: bits per second (Mbps)
latency: total time of trip 

Bandwidth is the data transfer rate. Nothing to do with speed, think size of pipe.

Latency is the time required to perform some action or to produce some result. Latency is measured in units of time -- hours, minutes, seconds, nanoseconds or clock periods.

Throughput is the number of such actions executed or results produced per unit of time. This is measured in units of whatever is being produced (cars, motorcycles, I/O samples, memory words, iterations) per unit of time. The term "memory bandwidth" is sometimes used to specify the throughput of memory systems.

A simple example

The following manufacturing example should clarify these two concepts:

An assembly line is manufacturing cars. It takes eight hours to manufacture a car and that the factory produces one hundred and twenty cars per day.

The latency is: 8 hours.

The throughput is: 120 cars / day or 5 cars / hour.
```


### activate interfaces
```bash
ifup eth1
ifdown eth1
```

### create a static route to 192.168.123.123
1) create file /etc/sysconfig/network-scripts/route-enp0s8
```bash
192.168.123.123/24 via 192.168.56.101 dev enp0s8
```

2) restart network
```bash
service network restart
```

### view interface statistics
```bash
ip -s link
```

# nmcli

### modify an interface
```bash
nmcli con mod enp0s3 ipv4.addresses "192.168.56.101/24"
nmcli con down enp0s3
nmcli con up enp0s3
ip a
```

### take down network connections by name"
```bash
mcli con down "Wired connection 1"
```

