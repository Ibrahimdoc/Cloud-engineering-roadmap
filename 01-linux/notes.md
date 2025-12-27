
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
