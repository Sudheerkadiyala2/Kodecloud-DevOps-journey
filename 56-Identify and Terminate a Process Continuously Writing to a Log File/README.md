# Day 56: Identify and Terminate a Process Continuously Writing to a Log File
 
**Platform:** KodeKloud DevOps 100 Days Challenge  
**Category:** Linux Process Management | Troubleshooting

---

## Challenge

A developer had created a testing program that continuously wrote data to the following log file:

```bash
/var/log/bad.log
```

The process was filling up the disk by constantly appending data to the log file.

The task was to identify the running process, terminate it, and ensure that the log file itself was **not deleted**.

### Task Requirements

- Identify the process writing to:

```bash
/var/log/bad.log
```

- Stop the process.
- Do **not** delete the log file.
- Ensure the log file size no longer increases.

---

## Objective

Investigate a Linux system to determine which process is writing to a log file, terminate that process, and verify that disk usage no longer increases.

---

## Why This Challenge Matters

One of the most common production issues in Linux systems is disk exhaustion caused by runaway log generation.

As a DevOps Engineer, it's important to:

- Monitor log growth.
- Identify offending processes.
- Stop unnecessary services.
- Preserve log files for future analysis.

This challenge simulated a real-world troubleshooting scenario frequently encountered on production servers.

---

## Commands Used

### Monitor the Log File

```bash
tail -f /var/log/bad.log
```

The log continued to grow continuously.

---

### Identify the Process Using the File

```bash
lsof /var/log/bad.log
```

Example Output:

```text
COMMAND   PID USER
badprog  1234 root
```

Alternatively:

```bash
fuser /var/log/bad.log
```

---

### Verify the Process

```bash
ps -fp 1234
```

---

### Terminate the Process

Gracefully stop:

```bash
kill 1234
```

If necessary:

```bash
kill -9 1234
```

---

### Verify Log Growth Has Stopped

```bash
tail -f /var/log/bad.log
```

or

```bash
watch ls -lh /var/log/bad.log
```

The file size should remain unchanged.

---

### Run Validation Script

```bash
/home/admin/agent/check.sh
```

The validation script confirms that the log file is no longer growing.

---

## Verification

### Check Running Process

```bash
ps -ef | grep bad
```

The offending process should no longer appear.

---

### Verify File Exists

```bash
ls -l /var/log/bad.log
```

Expected:

```text
-rw-r--r-- ...
```

The file should still exist.

---

### Verify File Size

```bash
watch ls -lh /var/log/bad.log
```

Expected:

```text
Size remains constant.
```

---

## What I Learned

This challenge demonstrated how Linux allows administrators to identify which process is currently using a file.

Instead of deleting logs, the correct approach is to stop the process responsible for generating unnecessary data.

I also learned how tools like `lsof` and `fuser` simplify process troubleshooting.

---

## Key Concepts

### Log Files

Log files record application and system events.

Example:

```bash
/var/log/bad.log
```

Excessive logging can:

- Fill disk space.
- Slow applications.
- Cause service failures.

---

### lsof (List Open Files)

Displays processes currently using files.

Example:

```bash
lsof /var/log/bad.log
```

Useful for:

- Finding file locks
- Identifying active processes
- Troubleshooting disk usage

---

### fuser

Shows which process is accessing a file.

Example:

```bash
fuser /var/log/bad.log
```

Can also terminate the process directly:

```bash
fuser -k /var/log/bad.log
```

---

### kill Command

Terminates running processes.

Graceful termination:

```bash
kill PID
```

Force termination:

```bash
kill -9 PID
```

---

### ps Command

Displays process information.

Example:

```bash
ps -fp PID
```

Useful for confirming:

- Process name
- Owner
- Command
- PID

---

## Visual Workflow

```text
Growing Log File
        |
        v

tail -f /var/log/bad.log
        |
        v

Identify Process
(lsof / fuser)
        |
        v

Find PID
        |
        v

Terminate Process
(kill PID)
        |
        v

Verify Log Stops Growing
        |
        v

Run Validation Script
```

---

## Useful Commands

Monitor log:

```bash
tail -f /var/log/bad.log
```

Find process:

```bash
lsof /var/log/bad.log
```

Alternative:

```bash
fuser /var/log/bad.log
```

View process:

```bash
ps -fp <PID>
```

Terminate process:

```bash
kill <PID>
```

Force terminate:

```bash
kill -9 <PID>
```

Run validation:

```bash
/home/admin/agent/check.sh
```

---

## Real-World Relevance

System administrators and DevOps engineers frequently troubleshoot issues such as:

- Log files filling disks
- Runaway applications
- High disk usage
- Zombie processes
- Resource leaks

Knowing how to identify and stop problematic processes without deleting important log files is a critical production support skill.

---

## Key Takeaways

- Use `lsof` or `fuser` to identify processes accessing files.
- Terminate unnecessary processes instead of deleting logs.
- Preserve log files for auditing and troubleshooting.
- Verify that the issue is resolved after stopping the process.
- Linux provides powerful tools for diagnosing process-related issues.

---

## Final Thoughts

This challenge provided practical experience in Linux process management and troubleshooting.

By identifying the process responsible for continuously writing to a log file and terminating it without deleting the log itself, I reinforced an essential system administration skill used in real-world production environments.

Understanding how processes interact with files is fundamental for maintaining healthy Linux systems and preventing resource exhaustion.
