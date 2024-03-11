# Task Scheduling

Task scheduling in Linux systems, such as Ubuntu, Redhat Linux, and Solaris, enables users to automate tasks at specific times or intervals, reducing manual intervention. Common use cases include software updates, script execution, database cleanup, and automated backups. Alerts can also be set up to notify administrators or users of specific events.

## Systemd

**Systemd** is a service in Linux systems used for starting processes and scripts at predefined times. To set up automated tasks with systemd:

### Create a Timer

Create a directory for the timer script:

```bash
z0x9n@htb[/htb]$ sudo mkdir /etc/systemd/system/mytimer.timer.d
z0x9n@htb[/htb]$ sudo vim /etc/systemd/system/mytimer.timer
```

Create the timer script, specifying when to start and activate the timer:

```txt
[Unit]
Description=My Timer

[Timer]
OnBootSec=3min
OnUnitActiveSec=1hour

[Install]
WantedBy=timers.target
```

### Create a Service

Create a service script, defining the description, script path, and activation unit:

```bash
z0x9n@htb[/htb]$ sudo vim /etc/systemd/system/mytimer.service
```

```txt
[Unit]
Description=My Service

[Service]
ExecStart=/full/path/to/my/script.sh

[Install]
WantedBy=multi-user.target
```

Reload systemd to include changes:

```bash
z0x9n@htb[/htb]$ sudo systemctl daemon-reload
```

Start the service manually and enable autostart:

```bash
z0x9n@htb[/htb]$ sudo systemctl start mytimer.service
z0x9n@htb[/htb]$ sudo systemctl enable mytimer.service
```

## Cron

**Cron** is another tool for scheduling and automating processes in Linux. It requires creating a crontab file to specify when tasks should run. The crontab structure includes minutes, hours, days of the month, months, and days of the week.

Example crontab:

```txt
# System Update
* */6 * * /path/to/update_software.sh

# Execute scripts
0 0 1 * * /path/to/scripts/run_scripts.sh

# Cleanup DB
0 0 * * 0 /path/to/scripts/clean_database.sh

# Backups
0 0 * * 7 /path/to/scripts/backup.sh
```

This crontab executes tasks like software updates, script execution, database cleanup, and backups at specific intervals. Notifications and logs can also be configured.

## Systemd vs. Cron

Systemd and Cron are both tools for task scheduling in Linux systems. The main difference lies in their configuration methods. Systemd requires creating timer and service scripts, while Cron involves setting up a crontab file to instruct the cron daemon when to run tasks.
