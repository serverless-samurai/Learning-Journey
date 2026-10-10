# Linux Process Management

## What is process?

A process is running program.


## Foreground Process

- Foreground is process of of running a job in active terminal.Lets suppose you want to run a job in forground mode then
  you ran that programm with command `sleep 300`. when you exwcute this command the will run in foreground mode. If you
  want to exit in foreground job we use `ctrl+c` to terminate the foreground job. `ctrl+c` will send `SIGINT`signal and the 
  numeric value of signal is 2.

## Background Process

- Backgrond is a process of running a job in background mode. when you excute a job command `sleep 300` with `&` then 
  the command is `sleep 300&` after excuting the command the job will run in background mode. If we want to inspect the 
  jobs in background mode we use `jobs -l` command after execution of inspect command the output will show `JID` which 
  refers to JOBID and  `PID` which refers to PROCESSID  and the job status either its running or terminated or killed.


## Linux Signals

 - **SIGTERM(15)**
  - `SIGTERM` is used for terminate a background and foreground process . It is very useful and safe while we are working in production
      environment . the command is execute when you use its numeric value `15` . `kill -15 PID`.

- **SIGKILL(9)**
 - `SIGKILL` is used for forecefully terminate the background and foreground procees.Its uses when process is stuck and 
    and we want to terminate the process.its numeric value is `9`. The command is `kill -9 PID`.
- Cannot caught or ignored.

- **SIGINT(2)**
  - `SIGINT` is a signal which is used to send to a running process to intrupt the process. `ctrl+c` normally send 
     `SIGINT` signal to foreground process.
## Production Best Practices

- Before termination of a process we need to verify the `PID` of that process becuse its a good practice to check the 
  PID . In production we use to verify the PID of intended application because PID can be reuse for multiple processes.

- we have to prefer `SIGTERM` instead of using `SIGKILL` becuse its safe and its give the opportunity to cleanup.
  If application didn't implement cleanup then process will be terminated without application level cleanup.

- Good practice is verify the logs after executing `SIGKILL` becuse there were some running request in process that will
  be incomplete or it may lost the track of those running requests.

