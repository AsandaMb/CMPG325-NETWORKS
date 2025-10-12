#CMPG325-NETWORKS
# Part | — Network Topologies Design & Simulation (60%)

## What to build
1. Five basic topologies** in Packet Tracer: Bus, Mesh, Star, Ring, Extended Start 
2. One hybrid topology that integrates all of these elements: IPv4 & IPv6, VLAN segmentation, HTTP/DNS/DHCP server(atleast one of these) and basic security.
3. Provide **documentation** (README, IP tables, screenshots) and **configuration notes in cfg fomat**.
> Server choice: **HTTP** (with DNS).

----

## 0) Preparation using VS Code & Packet Tracer
- Create this repo folder on your PC, Open it with VS Code.
- Open Packet Tracer and set "Preferences → Interface → Always show device names.


PART 1: Five basic topologies (quick builds)
The folder named 5 topologies includes 5 `.ptk` diagram (rendered on GitHub).
Recreate them in Packet Tracer with 3–5 devices each and use straight‑through copper links.

# 1.1 Bus
- 2 switches that acts as a shared medium; connect PC1—PC4 vertical to each other  and horizontal to that connect PC2 and PC3 vertical to each other.
- Assign IPs in 192.168.1.10 and test pings.

# 1.2 Mesh
- Use 4 switces in a square and interconnect
- Prove that every device has a direct path

# 1.3 Star
- 1 central switch with 5 PCs as spokes; single broadcast domain.

# 1.4 Ring
- 6 switches in a ring; configure static routes with two available paths.

# 1.5 Extended Star
- 1 core switch uplinks to two access switches; PCs hang off each access switch.
  Include a screenshot of successful pings.

The folder includes an expected screenshots word document that includes the tested ping of every topology included

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
Paste `part1/configurations/S-CORE.cfg`.

### 2.3 Core Router (R-CORE) — Router on a Stick
Paste `part1/hybrid topology/configurations/R-CORE.cfg`.

### 2.4 Branch routers
Paste `part1/hybrid topology/configurations/R-BR1.cfg` and `part1/configs/R-BR2.cfg`.

### 2.5 Server (SRV1)
Follow `part1/hybrid topology/configurations/Server-HTTP.txt` and upload `part1/hybrid topology/website/index.html`.

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

## 3) Evidence & GitHub
- Commit all configs, CSV tables, and screenshots.
- In the README, link to your test screenshots and include a short reflection (what worked, problems, fixes).
Place your screenshots in `hybrid topology/expected images/` when done.


# Part II — Configure Default Routes to a Central Router (20%)

Goal: All branch LANs use **R-CORE** as their path to the Internet.  
R-CORE forwards unknown traffic to the **ISP** (simulated by a stub router).

## Steps
1. On **R-BR1** and **R-BR2**, configure a default route *towards R-CORE* the main router:
   ```
   R-BR1(config)# ip route 0.0.0.0 0.0.0.0 10.255.1.1
   R-BR1(config)# ipv6 route ::/0 2001:DB8:255:1::1
   R-BR2(config)# ip route 0.0.0.0 0.0.0.0 10.255.1.5
   R-BR2(config)# ipv6 route ::/0 2001:DB8:255:1::5
   ```
2. On **R-CORE**, forward to ISP:
   ```
   R-CORE(config)# ip route 0.0.0.0 0.0.0.0 10.255.1.2
   R-CORE(config)# ipv6 route ::/0 2001:DB8:255:1::2
   ```
3. Verify from a branch PC:
   - `tracert 8.8.8.8` should show R-BR → R-CORE → ISP.
   - Load `http://www.nwu.local` (proves DNS traversal towards VLAN30).

Add screenshots to `part 2/ecpected screenshots/` and reference them in your README.


