[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/IAASVEAZ)

# CSIT5970 Assignment-1: EC2 Measurement (2 questions, 4 marks)

### Deadline: 11:59PM, Feb, 28, Friday

---

### Name: CHEN Bingjun

### Student Id: 21106754

### Email: bchenbi@connect.ust.hk

---

## Question 1: Measure the EC2 CPU and Memory performance

1. (1 mark) Report the name of measurement tool used in your measurements (you are free to choose _any_ open source measurement software as long as it can measure CPU and memory performance). Please describe your configuration of the measurement tool, and explain why you set such a value for each parameter. Explain what the values obtained from measurement results represent (e.g., the value of your measurement result can be the execution time for a scientific computing task, a score given by the measurement tools or something else).

    > I use the Phoronix Test Suite as the measurement tool. For the CPU performance test, I use the php-zip to test the compression rate and the decompression rate of the instance to indicate how well it performs and how much work it can do. And for the memory performance test, I test the copy integer rate of the instance to indicate its memory operation speed. The result of the CPU performance test is the MIPS(Million Instructions Per Second) of the compression rate and the decompression rate. The result of the memory performance test is how many MBs of memory can it operates per second. Both two results are the more the better.

2. (1 mark) Run your measurement tool on general purpose `t2.micro`, `t2.medium`, and `c5d.large` Linux instances, respectively, and find the performance differences among these instances. Launch all the instances in the **US East (N. Virginia)** region. Does the performance of EC2 instances increase commensurate with the increase of the number of vCPUs and memory resource?

    In order to answer this question, you need to complete the following table by filling out blanks with the measurement results corresponding to each instance type.

    | Size        | CPU performance                                                                                                             | Memory performance                                       |
    | ----------- | --------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------- |
    | `t2.micro`  | Compression Rating: 3656 MIPS average with 0.40% deviation<br>Decompression Rating: 3101 MIPS average with 0.05% deviation  | Copy Integer: 10670.38 MB/s average with 0.48% deviation |
    | `t2.medium` | Compression Rating: 10258 MIPS average with 0.24% deviation<br>Decompression Rating: 6052 MIPS average with 0.18% deviation | Copy Integer: 19955.03 MB/s average with 1.28% deviation |
    | `c5d.large` | Compression Rating: 7572 MIPS average with 0.18% deviation<br>Decompression Rating: 4906 MIPS average with 0.53% deviation  | Copy Integer: 14167.17 MB/s average with 0.40% deviation |

    > Region: US East (N. Virginia). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI.
    > Since the t2.medium and c5d.large both have a more vCPU and 3GiB more memory resources compared with t2.micro, and their performance on CPU and memory are better than t2.micro, I think that it can shows that the performance of EC2 instances increase commensurate with the increase of the number of vCPUs and memory resource.

## Question 2: Measure the EC2 Network performance

1. (1 mark) The metrics of network performance include **TCP bandwidth** and **round-trip time (RTT)**. Within the same region, what network performance is experienced between instances of the same type and different types? In order to answer this question, you need to complete the following table.

    | Type                      | TCP b/w (Mbps) | RTT (ms) |
    | ------------------------- | -------------- | -------- |
    | `t3.medium` - `t3.medium` | 3230           | 0.335    |
    | `m5.large` - `m5.large`   | 4970           | 0.154    |
    | `c5n.large` - `c5n.large` | 9130           | 0.162    |
    | `t3.medium` - `c5n.large` | 2390           | 0.720    |
    | `m5.large` - `c5n.large`  | 4960           | 0.128    |
    | `m5.large` - `t3.medium`  | 2340           | 0.686    |

    > Region: US East (N. Virginia). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI. Note: Use private IP address when using iPerf within the same region. You'll need iPerf for measuring TCP bandwidth and Ping for measuring Round-Trip time.
    > From the table we can know that the TCP bandwidth and the RTT performance seems are both limited by the instance with worse network performance between the two instances that are connecting. For m5.large and c5n.large, the different between the performance of network on same type or between each other seems small. But for t3.medium, the performance of connecting to the different type is obviously worse than the performance of connecting to the same type.

2. (1 mark) What about the network performance for instances deployed in different regions? In order to answer this question, you need to complete the following table.

    | Connection                | TCP b/w (Mbps) | RTT (ms) |
    | ------------------------- | -------------- | -------- |
    | N. Virginia - Oregon      | 32.3           | 62.747   |
    | N. Virginia - N. Virginia | 4650           | 0.181    |
    | Oregon - Oregon           | 4470           | 0.179    |

    > Region: US East (N. Virginia), US West (Oregon). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI. All instances are `c5.large`. Note: Use public IP address when using iPerf within the same region.
    > The performance between different regions are much worse than the performance between a same region.
    > I think the network performance can't be consistent over time, at least for TCP it can't be consistent over time. For the time when most of the people are using the internet, the increasing packets passing in the internet will increase the loss rate. The more loss rate will cause TCP to decrease the window size, and causing the bandwidth and RTT to performances worse. While during the time when most of people do not use the internet, the window size will be greater and allow bigger bandwidth and will also have a lower RTT since there are not that much packets in the internet.
