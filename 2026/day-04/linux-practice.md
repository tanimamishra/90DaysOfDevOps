# Day 04 – Linux Practice: Processes and Services

Date: 27/1/2026
System: ubuntu

## Process Checks
### Command 1: top snapshot

```bash
top -n 1 | head -15
```

#### OUTPUT:
```
top - 18:37:37 up 40 min,  1 user,  load average: 0.00, 0.00, 0.00
Tasks:  28 total,   1 running,  27 sleeping,   0 stopped,   0 zombie
%Cpu(s):  0.0 us,  0.0 sy,  0.0 ni,100.0 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
MiB Mem :   7805.2 total,   7329.1 free,    351.3 used,    124.8 buff/cache
MiB Swap:   2048.0 total,   2048.0 free,      0.0 used.   7308.4 avail Mem

    PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND                                      
      1 root      20   0  165904  10624   8064 S   0.0   0.1   0:00.46 systemd
      2 root      20   0    3120   1920   1920 S   0.0   0.0   0:00.00 init-systemd(Ub
      6 root      20   0    3136   1792   1792 S   0.0   0.0   0:00.00 init
     60 root      19  -1   64220  17972  17204 S   0.0   0.2   0:00.19 systemd-journal
     86 root      20   0   22848   5760   4608 S   0.0   0.1   0:00.67 systemd-udevd
     89 systemd+  20   0   25672  12856   8832 S   0.0   0.2   0:00.13 systemd-resolve
     90 systemd+  20   0   89364   7040   6272 S   0.0   0.1   0:00.14 systemd-timesyn
    168 root      20   0    4308   2432   2304 S   0.0   0.0   0:00.01 cron
```

### Command 2: List running processes
bash
ps aux | head -10

#### OUTPUT:
PID   PPID  PGID  WINPID  TTY   UID   STIME   COMMAND
322   248   322   28864   cons0 197609 00:33:03 /usr/bin/ps
323   248   322   3412    cons0 197609 00:33:03 /usr/bin/bash
248   1     248   22664   cons0 197609 00:29:53 /usr/bin/bash

## Service Checks

### Command 3: List active services

```bash
systemctl list-units --type=service --no-pager | head -15
```

#### OUTPUT:
```
UNIT                               LOAD   ACTIVE SUB     DESCRIPTION
  apport.service                     loaded active exited  LSB: automatic crash report generation
  console-getty.service              loaded active running Console Getty
  console-setup.service              loaded active exited  Set console font and keymap
  cron.service                       loaded active running Regular background program processing daemon
  dbus.service                       loaded active running D-Bus System Message Bus
  getty@tty1.service                 loaded active running Getty on tty1
  keyboard-setup.service             loaded active exited  Set the console keyboard layout
  kmod-static-nodes.service          loaded active exited  Create List of Static Device Nodes
  networkd-dispatcher.service        loaded active running Dispatcher daemon for systemd-networkd
  plymouth-quit-wait.service         loaded active exited  Hold until boot process finishes up
  plymouth-quit.service              loaded active exited  Terminate Plymouth Boot Screen
  plymouth-read-write.service        loaded active exited  Tell Plymouth To Write Out Runtime Data
  rsyslog.service                    loaded active running System Logging Service
  setvtrgb.service                   loaded active exited  Set console scheme
```
### Command 4: Inspect cron service

```bash
systemctl status cron
```

#### OUTPUT:
```
cron.service - Regular background program processing daemon
     Loaded: loaded (/lib/systemd/system/cron.service; enabled; vendor preset: enabled)
     Active: active (running) since Tue 2026-03-03 17:18:02 IST; 1h 26min ago
       Docs: man:cron(8)
   Main PID: 168 (cron)
      Tasks: 1 (limit: 9354)
     Memory: 408.0K
        CPU: 11ms
     CGroup: /system.slice/cron.service
             └─168 /usr/sbin/cron -f -P

Mar 03 17:18:02 LAPTOP-428H7RNM systemd[1]: Started Regular background program processing daemon.
Mar 03 17:18:02 LAPTOP-428H7RNM cron[168]: (CRON) INFO (pidfile fd = 3)
Mar 03 17:18:02 LAPTOP-428H7RNM cron[168]: (CRON) INFO (Running @reboot jobs)
```
## Log Checks

### Command 1: View cron service logs

```bash
journalctl -u cron --no-pager | tail -20
```

#### OUTPUT:
```
Mar 03 17:16:44 LAPTOP-428H7RNM systemd[1]: Started Regular background program processing daemon.
Mar 03 17:16:44 LAPTOP-428H7RNM cron[144]: (CRON) INFO (pidfile fd = 3)
Mar 03 17:16:44 LAPTOP-428H7RNM cron[144]: (CRON) INFO (Running @reboot jobs)
Mar 03 17:17:01 LAPTOP-428H7RNM CRON[432]: pam_unix(cron:session): session opened for user root(uid=0) by (uid=0)   
Mar 03 17:17:01 LAPTOP-428H7RNM CRON[433]: (root) CMD (   cd / && run-parts --report /etc/cron.hourly)
Mar 03 17:17:01 LAPTOP-428H7RNM CRON[432]: pam_unix(cron:session): session closed for user root
Mar 03 17:17:03 LAPTOP-428H7RNM systemd[1]: Stopping Regular background program processing daemon...
Mar 03 17:17:03 LAPTOP-428H7RNM systemd[1]: cron.service: Deactivated successfully.
Mar 03 17:17:03 LAPTOP-428H7RNM systemd[1]: Stopped Regular background program processing daemon.
Mar 03 17:17:21 LAPTOP-428H7RNM systemd[1]: Started Regular background program processing daemon.
Mar 03 17:17:21 LAPTOP-428H7RNM cron[186]: (CRON) INFO (pidfile fd = 3)
Mar 03 17:17:21 LAPTOP-428H7RNM cron[186]: (CRON) INFO (Running @reboot jobs)
Mar 03 17:18:01 LAPTOP-428H7RNM systemd[1]: Stopping Regular background program processing daemon...
Mar 03 17:18:01 LAPTOP-428H7RNM systemd[1]: cron.service: Deactivated successfully.
Mar 03 17:18:01 LAPTOP-428H7RNM systemd[1]: Stopped Regular background program processing daemon.
Mar 03 17:18:02 LAPTOP-428H7RNM systemd[1]: Started Regular background program processing daemon.
Mar 03 17:18:02 LAPTOP-428H7RNM cron[168]: (CRON) INFO (pidfile fd = 3)
Mar 03 17:18:02 LAPTOP-428H7RNM cron[168]: (CRON) INFO (Running @reboot jobs)
```
### Command 2: Check recent system logs

```bash
journalctl -xe --no-pager | tail -20
```

#### OUTPUT:
```
Mar 03 17:59:07 LAPTOP-428H7RNM systemd-resolved[89]: Clock change detected. Flushing caches.
Mar 03 17:59:39 LAPTOP-428H7RNM systemd-resolved[89]: Clock change detected. Flushing caches.
Mar 03 18:35:51 LAPTOP-428H7RNM systemd-resolved[89]: Clock change detected. Flushing caches.
Mar 03 18:35:58 LAPTOP-428H7RNM unknown: WSL (246) ERROR: CheckConnection: connect() failed: 101
Mar 03 18:36:03 LAPTOP-428H7RNM unknown: WSL (246) ERROR: CheckConnection: getaddrinfo() failed: -3
Mar 03 18:38:59 LAPTOP-428H7RNM unknown: WSL (246) ERROR: CheckConnection: getaddrinfo() failed: -3
Mar 03 18:39:41 LAPTOP-428H7RNM systemd-resolved[89]: Clock change detected. Flushing caches.
Mar 03 18:40:13 LAPTOP-428H7RNM systemd-resolved[89]: Clock change detected. Flushing caches.
Mar 03 18:40:45 LAPTOP-428H7RNM systemd-resolved[89]: Clock change detected. Flushing caches.
Mar 03 18:41:17 LAPTOP-428H7RNM systemd-resolved[89]: Clock change detected. Flushing caches.
Mar 03 18:41:49 LAPTOP-428H7RNM systemd-resolved[89]: Clock change detected. Flushing caches.
Mar 03 18:42:36 LAPTOP-428H7RNM systemd-resolved[89]: Clock change detected. Flushing caches.
Mar 03 18:43:08 LAPTOP-428H7RNM systemd-resolved[89]: Clock change detected. Flushing caches.
Mar 03 18:43:39 LAPTOP-428H7RNM systemd-resolved[89]: Clock change detected. Flushing caches.
Mar 03 18:44:11 LAPTOP-428H7RNM systemd-resolved[89]: Clock change detected. Flushing caches.
Mar 03 18:44:43 LAPTOP-428H7RNM systemd-resolved[89]: Clock change detected. Flushing caches.
Mar 03 18:45:15 LAPTOP-428H7RNM systemd-resolved[89]: Clock change detected. Flushing caches.
Mar 03 18:45:47 LAPTOP-428H7RNM systemd-resolved[89]: Clock change detected. Flushing caches.
Mar 03 18:46:19 LAPTOP-428H7RNM systemd-resolved[89]: Clock change detected. Flushing caches.
Mar 03 18:46:51 LAPTOP-428H7RNM systemd-resolved[89]: Clock change detected. Flushing caches.
```
## Mini Troubleshooting Scenario

If a service is not running:

1. Check service status:
   ```bash
   systemctl status <service-name>
   ```

2. Check logs for errors:
   ```bash
   journalctl -u <service-name> --no-pager
   ```

3. Try restarting the service:
   ```bash
   sudo systemctl restart <service-name>
   ```

4. Verify if service is active:
   ```bash
   systemctl is-active <service-name>
   ```

These steps help identify configuration errors, permission issues, or dependency failures.