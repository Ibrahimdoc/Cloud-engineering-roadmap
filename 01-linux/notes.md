.Day -0 - Git setup complete.
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

- A process is a running instance of a command.
- Foreground processes block the shell; background processes do not.
- ps shows a snapshot of running processes.
- top shows live CPU and memory pressure.
- servers run processes unateended; user sessions are not required.

 


cloud analogy:

chmod = permissions

chown = ownership

IAM = distributed Linux permissions



Key notes: 
cp makes the destination file identical to the source; if the destination already exists, its contents are overwritten without warning.
Mistakes usually come from command structure or wrong assumptions, not the system itself. 





