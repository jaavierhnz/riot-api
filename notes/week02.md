# Week 2 — Processes & systemd

## What is a process and a PID?
A process is basically every program running on our computer, in the foreground or in the background.
PID is the ID number of each process (Process ID).

## kill vs kill -9 vs Ctrl+C
`kill PID` sends SIGTERM (15), a clean way to stop a process.
`kill -9 PID` sends SIGKILL (9), it kills it straight away. Last resort.
Ctrl+C sends SIGINT (2) and stops the process running in the terminal.
If I close the terminal, its processes stop too (SIGHUP).

## What does systemd do?
systemd is PID 1, basically the main process, and it lets us control services with systemctl.
It also restarts a service if it dies: I killed my fake-riot-api with `kill -9`
and systemd brought it back with a new PID (`Restart=always`).

## start vs enable
`start` starts a service now, and `enable` makes it start every time the machine boots.

## What I broke and how I fixed it
i ran tail without entry as a mistake and the process just got froze. i killed it in another tab with kill -9.
i formated mi mac so i had problems to recover all the repo but here im.
