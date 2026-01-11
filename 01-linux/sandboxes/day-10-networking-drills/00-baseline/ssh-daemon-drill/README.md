# SSH Daemon Drill (Day 10)

## Goal
Prove you can diagnose "service not listening" vs other causes.

## Symptom
SSH fails and port 22 is not listening.

## Proof commands
- ss: `sudo ss -lntp | grep ':22' || echo "not listening"`
- service: `sudo systemctl status ssh ssh.socket --no-pager | head -n 25`

## Break (WSL note: stop both)
sudo systemctl stop ssh
sudo systemctl stop ssh.socket

## Expected proof (broken)
sudo ss -lntp | grep ':22' || echo "port 22 not listening (expected)"

## Restore
sudo systemctl start ssh.socket
sudo systemctl start ssh

## Expected proof (fixed)
sudo ss -lntp | grep ':22'
