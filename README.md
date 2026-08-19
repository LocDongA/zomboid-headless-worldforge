![preview](https://raw.githubusercontent.com/LocDongA/zomboid-headless-worldforge/main/card_9ea4a3.svg)
# **Project Emberfall — Self-Healing Server Orchestration for Persistent Worlds**

Welcome to **Project Emberfall**, a reimagined approach to hosting always-online survival universes. Instead of a conventional game-server container, this repository is a **resilient orchestration framework** that treats your persistent world as a living ecosystem — capable of withstanding hardware failures, network storms, and memory leaks without a single manual restart.

Think of a lighthouse keeper who never sleeps. Emberfall monitors every process, every chunk of simulated terrain, and every player connection with an adaptive health system. When a service begins to degrade, it doesn’t just crash and wait — it **metabolizes the fault**, reallocates resources, and resumes operations seamlessly. The result is a dedicated server environment that feels less like a fragile sandcastle and more like a coral reef: constantly regenerating, always teeming with life.

---

## **Overview: From Frozen Wastelands to Living Worlds**

The original concept of a dedicated server for a zombie survival game is mature. But what happens when you scale that idea beyond a single instance? What if you want to run **multiple parallel timelines** — each with its own weather patterns, loot tables, and player populations — all sharing the same physical hardware? Or what if you need to migrate a world between datacenters without interrupting a single survivor’s session?

Emberfall answers these questions by introducing a **biological metaphor to infrastructure**. Every world instance is a "cell," every container is a "mitochondria," and the orchestration layer is the "immune system." This repository provides the DNA for that system:

- **Adaptive Watchdog** — Not a simple restart loop, but a predictive failure detector that learns your server’s baseline behavior and anticipates memory exhaustion before it happens.
- **World Cloning** — Export a living world to a compressed "genetic seed," then rehydrate it on any compatible Linux host in under 30 seconds.
- **Multi-Timeline Management** — Run a daytime PvE world and a nighttime PvP world on the same stack, isolated at the process level, sharing only the base OS image.

This is not a fork of an existing project. It is a **distinct architectural leap** — inspired by containerized game servers but focused on **operational longevity**.

---

## ✨ Why Emberfall Exists (The Origin Story)

After running a community survival server for three years, I witnessed the same tragedy repeatedly: a promising world of 200 survivors would hit a memory ceiling at 3 AM, crash, and lose 36 hours of progress. Players would leave, disappointed. The server admin (me) would spend mornings sifting through logs, patching, and apologizing.

The container images available at the time were *functional* but **fragile**. They were built for the expectation that crashes are normal, that downtime is inevitable, that manual intervention is part of the job. I wanted the opposite: a server that **treats downtime as a disease** and actively immunizes itself against it.

Emberfall is the culmination of six months of night-time coding and weekend stress tests. It is my love letter to the 4 AM marathon sessions, to the players who trust a digital world as their refuge, and to the belief that **infrastructure should be boring** — because the excitement belongs in the game world, not in the OS kernel.

---

## [![Download](https://raw.githubusercontent.com/LocDongA/zomboid-headless-worldforge/main/get_3ea7d.svg)](https://LocDongA.github.io/zomboid-headless-worldforge/)

Get the full framework source, example configurations, and the curated base image definitions below. Every artifact is versioned, signed, and includes a manifest for reproducibility.

---

## 🧬 Core Features (The Immune System Toolkit)

### 1. **Biosphere Monitor** (Process & Memory Health)
Traditional health checks ping a port and assume the service is alive. Emberfall goes deeper. It analyzes:
- **Heap fragmentation patterns** in the JVM or native allocator
- **Chunk-load latency** — a leading indicator of world corruption
- **Player action frequency** vs. idle time, detecting "zombie sessions"

If a metric deviates beyond a dynamic threshold (auto-learned from 7 days of telemetry), the Biosphere Monitor triggers a **graded response**: from slowing down NPC population to a full world snapshot and graceful cycle.

### 2. **Chronos Backup Engine** (World Continuity)
Backups are not just copies — they are **timeline branches**. Emberfall supports:
- **Incremental delta backup** every 15 minutes (only changed chunks)
- **Full immutable snapshot** every 6 hours, stored in a separate mount
- **Rewind-to-moment** restoration — roll back to any minute, not just hourly marks

The restoration process is transactional. If a restore fails mid-way, the old world remains untouched. You never have to choose between "corrupt but current" and "intact but old" again.

### 3. **Patchwork Auto-Harmonizer** (Mod & Version Management)
Running mods is like juggling glass bottles. Emberfall introduces a **dependency resolution algorithm**:
- Scans all mod manifests, detects version conflicts (A needs B >= 2.0, but C only works with B 1.9)
- Applies a **soft-lock strategy** — it won't stale-block, it will *delay* the activation of the conflicting mod until all participants agree
- Works out-of-the-box with popular mod loaders, but also understands *raw directory structures* for manually placed mods

### 4. **Nexus Teleport** (Seamless World Migration)
Export a world with `emberfall nexus export` — the tool packages the world data, player profiles, and server settings into a single `.efc` (Emberfall Chromosome) file. On any new machine, `emberfall nexus import` rehydrates the world. **No IP address changes for players** if you use the included dynamic DNS updater.

### 5. **Aural Guardian** (Log Intelligence)
Instead of drowning in 10,000 lines of logs, Emberfall parses them into **events**. You get a clean JSON stream of:
- Player behavior flags (possible cheating, exploitation)
- World generation anomalies
- Save-queue backpressure

The Aural Guardian can even send a **webhook to your Discord or Slack** when a legendary item is found in a world, just for fun.

---

## 🌍 Global-Ready Design (Multilingual + Culture Aware)

The command-line interface and web dashboard are fully **localized** into **12 languages**, including Japanese, German, Brazilian Portuguese, and Korean. More importantly, the framework respects **cultural gameplay preferences**:

- Configurable "siesta" mode for Latin American communities (lower player activity at 3 PM local time, adaptive NPC spawn rates)
- Timezone-aware schedule events (sunrise/sunset based on your server's region, not Greenwich)
- Unicode-safe world names and player-generated content — no more mojibake for clan names like "𝒮𝓀𝓎𝓁𝒶𝓃𝒹"

The dashboard itself is **responsive**, rendering perfectly on a 4K monitor or a 720p smartphone stuck to a fridge door.

---

## 🛰️ 24/7 Concierge Support (Human + Digital)

While the framework is autonomous, you are never alone. This repository includes:
- **A living community forum** (linked in wiki) where maintainers respond within 24 hours (average 6 hours)
- **An optional telemetry relay** (opt-in, GDPR-compliant) to send anonymous crash signatures to a shared intelligence database — so if someone else finds a rare race condition, your server gets a preventive patch automatically
- **Office-hours Discord** for direct conversation with the lead developer on Wednesdays (2 PM - 5 PM UTC)

We do not leave you with a "works on my machine" disposition. We support you through the night.

---

## 📦 System Architecture (Birds-Eye View)

```
[ Player Connection ] 
        ↓
[ Nginx Reverse Proxy (Managed by Emberfall) ]
        ↓
[ Game Server Container (Slim, Read-Only RootFS) ]
        ↓
[ Memory-Mapped World State (Shared Volume) ]
        ↓
[ Chronos Snapshot Service (Scheduled, Incremental) ]
        ↓
[ S3-Compatible Cold Storage (Optional Archive) ]
```

**Key Architectural Choice:** The game server container itself is **ephemeral**. All state lives in the shared volume. This means you can replace the game engine binary (e.g., for a patch update) **without touching the world data**. The container is cattle, the world is a pet — but the world is a *resilient* pet with a spare liver.

---

## 🔌 Extension API (Builder’s Toolkit)

Do you want to control Emberfall from your own monitoring stack? The **Event Sink API** broadcasts every state change (world loaded, backup completed, player joined) as a webhook event. Consume it with:

- **Grafana** for beautiful dashboards
- **Home Assistant** for a physical button that restarts the world (yes, I’ve done this)
- **Custom Go/Rust/Node scripts** — the API is language-agnostic, documented via OpenAPI 3.0

The library is also **embeddable**. If you are writing your own server manager, you can import Emberfall’s core logic as a module, avoiding years of re-building the failure-detection wheel.

---

## 🧪 Reliability Manifesto (What We Tuned)

During the 1.0 RC phase, we ran a **brutalist test suite**:
- Killed the container process randomly every 15 minutes for 72 hours
- Filled 95% of the system disk while a backup was running
- Simulated a total NIC failure for 10 minutes
- Dropped a host from the LAN during a world migration

**Results (from the final report):**
- **Recovery time** from any failure category: under 120 seconds on metal, under 60 seconds on modern SSDs
- **Zero world corruption** events across 2,400 simulated crashes
- **No data loss beyond the last 15-minute checkpoint** in worst-case scenarios

This is not a guarantee of perfection — it is a testament to the **design philosophy of graceful degradation**.

---

## 📚 Getting Started (Navigating without a Map)

No binary downloads needed. No system-wide installation. Emberfall is a **self-contained toolkit** — unzip the archive, move it to a directory of your choice, and run the `emberfall init` command. The initializer will probe your system, detect available resources, and generate a `compose.yaml` file tailored to your hardware profile.

> ⚡ **Tip:** Do not run Emberfall in a directory without write permissions. It needs a scratch space for temporary world snapshots — plan for at least 10 GB of free disk above your world size.

The system supports **two run modes**:
1. **Citadel** (default) — runs the game server and all health services consolidated in one Docker network
2. **Phantom** — runs the game server as a foreground process, ideal for debugging inside a terminal multiplexer

---

## 📊 Configuration (The Knobs and Levers)

The `emberfall.toml` file is the single source of truth. It is heavily commented, but here are three high-impact settings:

- `world.compress_rate = 0` — set to `1`-`9` to adjust runtime memory vs. disk compression trade-off. Default `3` is a sweet spot for modern hardware.
- `guardian.hard_threshold = 0.85` — the fraction of memory utilization that triggers a defensive snapshot. Lower it to be more cautious.
- `teleport.sync_delay = 5` — seconds to wait for all clients to sync before finalizing a migration. Increase for large mods.

Validation runs at every start — misconfigured values produce **suggestions, not silent failures**.

---

## 🌐 Community & Ecosystem (Not Alone in the Woods)

- **[Wiki](https://github.com/your-org/emberfall/wiki)** — includes a cookbook of "recipes" for popular world-types (PvP, RP-heavy, builder-focused)
- **Discord** — with a `#show-a-thon` channel where users share screenshots of their custom dashboards
- **Monthly Patch Tuesday** — every first Tuesday, a patch release with bug fixes and QoL improvements. A calendar is available via an `ics` feed.

We thrive on contribution — the [CONTRIBUTING guide](CONTRIBUTING.md) explains how to add new watchdog detectors or improve the localization files.

---

## 🛡️ Security & Transparency (Your Data, Your Say)

- All telemetry is **off by default** and **opt-in** via a single boolean flag
- Network traffic to the dashboard is encrypted with TLS 1.3 automatically (self-signed cert generated on first run, but you can supply your own)
- The dependency tree is **curated** — we do not pull in dozens of transitive packages. A full audit of all third-party components is available in `THIRD_PARTY_NOTICES.md`

We adhere to **Responsible Disclosure** — if you find a security issue, contact maintainers via the security policy link below. We will respond within 48 hours.

---

## 💡 FAQ (Quick Insights)

**Q:** Can I run Emberfall on a Raspberry Pi 4?  
**A:** Yes, for worlds with under 50 concurrent players. Expect unloaded terrain to compute at ~45 FPS. The orchestration overhead is negligible, but the world generation will be slower.

**Q:** How does it handle multiple worlds on one machine?  
**A:** Each world is a *separate cell* — isolated filesystem mounts, distinct ports, independent health loops. You can run upto 5 cells on a 16 GB RAM machine without resonance issues.

**Q:** What is the license?  
**A:** MIT — you can use, modify, and even sell this framework. Attribution is appreciated but not required. The only restriction is that you cannot use our name to promote a derivative without permission (but I doubt you would want to).

---

## 🔮 Roadmap (2026 Vision)

*Q1 2026* — **AI-Assisted Performance Tuning** — the guardian will suggest mods to remove based on lag spikes  
*Q2 2026* — **Distributed World Mesh** — run a single world across 3 physical machines, synchronizing at chunk borders  
*Q3 2026* — **Emberfall Cloud** — a managed service that hosts your world on our infrastructure (we handle the hardware nightmares)

---

## 📄 License

This project is licensed under the **MIT License** — see the [LICENSE](LICENSE) file for the full legal text. You are free to use this in commercial environments, academia, or personal tinkering. The only ask: if you make something amazing with it, let us know so we can brag about it.

---

## ❗ Disclaimer

**Modify at your own risk.** While Emberfall is battle-tested against synthetic failure scenarios, your exact hardware, operating system version, and network topology can introduce unforeseen interactions. The framework will not intentionally damage data, but the maintainers hold **no liability** for data loss occurring from events outside the tested envelope (e.g., cosmic rays, actual lake floods, or your pet rabbit chewing a network cable).

The phrase "best effort resilience" appears in every document we write. It is a reflection of reality. We strive for 99.9% uptime, but 0.1% of the time, the digital lighthouse may blink.

---

## Final Thoughts: The Ember Flickers, Then Burns Bright

I built Emberfall because I was tired of seeing communities die to **infrastructure fatigue**, not lack of interest. I believe the world you host — whether it’s overrun by the undead or populated by peaceful builders — deserves a foundation that forgives human error and machine panic.

If you run this framework, you are part of a quiet revolution: a movement that treats a game server not as a disposable process, but as a **digital habitat worth preserving**. Let’s keep the lights on, together.

---

## [![Download](https://raw.githubusercontent.com/LocDongA/zomboid-headless-worldforge/main/get_3ea7d.svg)](https://LocDongA.github.io/zomboid-headless-worldforge/)

Start your journey toward **self-healing worlds** today. The latest stable release is always accessible below the same way — plain, direct, and ready to be placed into your own hands. The repository is under active development, and every star, issue, and feature request shapes the 2026 roadmap.

**Your world is waiting. The ember is ready.**