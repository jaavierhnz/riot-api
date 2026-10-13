# Week 03 — Users, groups and permissions

## What I learned
- `id` shows my user id (uid), my main group id (gid) and all the groups I belong to.
- In `/etc/passwd` each user has a home folder and a shell (for example /bin/bash).
A service user gets `nologin`, so nobody can open a session with it.
- `-rwxr-x---` is read in blocks: type, owner, group and others.
- Linux checks in order: am I the owner? am I in the group? if not, I'm in "others". Only the first match counts.
- r = 4, w = 2, x = 1, so 750 means the owner can do everything, the group can read and execute, and others get nothing.
- `chmod` changes the permissions and `chown` changes the owner and group (only root can do it).
- `umask 022` removes write from group and others, so new files are born as 644 and new folders as 755.
- In a folder, r lets you list the names and x lets you enter it with `cd`.

## What I broke and how I fixed it
- I removed x from a folder and `cd` gave me "Permission denied", even as the owner. I fixed it with `chmod u+x`.

## Why it matters
- The riot-api key should be in a file with permissions 600 (or 640 for the service group), so only the owner 
can read it. 
