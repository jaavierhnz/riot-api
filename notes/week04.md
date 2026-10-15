# Week 04 — Environment variables, packages and logs

Dates: Oct 13–15, 2026 · Ubuntu 26.04 on OrbStack

## 1. Environment variables
- `NAME=value` creates a variable. No spaces around `=`: with spaces, bash treats NAME as a command.
- `$NAME` means "give me the value". It is not an address/pointer like in C++.
- Without `export`, only the current shell sees it. With `export`, child processes see it too (a child `bash`, and later my Python API).
- No variable survives closing the terminal. To make it permanent: add `export NAME=value` to `~/.bashrc`, then `source ~/.bashrc` to apply it to the current shell.
- `.bashrc` is only read when a shell starts. `source` runs its lines in the current shell (that's why `cd ~` inside it moved me).
- Real API keys never go in `.bashrc`: they will go in a `.env` file ignored by Git.

## 2. PATH
- `PATH` is a list of folders separated by `:`, searched left to right.
- `command not found` usually means "not in any PATH folder", not "not installed".
- `which ls` → `/usr/bin/ls`. The full path works even with an empty PATH.
- `No such file or directory` (instead of `command not found`) means bash went to a cached path that no longer exists → `hash -r`.

## 3. Packages (apt)
- `sudo apt update` downloads the package list (the "menu"). It installs nothing.
- `sudo apt upgrade` installs newer versions of what I already have.
- `apt search` looks in names and descriptions. No sudo needed: it only reads.
- `sudo apt install` needs sudo because it writes into root-owned folders like `/usr/bin`.
- `dpkg -L <pkg>` lists every file a package installed: programs in `/usr/bin` or `/usr/games`, data and manuals in `/usr/share`.

## 4. Logs (journalctl)
- A service has no terminal: systemd stores its output in the journal.
- `sudo journalctl -u fake-riot-api -n 10` shows the last 10 lines. `-f` follows them live.
- Each line: date · host · process[PID]: message. Lines from `systemd[1]` are written by systemd (PID 1).
- `active (running)` only means the process is alive, not that it works. Always confirm in the logs.
- `kill -9` (SIGKILL) → journal says `status=9/KILL` and `Failed`. Plain `kill` (SIGTERM) → `Deactivated successfully`. With `Restart=always`, systemd restarts it in both cases after `RestartSec`.
- `enable` creates a link in `multi-user.target.wants/` (start at boot). `disable` removes it.

## What I broke and how I fixed it
- Exported a variable inside a child bash without noticing, so the next child saw nothing. Fix: `exit`, check which shell I'm in, export in the right one.
- Emptied PATH on purpose (`PATH=`): `ls: command not found`, but `/usr/bin/ls` still worked. Fix: `exit` the child shell.
- Broke the service script (`echo` → `cho`): `command not found` every 5 s while the service still showed `active (running)`. I only found it by reading the journal. Fixed the script and restarted the service.

## Monthly quiz (Oct 15)
- Concepts OK, exact commands still weak without the cheat sheet.
- To review: `$` = value, `journalctl -u X -f`, `echo $PATH`, `Restart=always` vs `RestartSec`, `git push -u`.