# SSH Auth Drill (Day 10)

## Goal
Prove you can diagnose auth failure when the service is listening.

## Symptom
SSH responds but denies key (Permission denied / publickey).

## Proof commands
- listening: `sudo ss -lntp | grep ':22'`
- perms: `ls -ld ~/.ssh && ls -l ~/.ssh/authorized_keys`

## Break (too-open perms on server-side auth file)
chmod 777 ~/.ssh/authorized_keys

## Test
ssh -o ConnectTimeout=3 -i ~/.ssh/id_ed25519 $USER@127.0.0.1

## Restore correct perms
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
