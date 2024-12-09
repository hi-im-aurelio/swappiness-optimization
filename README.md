# The Importance of Changing the Swappiness Value

## What is Swappiness?

**Swappiness** is a Linux kernel setting that controls the system's tendency to move data from RAM to swap space, a type of "virtual memory" on the hard drive. It is a numeric value, ranging from **0 to 100**, that determines when the system starts using swap:

- **0**: The system will use swap only when absolutely necessary.
- **100**: The system will use swap early, even when RAM is available.

By default, the **swappiness** value is set to **60**. This means that the system will start moving data to swap when 60% of the RAM is occupied.

### Why change the swappiness value?

On resource-constrained servers like yours, where RAM is a scarce resource, the default system behavior may not be ideal. When **swappiness** is set to **60**, the system may start using swap too early, which can lead to **thrashing**.

## What is Thrashing?

**Thrashing** occurs when the system starts to constantly move data between RAM and swap, a process that consumes a lot of resources and can make the system extremely slow. This happens because swap on disk is much slower than RAM, and the system spends more time swapping data between RAM and disk than actually performing useful tasks.

**Common causes of thrashing**:

- Processes or configurations that consume more memory than the system can handle right at startup.

- Excessive use of swap, resulting in a significant decrease in performance.

## How to Modify the Swappiness Value

To avoid thrashing and improve server performance, you can reduce the **swappiness** value to a lower level, such as **10** or **20**. This will cause the system to avoid excessive use of swap until the RAM is full.

### Step 1: Check the current swappiness value

To check the **swappiness** value currently set on your system, run the following command:

```bash
cat /proc/sys/vm/swappiness
```

### Step 2: Change the swappiness value temporarily

To adjust the **swappiness** value temporarily (the value will revert after reboot), run:

```bash
sudo sysctl vm.swappiness=10
```

Replace **10** with the desired value. To avoid premature use of swap, values ​​between **10** and **20** are recommended.

### Step 3: Make the change permanent

To ensure that the change is retained after the system reboots, edit the system configuration file:

1. Open the `/etc/sysctl.conf` file with a text editor:

```bash
sudo nano /etc/sysctl.conf
```

2. Add or change the line to set the desired **swappiness** value:

```bash
vm.swappiness=10
```

3. Save and close the file.

4. Apply the changes immediately:

```bash
sudo sysctl -p
```

The **swappiness** setting can have a significant impact on your server's performance, especially on systems with limited memory. Setting this value to a lower level helps prevent thrashing and improve performance, especially when dealing with applications that consume a lot of memory at first.

## Test Environment (Hardware and System)

The **thrashing** issue was identified in a resource-constrained server environment where RAM was scarce, which caused the system to start using swap too early, degrading performance. This situation occurred specifically on a virtualized server with the following hardware and system characteristics:

### **Server Characteristics (Hardware)**:

- **Processor**: Intel Xeon Processor (Cascadelake)
- **Architecture**: x86_64 (64-bit)
- **Cores**: 1 physical core (with 2 threads due to Hyper-Threading)
- **Frequency**: Not specified, but suitable for light workloads
- **Caches**:
- L1d (data): 64 KiB
- L1i (instructions): 64 KiB
- L2: 4 MiB
- L3: 16 MiB
- **RAM**: Approximately 1 GB
- **Storage**: 61 GB (with 41 GB in use and 18 GB available)

### **Operating System**:

- **Distribution Linux**: Debian
- **Kernel**: Linux
- **Virtualization**: The server is virtualized in a Hyper-V environment (Microsoft Hypervisor)

### **How to View Your Own Environment**:

You can use some commands to view the main characteristics of your system and hardware resources:

1. **Check memory usage and swappiness settings**:

```bash
free -h
```

2. **Check the number of processor cores and threads**:

```bash
lscpu
```

3. **Check swappiness settings**:

```bash
cat /proc/sys/vm/swappiness
```

4. **View disk space and usage**:

```bash
df -h
```

These commands will allow you to view key hardware, memory, and disk characteristics, helping you understand how your system is configured and whether it may be subject to **thrashing**-like issues.

### Tips:

- **Monitor server memory** using tools such as `free`, `top`, or `htop` to understand how the system is managing resources. - Also consider optimizing memory-intensive processes to ensure that the server does not quickly overload RAM.
