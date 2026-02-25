# 🐉 RDE Bus Driver System

[![Version](https://img.shields.io/badge/version-1.0.0-red?style=for-the-badge)](https://github.com/RedDragonElite/rde_busdriver/releases)
[![License](https://img.shields.io/badge/license-RDE%20Black%20Flag-black?style=for-the-badge)](LICENSE)
[![FiveM](https://img.shields.io/badge/FiveM-Compatible-blue?style=for-the-badge)](https://fivem.net)
[![ox_core](https://img.shields.io/badge/ox__core-Required-orange?style=for-the-badge)](https://github.com/overextended/ox_core)
[![Nostr](https://img.shields.io/badge/Nostr-Integrated-purple?style=for-the-badge)](https://github.com/RedDragonElite/rde_nostr_log)

**The most realistic AI-powered public transport for FiveM — 100% Free Forever**

*Built by [Red Dragon Elite](https://rd-elite.com) | Powered by Nostr | Uncensorable by Design*

[📖 Installation](#-installation) • [🚀 Quick Start](#-quick-start) • [🌐 Website](https://rd-elite.com) • [🐙 GitHub](https://github.com/RedDragonElite)

---

## 🔥 Why This Changes Everything

Most FiveM bus systems are either broken, paywalled, or a buggy mess.  
RDE Bus Driver is different:

| ❌ Other Bus Scripts | ✅ RDE Bus Driver |
|---|---|
| **Ghost buses** — every client spawns their own vehicle | **One entity** — single vehicle synced via NetID |
| **Server empty = no buses** — they just don't spawn | **Pending queue** — buses wait for players, auto-spawn on join |
| **Can't enter** — door lock status desynced | **Local state tracking** — reliable door detection on every client |
| **Naked logs** — Discord webhook rate limits, no detail | **DB + Nostr** — permanent, decentralized, unlimited |
| **Paid** | **100% Free Forever** |

---

## ✨ Features

### 🚌 Bus System
- ✅ Fully autonomous AI bus driver following real city routes
- ✅ Realistic stop behavior — arrives, opens doors, boards, departs
- ✅ **Single entity sync** via NetID — zero ghost buses across clients
- ✅ **Pending queue** — buses survive server restart / empty server
- ✅ Automatic route cycle with configurable respawn delay
- ✅ Configurable speed, driving style, stop duration, door timing
- ✅ Auto-generated bus plate per route (`BUS_xxxx`)

### 🎫 Ticket System
- ✅ Buy ticket directly at bus via ox_target interaction
- ✅ Configurable ticket validity (default 1 hour)
- ✅ Random ticket inspector with configurable chance
- ✅ Black rider fine system
- ✅ Full **ox_inventory** integration (money as item — not broken account system)

### 🗺️ Routes & Stops
- ✅ Default Beach Line route included, auto-creates on first start
- ✅ In-game admin menu to create/delete routes and stops
- ✅ Stop placement mode — walk to location, press `E` to place
- ✅ Map blips for all stops (configurable icon, short range)
- ✅ 50m minimum distance check between stops

### 📝 Logging System
- ✅ Every stop arrival/departure → `rde_bus_stop_logs`
- ✅ Every ticket sold → `rde_bus_ticket_logs` (player, CitizenID, route, price)
- ✅ Every fine issued → `rde_bus_fine_logs`
- ✅ Daily stats auto-saved → `rde_bus_stats`
- ✅ Admin stats dashboard with recent activity

### 📡 Nostr Integration *(Optional)*
- ✅ **Bus arrivals & departures** posted to Nostr network
- ✅ **Ticket purchases** logged with player info & route
- ✅ **Black rider fines** logged permanently
- ✅ **Admin actions** — stop created/deleted, route created/deleted, bus spawned
- ✅ Zero-crash design — fails silently if `rde_nostr_log` not running
- ✅ Full tag system for searchability on Nostr clients

### 👥 NPC Passengers *(Beta)*
- ✅ Random peds waiting at stops with proper ground Z detection
- ✅ Randomized idle scenarios (phone, smoking, impatient)
- ✅ Auto-cleanup beyond render distance

---

## 🛠️ Dependencies

| Resource | Required | Link |
|---|---|---|
| `ox_core` | ✅ Yes | [overextended/ox_core](https://github.com/overextended/ox_core) |
| `ox_lib` | ✅ Yes | [overextended/ox_lib](https://github.com/overextended/ox_lib) |
| `oxmysql` | ✅ Yes | [overextended/oxmysql](https://github.com/overextended/oxmysql) |
| `ox_inventory` | ✅ Yes | [overextended/ox_inventory](https://github.com/overextended/ox_inventory) |
| `ox_target` | ✅ Yes | [overextended/ox_target](https://github.com/overextended/ox_target) |
| `rde_nostr_log` | ⚪ Optional | [RedDragonElite/rde_nostr_log](https://github.com/RedDragonElite/rde_nostr_log) |

---

## 📦 Installation

### Step 1: Place Files

```
resources/
└── rde_busdriver/
    ├── client/
    │   ├── main.lua
    │   └── admin.lua
    ├── server/
    │   └── main.lua
    ├── locales/
    │   ├── en.lua
    │   └── de.lua
    ├── config.lua
    └── fxmanifest.lua
```

### Step 2: server.cfg

```
ensure rde_busdriver
```

If you want Nostr logging, also add:
```
ensure rde_nostr_log
ensure rde_busdriver
```

### Step 3: Database

Tables are created **automatically** on first start:

| Table | Purpose |
|---|---|
| `rde_bus_stops` | Stop locations |
| `rde_bus_routes` | Route configs with stop arrays |
| `rde_bus_stats` | Daily passenger & revenue totals |
| `rde_bus_stop_logs` | Every arrival/departure with boarding counts |
| `rde_bus_ticket_logs` | Every ticket sold |
| `rde_bus_fine_logs` | Every black rider fine |

### Step 4: Configure

```lua
-- config.lua

-- Admin permissions
Config.AdminGroups  = { admin = 1, moderator = 2 }
Config.AdminAce     = 'rde.admin'
Config.AdminSteamIDs = {
    -- 'steam:110000xxxxxxxxx'
}

-- Ticket system
Config.TicketPrice      = 50      -- ox_inventory money item count
Config.BlackRiderFine   = 250
Config.InspectorChance  = 0.15    -- 15% per stop

-- Bus behavior
Config.MaxBusSpeed     = 50.0     -- km/h
Config.StopDuration    = 15000    -- ms at each stop
Config.MaxActiveBuses  = 3
```

### Step 5: ACE Permissions *(optional)*

```
add_ace group.admin rde.admin allow
```

---

## 🚀 Quick Start

1. Install → `ensure rde_busdriver` in server.cfg
2. Start server — Beach Line spawns automatically
3. Walk to a bus stop blip on the map
4. Wait for the bus — it arrives on a timed cycle
5. Use `ox_target` on the bus → buy ticket → enter
6. Ride to next stop → exit with ox_target

**Admin Menu:** `/busadmin` or `F7`

---

## 🎮 Commands & Controls

| Command / Key | Description | Permission |
|---|---|---|
| `/busadmin` | Open admin management menu | Admin |
| `F7` | Quick-open admin menu | Admin |
| `E` | Place stop (during placement mode) | Admin |
| `X` | Cancel stop placement | Admin |

---

## 🗺️ Default Routes

**Beach Line** — auto-created on first start:

| # | Stop Name |
|---|---|
| 1 | Del Perro Pier |
| 2 | Magellan Ave / Vespucci Beach |
| 3 | New Empire Way / LSIA Terminal |
| 4 | Ginger St / Vespucci |
| 5 | Marathon Ave / Vespucci Beach |
| 6 | Puerto Del Sol |

Additional routes (`Downtown Express`, `North Loop`) are in `config.lua`, commented out — uncomment to enable.

---

## 📡 Nostr Integration

When `rde_nostr_log` is running, the following events are posted to the Nostr network with full tags:

```
🚌 Bus arrived    | Route: Beach Line | Stop: Del Perro Pier
🚌 Bus departed   | Route: Beach Line | Stop: Del Perro Pier | Boarded: 2 | Alighted: 1
🎫 Ticket sold    | Player: Shin | Stop: Del Perro Pier | Route: Beach Line | $50
🚨 Black rider    | Player: Skid123 | Fine: $250 | Stop: Vespucci Beach | Route: Beach Line
⚙️ Admin action   | Admin: Shin | Created stop: New Downtown Hub
⚙️ Admin action   | Admin: Shin | Spawned bus on Route: Beach Line
```

**Every event includes tags:**
```
event_type, resource=rde_busdriver, player/admin, citizenid, route, stop, ...
```

**Zero-crash design** — if `rde_nostr_log` is not running, the bus system works perfectly with no errors. Nostr is detected at startup and all calls are wrapped in `pcall`.

---

## 📊 Database Schema

```sql
-- Every bus stop visit
rde_bus_stop_logs (
    bus_id, route_name, stop_name,
    arrived_at, departed_at,
    passengers_boarded, passengers_alighted
)

-- Every ticket sold
rde_bus_ticket_logs (
    player_id, player_name, citizenid,
    stop_name, route_name,
    price, valid_until, purchased_at
)

-- Every fine issued
rde_bus_fine_logs (
    player_id, player_name, citizenid,
    fine_amount, stop_name, route_name, fined_at
)

-- Daily totals
rde_bus_stats (date, passengers, revenue)
```

---

## 🛣️ Roadmap

### v1.1 (Next)
- [ ] NPC passengers that actually board & exit the bus
- [ ] Bus schedule display at stops (next departure countdown)
- [ ] Multiple bus models per route

### v2.0 (Future)
- [ ] Season pass / subscription tickets
- [ ] Discord webhook as fallback (alongside Nostr)
- [ ] Admin live map showing all active buses
- [ ] Player-driven bus job (earn money as driver)

---

## 🐛 Troubleshooting

**Bus doesn't spawn when I join:**
> Server auto-spawns buses after 5s. If no players are online at startup, buses queue and spawn when the first player joins. Check console for `🚌 Queued` messages.

**Can't enter the bus:**
> You can only enter when doors are open (bus is at a stop). The enter option only appears during the boarding phase. Wait for the doors-opening notification.

**Ticket purchase says "not enough money":**
> The script uses `ox_inventory` money items, not bank accounts. Make sure you have `money` items in your inventory, not just bank balance.

**NPCs spawn underground:**
> Set `Config.MinNPCsPerStop = 0` and `Config.MaxNPCsPerStop = 0` to disable NPCs. This is still Beta.

**Nostr not logging:**
> Check that `rde_nostr_log` is started BEFORE `rde_busdriver` in your `server.cfg`. Console will show `📡 Nostr integration active` on successful detection.

---

## 🤝 Contributing

1. **Fork** the repository
2. **Branch:** `git checkout -b feature/amazing-feature`
3. **Commit:** `git commit -m 'Add amazing feature'`
4. **Push:** `git push origin feature/amazing-feature`
5. **Pull Request** — and we'll review it

**Rules:**
- ✅ Keep the RDE header intact
- ✅ Follow existing code style
- ✅ Test on a live server
- ✅ Update README for new features
- ❌ No paid features, no telemetry, no BS

---

## 📜 License

```
###################################################################################
#                                                                                 #
#      .:: RED DRAGON ELITE (RDE)  -  BLACK FLAG SOURCE LICENSE v6.66 ::.         #
#                                                                                 #
#   PROJECT:    RDE_BUSDRIVER (AI-POWERED FIVEM BUS SYSTEM, NOSTR INTEGRATED)     #
#   ARCHITECT:  .:: RDE ⧌ Shin [△ ᛋᛅᚱᛒᛅᚾᛏᛋ ᛒᛁᛏᛅ ▽] ::. | https://rd-elite.com     #
#   ORIGIN:     https://github.com/RedDragonElite                                 #
#                                                                                 #
#   WARNING: THIS CODE IS PROTECTED BY DIGITAL VOODOO AND PURE HATRED FOR LEAKERS #
#                                                                                 #
#   [ THE RULES OF THE GAME ]                                                     #
#                                                                                 #
#   1. // THE "FUCK GREED" PROTOCOL (FREE USE)                                    #
#      You are free to use, edit, and abuse this code on your server.             #
#      Learn from it. Break it. Fix it. That is the hacker way.                   #
#      Cost: 0.00€. If you paid for this, you got scammed by a rat.               #
#                                                                                 #
#   2. // THE TEBEX KILL SWITCH (COMMERCIAL SUICIDE)                              #
#      Listen closely, you parasites:                                             #
#      If I find this script on Tebex, Patreon, or in a paid "Premium Pack":      #
#      > I will DMCA your store into oblivion.                                    #
#      > I will publicly shame your community.                                    #
#      > I hope your server lag spikes to 9999ms every time you blink.            #
#      SELLING FREE WORK IS THEFT. AND I AM THE JUDGE.                            #
#                                                                                 #
#   3. // THE CREDIT OATH                                                         #
#      Keep this header. If you remove my name, you admit you have no skill.      #
#      You can add "Edited by [YourName]", but never erase the original creator.  #
#      Don't be a skid. Respect the architecture.                                 #
#                                                                                 #
#   4. // THE CURSE OF THE COPY-PASTE                                             #
#      This code uses network-synced entities and async bus logic.                #
#      If you just copy-paste without reading, it WILL break.                     #
#      Don't come crying to my DMs. RTFM or learn to code.                        #
#                                                                                 #
#   --------------------------------------------------------------------------    #
#   "We build the future on the graves of paid resources."                        #
#   "REJECT MODERN MEDIOCRITY. EMBRACE RDE SUPERIORITY."                          #
#   --------------------------------------------------------------------------    #
###################################################################################
```

**TL;DR:**
- ✅ Free forever — use, edit, learn
- ✅ Keep the header — credit where it's due
- ❌ Don't sell it — commercial use = instant DMCA
- ❌ Don't be a skid — read the code before you break it

---

## 🌐 Community & Support

### Official Links

| | |
|---|---|
| 🌍 Website | [rd-elite.com](https://rd-elite.com) |
| 🐙 GitHub | [github.com/RedDragonElite](https://github.com/RedDragonElite) |
| 📡 Nostr Bot | [rde_nostr_log](https://github.com/RedDragonElite/rde_nostr_log) |

### Creator

**Shin | Red Dragon Elite**
- Nostr: `npub1wr4e24zn6zzjqx8kvnelfvktf0pu6l2gx4gvw06zead2eqyn23sq9tsd94`
- Web: [rd-elite.com](https://rd-elite.com)

### Getting Help

1. 📖 Read this README fully
2. 🔍 Check Troubleshooting section
3. 🐛 [Open an Issue](https://github.com/RedDragonElite/rde_busdriver/issues)

**Please DO:**
- ✅ Include console errors when asking for help
- ✅ Mention your FiveM/ox_core version
- ✅ Search existing issues before creating new ones

**Please DON'T:**
- ❌ DM for basic setup questions (RTFM first!)
- ❌ Ask "is it working?" without providing logs

---

## 🏆 Credits

**Built by:** [Red Dragon Elite](https://rd-elite.com)
**Creator:** Shin

**Special Thanks:**
- [Overextended](https://github.com/overextended) — ox_core, ox_lib, ox_inventory, ox_target, oxmysql
- The FiveM community
- Everyone who believes free > paid

---

**Made with 🔥 by [Red Dragon Elite](https://rd-elite.com)**

*REJECT MODERN MEDIOCRITY. EMBRACE RDE SUPERIORITY.*

[![Website](https://img.shields.io/badge/Website-Visit-red?style=for-the-badge&logo=google-chrome)](https://rd-elite.com)
[![GitHub](https://img.shields.io/badge/GitHub-Follow-black?style=for-the-badge&logo=github)](https://github.com/RedDragonElite)
[![Nostr](https://img.shields.io/badge/Nostr-Follow-purple?style=for-the-badge&logo=rss)](https://github.com/RedDragonElite/rde_nostr_log)
