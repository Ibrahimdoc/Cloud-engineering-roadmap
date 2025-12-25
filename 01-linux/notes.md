p.Day -0 - Git setup complete.
Created roadmap structure and connected to Github.

Day 1 - Beginner 

- pwd shows current location.
- ls lists contents of current directory.
- cd changes working directory.


Why this matters: 
 Linux commands always act relative to the current directory.

Day 1 - Medium 

- mkdir creates directories. 
- touch creates empty files.
- echo > file writes content to a file.
- cp copies a file's contents to a destination.
- mv moves or renames files.
- rm deletes files permanently; rm -r deletes directories recursively.
- rm - deletes directories recursively; checking pwd first prevents catastrophic mistakes.
- Linux commands fail silently if syntax is wrong.

Day 2 -  Path intuition (core concept)

- Relative paths depend entirely on pwd.
- Absolute paths do not depend on context.
- Using .. to go up and back down is valid but bad practice.
- Clear paths minimize unnecessary movement and maximize clarity.

Day 3 - Permissions

- chmod controls access, not intent.
- Files and directories interpret x differently.
- Numeric permissions encode intent precisely.
- Directories require x to be entered.
- pwd before chmod or rm is non-negotiable

Day 4 - Processes 

- Processes are running programs identified by PID.
- PS = snapshot; top =live system view.
- Foreground blocks the shell; background does not.
- jobs shows shell-local processes only.
- Kill sends signals; -9 is last resort.

Day 5 - Networking

- ip a shows interfaces + IP scope (loopback vs private IP)
- ip route shows the routing table - the OS's map of "which path to take for which destination."
- ss -tulpen shows local services listening (not internet access)
- cloud mapping:Listen(host) + 5G/NACL (network) both must allow.
- DNS turns names into IPSd (getent hosts,dig).
- ss -tulpen shows listening ports; AWS SG/NACL controls reachability.

Day 6 - Log triage (pipes, grep, tail, redirects)

- Less paginates large files; search inside logs quickly. 
- tail -n shows latest context; tail -f follows live logs.
- grep filters signal from noise; pipe chain filters.
- > overwrites, >> appends; 2> captures errors (stderr).

 
Day 7 - Packages + Services (apt + systemctl + journalctl) {nginx}

- apt = installs software onto disk: apt update (refresh index), apt install <pkg>.
- systemctl = controls background services (daemons): status/start/stop/restart.
- enabled ≠ running: "enabled" = starts on boot; "active (running)" = running now.
- nginx uses ExecStartPre "nginx -t" to test config before starting (prevents bad config start).
- Proof checks:
  - curl -I http://127.0.0.1 = HTTP response proof
  - ss -lntp | grep ':80' = listening socket proof
- Logs for a service:
  - journalctl -u nginx -n 50
  - journalctl -u nginx -p err -n 50
- Break/fix loop: stop nginx => curl fails; start nginx => curl 200 OK.
