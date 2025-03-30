# 8. Linux Processes and Job Scheduling

**Exam obj:**

1. Identify CPU/memory intensive processes, adjust process priority with `renice`, and kill processes
2. Adjust process scheduling

38.  Schedule tasks using `at` and `cron` 

| Command | Use | Switches | Syntax |
| --- | --- | --- | --- |
| `ps` | View and monitor processes | `-efl` = (every)(full-format)(long listing) = shows details about every process running on the system and their niceness (NI) and priority (PRI)

`-C` = (command list) = specify a command name

`-U/-G` = list processes by user or group 

 | `ps -efl | grep top` = Check priority and niceness for the top command 

`ps -ef | grep <process>` = Check if specific process is still running |
| `top` | View and monitor processes (in real time) | “**`q`**” or **Ctrl+c** to quit
”`o`” = Re-sequence the process list 
”`f`” = Add/remove fields
”`F`” = Select field to sort on
”`h`” = Obtain help |  |
| `pgrep` | List the PID of a specific process |  | `pgrep <process_name>` |
| `nice` | Launch a process with a priority diff from the default | `-n` = specifies you want to add an integer | `nice -n <number (-20 to 19)> <process_name>` |
| `renice` | Alter a currently running process’ priority  |  | `renice -n <number (-20 to 19)> <PID of desired process>`

`sudo renice -n -5 $(pgrep top)` = Change the niceness (priority) of the ‘top’ process to -5. This command grabs the process ID through command substitution (i.e., executes the command in parenthesis and pipes it into `renice` command.) |
| `kill` | Send a signal to a process (using PID of process) |  | `kill -<signal_number> <PID>
kill -9 $(pgrep crond)` = Terminates the crond process abruptly |
| `pkill` | Send a signal to a process (using process’ name) |  | `pkill -<signal_number> <process_name>`  |
| `killall` | Terminate all processes that match a criterion (useful if multiple processes of the same name are running) |  | `sudo killall <process_name>` |
| `ps -ef | grep <process>`  | Check if specific process is still running |  |  |
| `at <date and/or time>` | Schedule a one-time execution of a command | `-f` = allows you to supply a filename |  |
| `at ls -l` | List spooled jobs |  |  |
| `at -c <jobID>` | List content of the job file | Note = jobID is found in first column when the spooled job is listed |  |
| `at -d <jobID>` | Remove a spooled job  |  |  |
| `crontab` | Schedule periodic execution of a command | `-e` = Edit
`-l` = List
`-r` = Remove
`-u` = Modify a different user’s crontable | `crontab -e` 
Add this as a line entry to crontable: `*/5 10-11 5,20 * * echo”Hello, this is a cron test.” > /tmp/hello.out` |
| `sudo vim /etc/cron.allow` | Add user to cron.allow |  |  |
| `cat /etc/crontab` | Print crontab syntax |  |  |
| `sudo ls -l /var/spool/cron` | Check for presence of a new job file |  |  |
| `sudo cat /var/log/cron` | View all activities for atd and cron services   |  |  |
| `sudo ls -l /var/spool/at` | View all submitted jobs that are currently spooled |  |  |

- For `nice`/`renice` = You need to use `sudo` any time you’re lowering a process’ priority
- Use the ***command name*** for `nice` and the ***process ID*** for `renice`

## Job Scheduling

- Allows a user to submit a command for execution at a specified time
- Can be single use or periodic
- Job scheduling is taken care of by (2) service *daemons* (background system processes):

*—> **atd*** (manages one-time job scheduling) *& **crond*** (manages periodic job scheduling)

Controlling User Access

- Allowing/denying users to perform job scheduling is done using the at.deny/at.allow & cron.deny/cron.allow files in the /etc directory
- One username is entered per line entry

Scheduler Log File

- All activities for *atd* and *crond* services are logged to the ***/var/log/cron*** file
- All submitted jobs are spooled in the ***/var/spool/at*** directory (and executed by atd daemon at the specified time)

### Using `at`

Schedule a job using `at` command:

1. Run `at` command and specify desired execution time and/or date and press Enter

`at 11:30pm 12/31/2024`

1. Type entire command at the first at> prompt and press Enter

`date &> /tmp/date.out`

1. Ctrl+d at the second at> to complete the job submission and return to shell prompt

### Using `crontab`

- Crontables (aka crontab files) are located in the */var/spool/cron* directory
- Crontable for user1 would in the: */var/spool/cron/user1* directory
1. Add user1 to /etc/cron.allow
2. Open crontable and append desired cron job 

`crontab -e` 

`*/5 10-11 5,20 * * echo ”Hello, this is a cron test.” > /tmp/hello.out`

### Syntax of User Crontables

**Exam Tip: Make sure you understand and memorize the order of the fields defined in crontables**

![image.png](image%205.png)

In crontab job scheduling, **step values** allow you to specify intervals for executing commands within a defined range. This feature enhances the flexibility of scheduling by enabling you to run jobs at regular intervals without having to list every individual time value.

### **Understanding Step Values**

Step values are indicated by a forward slash (

```
/
```

) following a range or a single value. The general syntax is:

`<min>-<max>/<step>`

- **`<min>`**: The starting point of the range.
- **`<max>`**: The endpoint of the range.
- **`<step>`**: The increment value that determines how often the job should run within that range.

### **Examples of Step Values**

1. **Every N Minutes**: If you want a job to run every 15 minutes, you can use: This means the job will execute at minute 0, 15, 30, and 45 of every hour.
    
    
    - `/15 * * * *`
2. **Every Other Hour**: To run a job every other hour, you can specify: This will execute the job at the start of every even-numbered hour (0:00, 2:00, 4:00, etc.).
    
    
    `0-23/2 * * * *`
    
3. **Specific Days of the Week**: If you want to run a job every Monday and Wednesday at 3 PM, you could use: However, if you wanted to run it every other week on those days, you would need to manage that manually, as crontab does not support step values directly for days of the week.
    
    
    `0 15 * * 1,3`
    

### **Practical Use Cases**

Using step values is particularly useful for tasks that need to be executed at regular intervals without cluttering the crontab with multiple entries. For example, if you have a script that needs to run every 10 minutes, instead of writing:

`0,10,20,30,40,50 * * * *`

You can simply write:

- `/10 * * * *`

This makes the crontab easier to read and maintain.

### Summary

Step values in crontab scheduling provide a powerful way to define execution intervals for cron jobs. By using the syntax

```
<min>-<max>/<step>
```

, you can efficiently schedule tasks to run at regular intervals, enhancing both the clarity and functionality of your cron configurations.

Review Questions: 

How do you identify CPU/memory intensive processes? 

How do you adjust process priority? 

How do you kill processes? 

How do you schedule/adjust process scheduling? 

Challenge: Submit, view, list, and erase an `at` Job