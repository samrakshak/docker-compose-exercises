If container doesn't have netstat package using `sudo apt install net-tools` to install tools that included `netstat`.

---
Using `netstat -tuap` to show the port mapping table.

**-t**: Show the tcp protocol.

**-u**: Show the udp protocol.

**-a**: Show all.

**-p**: Show PID

---

The docker storage overview:
 - volume: manage data by docker.
 - bind mount: manage data by host file system.
 - tmpfs: non-persisted on the disk.
