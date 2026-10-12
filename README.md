# Terraria server hosting: how much RAM you actually need, and getting a 24/7 world online without port forwarding

Most people searching for Terraria server hosting are trying to solve one of three problems. Their friends can only play when the host's PC is on. Steam's "Host & Play" gives everyone 500ms of rubber-banding. Or they've already tried running `TerrariaServer.exe` at home and got stuck on port 7777 and a router admin page that hasn't been updated since 2016.

The actual decision underneath is smaller than it looks: pick a server flavor, size it with roughly the right RAM, and choose between a machine you administer and a panel that does the boring parts for you. Everything below is about those three things, with real prices.

## First, decide which Terraria server you're renting

"Terraria server hosting" is three different products wearing one name, and plenty of hosts only sell you two of them.

- **Vanilla (the "Stable" build)** — the server that ships with the game. Runs `TerrariaServer.bin` (Linux) or the exe on Windows. Lowest overhead, no plugin safety net, no mods.
- **TShock** — a community server that adds accounts, permissions, groups, region protection, rollback and anti-grief tooling. This is what you want for an open community world where you don't know everyone. Plugins are `.dll` files dropped into `ServerPlugins/`, config lives in `config.json` and `sscconfig.json`.
- **tModLoader** — required for any Terraria mod. Calamity, Thorium, Fargo's: all of it runs through tModLoader, not vanilla. Mods are `.tmod` files in a `Mods/` folder, and tModLoader rewrites `enabled.json` from whatever is actually in that folder at boot.

Two practical notes that trip people up. First, TShock and tModLoader are not interchangeable — you cannot run Calamity on TShock. Second, before you buy anything, confirm the host offers all three, because a lot of plans advertise "Terraria hosting" and quietly mean vanilla plus tModLoader only.

## Self-hosting versus renting: the honest version

Self-hosting is genuinely fine in one narrow case: a two-person group that plays on the same schedule, on a PC that's already on. Beyond that, the friction compounds.

| Factor | Self-hosting at home | Rented server |
| --- | --- | --- |
| Monthly cost | Electricity and hardware wear | From about €4 / $3 a month at the cheap end |
| Setup time | 30–120 minutes (port forwarding, firewall, config files) | Minutes |
| World availability | Only while your PC is on | 24/7 |
| Lag source | Your upload bandwidth | Datacenter network |
| Restarts for memory creep | Manual — Terraria's server process accumulates memory over long sessions | Scheduled from the panel |
| Backups | Whatever you remember to copy | Automated on most plans |
| DDoS protection | None | Included |

That memory item is not a theoretical concern. Long Terraria sessions progressively slow down, and the standard fix is a scheduled daily restart — trivial to configure when the panel has a cron field, tedious when it's your desktop and you're the one who has to be awake.

The other recurring annoyance is tModLoader. When a mod conflict hits, all players get dropped and the log tells you almost nothing until you start disabling mods one by one. On a panel with file access and on-demand backups, that's a twenty-minute chore. On a home machine mid-session, it ends the evening.

## Sizing: what your group actually needs

Terraria's tick rate is largely single-thread bound, so single-core clock speed matters more than core count once you're above two cores. RAM is driven by world size, player count and mod load.

| Your setup | RAM | Notes |
| --- | --- | --- |
| Vanilla / small world, 2–4 players | 1–2 GB | The server binary is light; 2 GB is comfortable |
| Vanilla, 4–8 players, medium world | 2–4 GB | Where most friend groups land |
| tModLoader with light mods, 4–12 players | 2–4 GB | Modpack memory is the variable, not player count |
| tModLoader, Calamity + Thorium, 8+ players | 4–8 GB | Large worlds plus active event nights |
| TShock community server, 16+ players | 4–8 GB | TShock itself is cheap; the world state isn't |

A useful frame from the host side: Terraria's engine starts feeling cramped above roughly 16 players, so 32-slot plans are for events and drop-in communities rather than a normal playthrough.

Storage is the quiet one. Defaults are small — a 10 GB system disk is common on panel products — and Terraria world files plus tModLoader's downloaded mod cache and logs add up. Pick more disk than you think you need on day one; expanding later means a restart.

## The category most "best Terraria host" lists skip

Almost every comparison you'll find ranks the same six Western prepaid hosts: Shockbyte from €4.28/month for 2 GB and 4 players, Bisect Hosting and Apex from around $2.99, ScalaCube at $1.49, Supercraft from $5.99 for 8 players. These are slot-based plans — you pay per player count and RAM, and everything is managed for you.

The other option is a **game cloud**, which most guides ignore because the well-known ones are regional. You buy a container or VPS with configurable CPU, RAM, bandwidth and disk, install the game server from a one-click list, and pay monthly, daily or on a metered model. RainYun, a Chinese provider that's been running since 2018 and holds ISP, IDC and CDN licences, sells exactly this under its game cloud line, and Terraria is one of the games it installs with a single click.

It comes in two shapes:

- **VPS game cloud** — full root access, Windows or Linux, 15 open ports, snapshots, the option to attach a dedicated IPv4. You install and patch everything yourself.
- **Panel game cloud** — a container on MCSManager or RainYun's own K8s-based panel, managed entirely in a browser. One-click installs for Terraria (vanilla, TShock and tModLoader), Minecraft across roughly every server core, Palworld, Don't Starve Together, Project Zomboid and Unturned, plus MySQL, SFTP, a web file manager, backups, scheduled tasks and an editable start command.

For a Terraria group, the panel product is the interesting one. You pick the game type, the panel drops the server files in place, and you get `something.rainplay.cn:PORT` to hand your friends. If the world gets heavy, you raise RAM or switch CPU model from the console — RainYun's panel runs on a Kubernetes cluster, so a machine change is a setting rather than a migration.

👉 [See the current Terraria-ready game cloud options](https://www.rainyun.com/MzA2MTc2_)

## What it actually costs

Here's the part that differs from the slot-based hosts you may have compared. RainYun doesn't sell fixed "Vanilla 4 players" packages. It sells a configurator: you choose product shape (VPS / RainYun panel / MCSM panel), CPU model, region, billing mode, and then cores, RAM, bandwidth and disk. So there is no single canonical plan list, and any price table that claims otherwise is probably out of date.

The configuration tiers published on the game cloud page for the VPS product look like this:

| Tier | CPU / RAM | Bandwidth | Disk | From | Buy |
| --- | --- | --- | --- | --- | --- |
| Entry | 2 cores / 4 GB | 10 Mbps up, 200 Mbps down | 30 GB system + 20 GB data | ¥125/month | [Check current pricing](https://www.rainyun.com/MzA2MTc2_) |
| Advanced | 4 cores / 8 GB | 15 Mbps up, 200 Mbps down | 30 + 20 GB | ¥220/month | [View this tier](https://www.rainyun.com/MzA2MTc2_) |
| High-end | 6 cores / 12 GB | 20 Mbps up, 200 Mbps down | 30 + 20 GB | ¥360/month | [Open the plan page](https://www.rainyun.com/MzA2MTc2_) |

Two things move those numbers a lot:

1. **CPU model.** The panel offers Intel i9 14900KS (overclocked, very high single-core clock) and Xeon Platinum 8259CL (cheaper, lower clock, fine for a vanilla world). The VPS line adds i9 14900KF and 13900KF, Ryzen 9950X/7950X/5900X, Xeon Gold 6146 and E5 2666v3, with EPYC 7A23 in some regions. For Terraria, the cheap Xeon is usually the right call: the server is far lighter than a modded Minecraft instance, and the money is better spent on RAM and disk. Modded tModLoader with a big pack is the case for the 14900KS.
2. **Billing mode.** Fixed billing is classic monthly rent. Dynamic billing charges a low base rent plus a daily settlement for CPU usage above a 20% baseline — the idea being that an idle server costs almost nothing. Panel servers also support daily billing, which is settled from your account balance each day and lets you shut the instance down between sessions. Community guidance from RainYun's own forums is consistent: if your group plays more than a couple of hours a day, take fixed billing; for occasional weekend worlds, dynamic or daily usually wins.

Third-party listings for the panel product put a Platinum 8259CL 2 cores / 4 GB instance at roughly ¥25/month, 4 cores / 8 GB at roughly ¥65, and 6 cores / 12 GB at around ¥120, with i9 14900KS tiers starting around ¥90 (2C/4G) and ¥180 (4C/8G). At recent exchange rates that's about $3.50 and $9 for the two cheaper 8259CL tiers — the same order of magnitude as Shockbyte's €4.28 vanilla plan, before any discount, for hardware you configure yourself. Treat those panel figures as a starting point, not a quote: the live configurator is what decides.

Note also that the ¥125/¥220/¥360 tiers above are the VPS shape, which is the pricier one because it includes a full operating system and open ports. Terraria almost never needs that.

### Discounts and trial terms that are verifiable

- New accounts registering through a referral link and binding WeChat get a **50% off first-month coupon** — repeatedly documented in RainYun's forum threads and tutorials.
- **Annual billing is 30% off** (yearly payment = 70% of the monthly rate), and community posts describe stacking a further 20% new-purchase coupon from the points shop on top, taking an annual plan to about 56% of list.
- **One-day trial from ¥1–2**, once per product line, with pricier configurations costing more.
- **7-day no-reason refund**, up to three times per account.
- There's also a points store where a free game cloud instance occasionally appears, renewable with points earned from daily check-ins and account binding. Stock is limited and third-party guides report restocks around 20:00 China time.

The refund window matters more than usual here, because the honest way to test whether a China-based node is good enough for your group is to run the trial for a day with everyone online and look at the actual ping.

## Setting up a Terraria server on a panel

The workflow is short enough to describe completely.

1. Create an account and open the game cloud purchase page from the console.
2. Choose **server type**: RainYun panel (or MCSM panel if you prefer the open-source MCSManager interface).
3. Choose **game type**: Terraria, then the flavor — vanilla, TShock for admin tooling, or tModLoader for mods.
4. Pick **CPU model** (8259CL for vanilla, 14900KS if you're running a heavy modpack), then **cores and RAM** — 2C/4G covers a vanilla world of 4–8 players, and raising this later is a console setting rather than a rebuild.
5. Raise the **disk** above the default if you're running tModLoader, and choose the **billing mode** and term.
6. Buy, then open the instance from *Cloud Products → Game Cloud*. The panel address, username and password are on the management page.
7. Start the instance. The first boot downloads and installs the server; the log printing `Done` means it's up.
8. Copy the address and port from the console (the panel shows it as `host:port`) and enter it in Terraria under *Multiplayer → Join via IP*. Give the world a password in the server config if it's public-facing.
9. Grant yourself admin from the panel console once you're connected.

If you're migrating an existing TShock world, use the file manager or SFTP to replace the `ServerPlugins` and `TShock` folders — and leave the startup script and `TShock.Server` binary alone, since those are the Linux builds the container expects. Uploading world files works the same way on any flavor.

👉 [Start a Terraria server and pick your configuration](https://www.rainyun.com/MzA2MTc2_)

## Where a China-based game cloud is the wrong choice

This is the section most hosting recommendations leave out, so here it is plainly.

**Latency.** The game cloud nodes sit in mainland China — Jiangsu Suqian, Hubei Shiyun, Zhejiang Ningbo and Wenzhou, Chongqing and similar — which is excellent for players across Asia and poor for a group split between Chicago and Berlin. Terraria is a 2D game and tolerates latency better than an FPS, but lag becomes noticeable past roughly 100 ms during co-op boss fights, and cross-continent routing to a Chinese node will clear that comfortably. If your group is mostly North American or European, a Western host with a datacenter near your players is the better answer, full stop.

**Language.** RainYun's console, documentation and support run in Chinese. Translation gets you through the purchase flow, and the panel itself is largely visual, but a support ticket about a weird TShock plugin error is going to be a Chinese-language conversation.

**No slot-based pricing.** If you want to pay per player slot and never think about cores again, the prepaid Western hosts are simpler. The configurator model is cheaper for what you get, but it asks you to have an opinion about RAM.

**Shared IP by default.** NAT mode is the cheap default and works fine for a game server — you're mapping ports, not publishing a website — but a dedicated IPv4 costs extra, and if your players need a memorable static address rather than `something.rainplay.cn:23317`, factor that in.

One more: the 15-port allowance and dual-stack bandwidth figures apply to the VPS shape. Panel instances get NAT port mapping rather than a stack of open ports, which is plenty for Terraria and its query port, and not enough if you planned to run three game servers on one instance.

## FAQ

**How much RAM does a Terraria server need?**
1–2 GB handles a vanilla world for 2–4 players. 2–4 GB covers vanilla with 8 players or tModLoader with light mods. Calamity plus Thorium on a large world with a full group wants 4–8 GB.

**Can I run Calamity on a TShock server?**
No. Calamity is a tModLoader mod. TShock and tModLoader are separate server flavors, so decide before you buy — RainYun's panel lists both, along with vanilla.

**Is a panel server or a VPS better for Terraria?**
The panel, unless you specifically want root access for other services. Terraria isn't resource-hungry enough to justify a full OS, and the panel gives you backups, MySQL and file management without setup.

**What does a Terraria server cost per month?**
Western managed hosts start around $1.49–$5.99/month depending on RAM and slots. A RainYun panel instance on the cheaper Xeon starts in the region of ¥25/month, before the first-month coupon or the annual 30% discount.

**Do I need to buy a domain or file anything with Chinese authorities?**
No. Game traffic through NAT port mapping doesn't involve the domain filing that applies to HTTP hosting on mainland IPs.

**Can I test before committing?**
Yes — one-day trials start at ¥1–2 depending on configuration, and there's a 7-day no-reason refund window, up to three times per account.

## The short version

Rent, don't self-host, unless your group is two people with matching schedules. Match the server flavor to your plans: vanilla for a straight playthrough, TShock if strangers will be joining, tModLoader if mods are involved. Size at 2 GB for a vanilla friend group and 4–8 GB for a modded world, and buy more disk than the default.

If your players are in Asia and you want to pay for hardware rather than per slot, RainYun's panel game cloud is a genuinely cheap way to get a 24/7 Terraria world with one-click install, switchable CPU models and automated backups — just confirm the ping during the trial before you commit to a year.

👉 [Pick a Terraria-ready configuration on RainYun](https://www.rainyun.com/MzA2MTc2_)
