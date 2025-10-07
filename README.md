# CMPG325-NETWORKS
# Part I — Network Topologies Design & Simulation (60%)

## What to build
1. **Five basic topologies** in Packet Tracer: **Bus, Mesh, Star, Ring, Extended Star**  
2. **One hybrid topology** that integrates elements above, **IPv4 & IPv6**, **VLAN segmentation**, **HTTP/DNS/DHCP server**, and **basic security**.
3. Provide **documentation** (README, IP tables, screenshots) and **configuration notes**.

> Server choice: **HTTP** (with DNS).

---

## 0) Preparation (VS Code + Packet Tracer)
- Create this repo folder on your PC. Open it in **VS Code**.
- Open Packet Tracer and set **Preferences → Interface → Always show device names**.
- Keep the files in `part1/configs` and copy/paste to your routers/switches.

---

## 1) Five basic topologies (quick builds)
Each folder contains a `.ptk` diagram (rendered on GitHub). Recreate them in Packet Tracer with 3–5 devices each and straight‑through copper links.

### 1.1 Bus
- 1 switch acts as a shared medium; connect PC1—PC3 in a line.
- Assign IPs in 192.168.1.10 and test pings.

### 1.2 Mesh
- Use three routers in a triangle; run static routes or RIP for simplicity.
- Prove any single link failure still preserves connectivity.

### 1.3 Star
- 1 central switch with 3–4 PCs as spokes; single broadcast domain.

### 1.4 Ring
- 4 routers in a ring; configure static routes with two available paths.

### 1.5 Extended Star
- 1 core switch uplinks to two access switches; PCs hang off each access switch.

Document a **short paragraph** per topology describing benefits/trade‑offs and include a screenshot of successful pings.

---

## 2) Hybrid Campus + Branches (full build)
Use the addressing/VLAN plan from `docs/ip_tables.csv` and `docs/vlan_plan.csv`.

### 2.1 Devices & links
- R-CORE ↔ S-CORE (802.1Q trunk)
- VLAN10 (Students) PCs on S-CORE access ports
- VLAN20 (Staff) PCs on S-CORE access ports
- VLAN30 (Servers) for SRV1 (HTTP + DNS)
- Serial links: R-CORE ↔ R-BR1, R-CORE ↔ R-BR2

### 2.2 Switch (S-CORE)
Paste `part1/configs/S-CORE.cfg`.

### 2.3 Core Router (R-CORE) — Router on a Stick
Paste `part1/configs/R-CORE.cfg`.

### 2.4 Branch routers
Paste `part1/configs/R-BR1.cfg` and `part1/configs/R-BR2.cfg`.

### 2.5 Server (SRV1)
Follow `part1/configs/Server-HTTP.txt` and upload `part1/hybrid/website/index.html`.

### 2.6 Security hardening (basic)
- `no ip http server` on routers (unless required).
- `banner motd`, disable unused ports on S-CORE, set `login` & `service password-encryption`.

### 2.7 Tests to capture as screenshots
Replicate the images in `assets/expected_screens/`:
- IPv4 ping to **10.10.30.10**
- IPv6 ping to **2001:DB8:30::10**
- `show vlan brief` on S-CORE
- `show ip route` on R-CORE
- `tracert 8.8.8.8` from a PC (via default route)

Place your screenshots in `assets/my_screens/` when done.

---

## 3) Evidence & GitHub
- Commit all configs, CSV tables, and screenshots.
- In the README, link to your test screenshots and include a short reflection (what worked, problems, fixes).
