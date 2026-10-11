# Linux Process Monitoring

## Process Inspection

- for process inspection we use `ps` and `ps aux` command.`ps` command is used for report infromation about currently running 
  process.
- `ps aux` command provide comprehensive view of all processes , including those are not attached to a terminal and those owned by
   others user as well.
- `a` : shows all the process for all users, not just the current user.
- `u` : displayed a user oriented format with detailed infromation such as CPU , Memory Usages , start time and the command that 
       started the process.
- `x` : includes all the processes that are not attached to a terminal, such as background deomons and system services.

## When you ran `ps aux` , the output typically includes the following columns:

- `USER` : The owner of the process.

- `PID`: Process ID, used to Indentify and manage process.

- `%CPU`: Percentage of CPU time used by the Process.

- `%MEM` : Percentage of Physical memory used.

- `VSZ`: Virtual Memory size of the process in kilobytes.

- `RSS`: Resident Set Size , the actual physical memory used in kilobytes.

- `TTY`: Terminal associated with process;a? indicate no terminal.

- `STAT`: Process State (eg- running, sleeping, zombie etc)

- `START`: Time when the process started.

- `TIME` : Total CPU time consumed by Process.

- `COMMAND`: The command that started the process.


## Live CPU Monitoring 

- `top`: A live system monitor that refresh every 3 seconds and sort by CPU Usages. It provides a continuously update
        view of running processes.

## Memory Monitoring

- `free -h` : Coomand is used to Memory monitoring . `-h` is stands for human readable format.
- `total`: Represents the total RAM available to the Linux system.
- `used`: RAM currenty used ecluding cache.
- `free`: Unsused RAM
- `available`: Estimated memory which used by apps without significant swapping.
- `swap`: Disk-backed space that Linux can use as part of memory management when needed.
- `Swap total`: Total configured swap space.
- `Swap used`: Amount of swap space currently in use.
- Swap is generally slower than RAM and is not a direct replacement for physical memory.
- `swap`: swap space currently consumed. Its as disc backed storage space which can be used for Linux managment memory. Its not
         a replacement of RAM and gnerally slow as comapred to RAM.
## System Uptime and Load Average

 - `uptime`: It provides a quick overview of system's operational status. It display how long the system has been running , the 
             current time, the no of logged in users and the system load average over the past 1, 5 and 15 minutes.

 - The Output includes:
  - `Current Time`: The time when the command executed.
  - `Uptime`: The duration the system has been running.
  - `Number of users`: The count currently logged in users.
  - `Load Average`: The system average load over the last 1,5 and 15 minutes.

### Understanding Load Average

- Load average indicates the average number of runnable and uninterruptible-sleep tasks over time.
- The first value represents the 1-minute load average.
- The second value represents the 5-minute load average.
- The third value represents the 15-minute load average.
- Load average is not CPU utilization percentage.
- We compare load average with available logical CPUs to assess potential CPU contention.
- High load average does not always indicate high CPU usage because tasks waiting for I/O can also contribute to system load.


### Practical Example

- CPU cores: 4
- Load average: 9.0, 5.0, 1.0
- My observation: Load recently increased if we see the matrics 1 minute interval load average is 9.0 , 5 minute interval load average
  is 5.0 and 15 minute load average is 1.0
- Possible production impact: if Apllication response is slow , use `top` command to check CPU Usage, using `ps aux` command to
  to identify the process and if application latency incread then compare load average and CPU idle.


## CPU Core Count

- `nproc`: Displays the number of processing units available to the current process.
- In my WSL Ubuntu environment, `nproc` returned 8, indicating 8 available logical CPUs at the time of observation.
- A physical CPU is the processor chip installed in a computer or server.
- A physical core is an execution core inside a physical CPU.
- A logical CPU is a processing unit visible to the operating system for scheduling tasks.
- Physical cores and logical CPUs are not always the same.
- SMT (Simultaneous Multithreading) allows a physical core to expose multiple logical CPUs.
- For example, 4 physical cores with 2 hardware threads per core can provide 8 logical CPUs.
- Multiple logical CPUs sharing a physical core do not guarantee the same performance as separate physical cores.
- Comparing load average with available logical CPUs helps identify potential CPU contention when the workload is primarily CPU-bound.


    

## Production Troubleshooting — INC-001

### Problem
- Application response time increased from 1 second to 10 second.

### Observations
- Logical CPUs: 4
- Load average: 7.8
- CPU idle: 3%
- Available RAM: 5.2 GiB
- My conclusion: We can see Load average is high as compare to 4 Logical CPU and CPU Idle 3% indicates that CPU is mostly busy but
  Available RAM indicate that Memory availablity is looking good. We need to inspect more to check the process and which process is
  utilizing the CPU most.

### Investigation
- `top`: I will check the CPU usage of running process.
- `ps aux`: I will identify which process have high CPU utlilization.

### Safe Action
- I will not immediately use `kill -9` because if there are 50 request are serving from that process , it will kill the process and
  those request will be incomplete.
  
