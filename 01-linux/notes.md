
Day 0 — Git setup
- Git setup complete. Created roadmap structure and connected to GitHub.

Day 1 — Beginner
- pwd shows current location.
- ls lists contents of current directory.
- cd changes working directory.
- Why this matters: Linux commands act relative to the current directory (pwd).

Day 1 — Core file ops
- mkdir creates directories.
- touch creates empty files.
- echo > file writes/overwrites content to a file.
- cp copies files.
- mv moves or renames files.
- rm deletes files; rm -r deletes directories recursively.
- Non-negotiable: verify pwd before destructive commands (rm/chmod).

Day 2 — Path intuition
- Relative paths depend entirely on pwd.
- Absolute paths do not depend on context.
- Using .. is valid, but clear paths reduce mistakes and improve clarity.

Day 3 — Permissions
- chmod controls access, not intent.
- Files vs directories interpret x differently:
  - file x = executable
  - directory x = traverse/enter
- Numeric permissions encode access precisely (r=4, w=2, x=1).
- Directories require x to be entered/traversed.

Day 4 — Processes
- Processes are running programs identified by PID.
- ps = snapshot; top = live system view.
- Foreground blocks the shell; background (&) does not.
- jobs shows shell-local background jobs.
- kill sends signals; -9 is last resort.

Day 5 — Networking
- ip a shows interfaces + IP scope (loopback vs private IP).
- ip route shows routing table (where traffic goes by destination).
- ss shows listening ports/services (listening ≠ reachable from the internet).
- DNS resolves names → IPs (getent hosts, dig).
- Cloud mapping: service must be LISTENING + SG/NACL must allow + route must exist (IGW/NAT as needed).

Day 6 — Log triage (pipes, grep, tail, redirects)
- less paginates large files; search inside logs quickly.
- tail -n shows latest context; tail -f follows live logs.
- grep filters signal from noise; pipes chain filters.
- > overwrites, >> appends; 2> captures errors (stderr).

Day 6a — Packages + Services (apt + systemctl + journalctl) [nginx lab]
- apt = installs software onto disk: apt update (refresh index), apt install <pkg>.
- systemctl = controls background services: status/start/stop/restart.
- enabled ≠ running:
  - enabled = starts on boot
  - active (running) = running now
- nginx ExecStartPre uses `nginx -t` to test config before starting.
- Proof checks:
  - curl -I http://127.0.0.1 = HTTP response proof
  - ss -lntp | grep ':80' = listening socket proof
- Logs:
  - journalctl -u nginx -n 50
  - journalctl -u nginx -p err -n 50
- Break/fix loop: stop nginx => curl fails; start nginx => curl 200 OK.

Day 7 — SSH + Keys (cloud-critical)
- SSH = how you access/manage Linux servers (EC2/VMs) remotely.
- Keypair model:
  - Private key stays on your machine (never share).
  - Public key goes on the server under the target user: ~/.ssh/authorized_keys (per-user access).
- Permissions intuition (SSH is strict):
  - Directory x = traverse/enter.
  - ~/.ssh = 700, private key = 600, authorized_keys = 600.
- Service + port checks:
  - systemctl enable --now ssh (start now + on reboot)
  - ss confirms listening on :22
- SSH triage (3 layers):
  1) Network path (SG/NACL/route allows 22)
  2) Server daemon (ssh running + listening)
  3) Auth (right user/key + key in authorized_keys + perms correct)
- Proof lab: enabled SSH server, verified :22 listening, SSH’d into localhost using key auth.

Day 8 — Disk triage (df/du)

- df -h checks filesystem usage; focus on mountpoints (especially /).
- Incident pattern: “disk full” commonly comes from /var (logs/caches/app state).
- Drill-down workflow:
  1) df -h (find full mount)
  2) sudo du -xhd1 / | sort -h (top-level hogs; -x avoids other filesystems)
  3) drill into biggest directory (e.g., /var → /var/log → /var/log/journal)
- systemd journal can consume space; verify with:
  - journalctl --disk-usage
  - (control growth with vacuum policies when appropriate)

Day 9 — Minimal scripting (evidence pack)

- Built a bash script to collect repeatable “evidence packs” (system/disk/memory/network/ports) into a timestamped folder.
- Purpose: speed + consistency in debugging (same baseline every time); attach outputs to tickets/runbooks.
- Key mechanics: shebang selects bash, chmod +x makes it runnable, redirect output to files for evidence.


Day 10 - Networking Consolidation (Operator Drills)

- Goal: build reflex to diagnose connectivity issues (no theory dumps).
- 3-layer model: Network path → Daemon/service → Auth.
- Rule: every command must produce signal → interpretation → action → proof.

Daemon layer (service not listening)
- ss -lntp | grep ':22' = proves whether SSH is listening on port 22.
- systemctl status/start/stop ssh (+ ssh.socket) = controls/inspects the SSH service.
- If :22 is NOT listening, keys don’t matter yet → fix daemon first (start ssh/ssh.socket).

Auth layer (port listening but denied)
- If :22 is listening but SSH says “Permission denied (publickey)” → auth/permissions issue.
- Meaning: sshd didn’t accept any key for that user (wrong user, missing key, wrong key, or insecure perms).
- Permissions must be strict:
  - ~/.ssh = 700
  - ~/.ssh/authorized_keys = 600
  - client private key (e.g. ~/.ssh/id_ed25519) = 600
- SSH may ignore authorized_keys if it’s writable by group/others (e.g. chmod 777) for security.

Network-path quick classifier (WSL-safe)
- Timeout (“Connection timed out”) tends to indicate network-path issues.
- Connection refused often indicates daemon/service not listening.
- Permission denied indicates auth issue (assuming port is reachable).


