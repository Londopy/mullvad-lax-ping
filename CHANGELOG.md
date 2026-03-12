# Changelog

All notable changes to this project will be documented here.
Format follows [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).

---

## [1.0.0] — 2026-03-12

### Added
- Ping all 31 Mullvad LAX WireGuard servers (`us-lax-wg-001` through `us-lax-wg-704`)
- Live results table with avg / min / max latency, packet loss %, and color-coded status
- Summary stat cards: best server, worst server, average latency, reachable count
- **Statistics tab** with five Seaborn/Matplotlib charts:
  - Sorted bar chart with min/max error bars
  - KDE latency distribution per datacenter group
  - Box plot by datacenter group
  - Min vs. Max scatter plot (jitter detection)
  - Packet loss heatmap
- **Map tab** with static Matplotlib scatter map of LA-area datacenters
- **Interactive Folium map** (opens in browser) with zoomable datacenter circles,
  individual server dots, hover tooltips, and click popups
- **Address-based location** via Nominatim (OpenStreetMap) — street-accurate,
  no API key required
- IP-based location fallback (ip-api.com) used only to pre-fill the search box
- User position shown on both static and interactive maps with distance lines
  to each datacenter (km) and color-coded PolyLines by latency
- **GUI Installer** (`installer.py`) with:
  - Python detection and download link
  - Per-package install status with enable/disable toggles
  - Custom install directory picker
  - Desktop + Start Menu shortcut creation (Windows)
  - Live pip install log
  - Launch-after-install option
- Converts to a standalone `.exe` via PyInstaller (`--onefile --windowed`)
- Cross-platform ping flag handling (Windows `-n`, macOS/Linux `-c`)
- Dark GitHub-style theme throughout

---

## [2.0.0] — 2026-03-12

### Added
- **7 regions**: Los Angeles, New York, Seattle, Chicago, Dallas, Atlanta, Miami
  — live region switcher in the header, datacenters mapped per region
- **Ping animation**: per-packet sparkline bar builds live in the table as
  packets arrive (▁▂▄▆█) — no more waiting for all packets to finish
- **Jitter column**: max−min range shown per server in the results table
- **Ping count slider**: choose 2–20 packets per server (header)
- **Threshold alerts**: configurable ms ceiling — row highlighted red + status
  bar message when any server exceeds it
- **Auto-ping scheduler**: repeat test every N minutes (0.5–60), configurable
  via slider in the header
- **Export CSV / JSON**: save full results to file via toolbar buttons
- **Copy best server**: one click copies the lowest-latency hostname to clipboard
- **Side-by-side comparison**: save two runs as A/B, compare with a delta table
  (Δavg ms, Δjitter, Winner column)
- **Historical graphing**: SQLite database stores every run; History tab shows
  time-series latency chart + best-15 bar chart with server filter and run limit
- **Traceroute tab**: run traceroute/tracert to any server with live hop table
  + bar chart of hop latencies; "Use Best Server" button auto-fills target
- **Speed test tab**: HTTP throughput test (Cloudflare, Mullvad CDN); donut gauge
  showing avg + peak Mbps
- **Dark / light theme toggle**: full re-theme including matplotlib charts
- **"Best server" auto-connect**: shells out to `mullvad relay set hostname`
  if the Mullvad CLI is installed
- **System tray mode**: minimize to tray (requires `pystray` + `Pillow`);
  tray menu for Show, Run Ping, Quit
- **Sortable columns**: click any column header in the results table to sort
- Statistics tab: added jitter box plot; threshold line on bar chart

### Changed
- Ping engine rewritten to stream per-packet results for live animation
- Location detection: address-search dialog now prefills from IP city
- Datacenter group info expanded to 7 regions × 2–5 DCs each
- `requirements.txt` updated: added `pystray`, `Pillow`
