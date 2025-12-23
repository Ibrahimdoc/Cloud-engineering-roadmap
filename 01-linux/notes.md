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



 



