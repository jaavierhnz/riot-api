# Week 06 — DNS

## What I learned
- DNS translates names into IPs. Programs use names; the network only uses IPs.
- Linux resolves a name in this order:
  1. `/etc/hosts` — hand-written answers (name → IP). If the name is here, it wins.
  2. The DNS server — its IP is in `/etc/resolv.conf` (`nameserver` line).
     In OrbStack it is `0.250.250.200`, which forwards the question to the Mac.
- `hosts` = answers. `resolv.conf` = who to ask.
- Every machine has its own `/etc/hosts`. Everything starting with `127.` is loopback.
- `dig` and `nslookup` ask the DNS server directly and skip `/etc/hosts`.
  `ping`, `curl`, browsers and riot-api read `/etc/hosts` first.
- Reading a `dig` line: `google.com. 374 IN A 142.251.24.100`
  - `374` = TTL: seconds the answer can be cached
  - `A` = IPv4, `AAAA` = IPv6
  - One name can have several IPs (google.com returned 6)
- `Query time: 1 msec` means the answer came from cache (first time: 55 ms).
- DNS uses port 53 (`SERVER: 0.250.250.200#53`).

## What I broke
- Added `127.0.0.1 google.com` to `/etc/hosts`:
  `ping` went to 127.0.0.1, but `dig` still returned Google's real IPs.
- Removed the line and `ping` went back to Google.
- Challenge: did the same with `riotgames.com` (real IP 23.53.3.142, TTL 377).
  My first edit didn't work and I fixed it myself.

## Debugging rule
- Wrong IP for a name → name problem → check `/etc/hosts`, then `dig`.
- `ping` and `dig` disagree → it's almost always `/etc/hosts`.
- No IP reachable at all (`Network is unreachable`) → route problem → `ip route`.

## Quiz
- Start-of-week review (weeks 1-5): 4 right, 4 to reinforce
  (apt update + upgrade, octal permissions, private IPs, Network is unreachable).
- End-of-week quiz: 3/6. Still mixing up `/etc/hosts` and `/etc/resolv.conf`,
  and name problems vs route problems.

## Commands
`cat /etc/hosts` · `cat /etc/resolv.conf` · `dig` · `dig +short` · `nslookup` ·
`sudo cp /etc/hosts /etc/hosts.bak`
