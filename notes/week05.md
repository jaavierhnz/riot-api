# Week 05 — Networking: IPs, routing and subnetting

Start of Phase 2 (Networking). Dates: Oct 15–16, 2026.

## What I learned

- **Interfaces:** `ip addr` shows them. `lo` is the loopback (talks only to the same machine), `eth0` is the real door to the outside. `inet` = IPv4, `inet6` = IPv6.
- **Loopback:** `127.0.0.1` (`localhost`) means "myself". Every machine has its own, so it never works to reach another machine.
- **Network vs host:** in `192.168.139.68/24`, the first 24 bits (`192.168.139`) are the network and the last number is the host. The `/` is not a division.
- **Gateway:** the neighbor in my own network that forwards everything else. `ip route` shows it in the `default via 192.168.139.1` line. It must be in my same network.
- **DHCP** gave me my IP and my default route automatically (`proto dhcp`, `dynamic`).
- **ping / traceroute:** loopback 0.09 ms, gateway 0.16 ms, Google 45 ms. There were 12 hops to 8.8.8.8 (I guessed 4–5) and 14 to 1.1.1.1.
- **Private IPs:** `10.x`, `172.16`–`172.31`, `192.168.x`. They don't travel on the internet.
- **NAT:** the edge router swaps my private IP for its public one (mine: `180.71.27.250`, seen with `curl ifconfig.me`). It only lets in answers to requests made from inside, so nobody outside can reach my Ubuntu directly.
- **Subnetting:** a bigger number after the `/` means a smaller network; each extra bit halves it. A `/24` has 256 addresses but 254 usable (network + broadcast are reserved). Big networks give room to grow; small ones give isolation.
- **Ports:** IP = building, port = office (22 SSH, 443 HTTPS, 8000 for riot-api in dev).
- **TCP/IP layers:** application (HTTP), transport (TCP + port), internet (IP), link (eth0). Debug from the bottom up.
- **Git:** `origin` is the nickname for my GitHub repo address (`git remote -v`), not my local folder.

## What I broke on purpose

1. Changed `eth0` from `/24` to `/26`. The gateway `.1` fell outside my block (`.64–.127`), the default route disappeared, and `ping 8.8.8.8` gave `Network is unreachable`.
2. Put `/24` back, but ping still failed: the default route doesn't come back by itself. Fixed it with `sudo ip route add default via 192.168.139.1`.
3. In the challenge I deleted the default route and tested wrong commands on purpose: `ip route add default` alone gives `No such device` (Linux doesn't guess the gateway) and `default via` without an IP is incomplete.

Lesson: `Network is unreachable` → check `ip route` for a `default` line.

## Quiz and challenge

- Closing quiz: 3 right, 2 half, 1 wrong. Still to review: `127.0.0.1` vs gateway, what `origin` is, and the `172.16–31` private range.
- No-guide challenge: done (network sheet, break and fix the default route, `git remote -v`).
- Finally got `journalctl -u <service> -f` right with no hints.