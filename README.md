# mullvad-wg-monitor

A Python desktop tool for benchmarking Mullvad VPN WireGuard servers across seven US cities. Pings servers sequentially with per-packet live animation, stores results in SQLite for historical trending, and visualises everything with Seaborn/Matplotlib and an interactive Folium map.

![Python](https://img.shields.io/badge/python-3.8%2B-blue)
![License](https://img.shields.io/badge/license-MIT-green)
![Platform](https://img.shields.io/badge/platform-Windows%20%7C%20macOS%20%7C%20Linux-lightgrey)
![Version](https://img.shields.io/badge/version-2.0.0-informational)

---

## Features

### Core
- **7 US regions** — Los Angeles, New York, Seattle, Chicago, Dallas, Atlanta, Miami; switch regions from the header dropdown at any time
- **Live ping animation** — per-packet sparkline bar (▁▂▄▆█) builds in real time as packets arrive; no waiting for all packets to finish
- **Jitter column** — max−min range shown alongside avg/min/max latency and packet loss per server
- **Ping count slider** — choose 2–20 packets per server; more = more accurate averages
- **Threshold alerts** — set a ms ceiling; rows highlight red and the status bar fires a message when any server exceeds it
- **Sortable columns** — click any column header in the results table to sort

### Automation
- **Auto-ping scheduler** — repeat the test every 0.5–60 minutes; configurable via slider
- **System tray mode** — minimize to the system tray with a coloured dot; tray menu for Show, Run Now, Quit
- **"Best server" auto-connect** — shells out to `mullvad relay set hostname` if the Mullvad CLI is installed

### Data & Export
- **Export CSV / JSON** — save the full results table to a file
- **Copy best server** — one click copies the lowest-latency hostname to clipboard for pasting into the Mullvad app
- **Historical graphing** — SQLite database stores every run; History tab shows time-series latency and a best-15 bar chart, filterable by server name
- **Side-by-side comparison** — save any two runs as A and B, then compare with a delta table (Δavg ms, Δjitter, Winner column)

### Visualisation
- **Statistics dashboard** — five Seaborn/Matplotlib charts: sorted bar chart with error bars, KDE distribution curves, jitter box plot, min vs max scatter, packet loss heatmap
- **Static in-app map** — Matplotlib scatter of datacenter locations with bubble size scaled to server count, live-updated colours after each test, and distance lines from your location
- **Interactive browser map** — Folium/Leaflet dark map with datacenter circles, individual server dots, hover tooltips, dashed polylines to your location coloured by latency
- **Traceroute tab** — run traceroute/tracert to any server with a live hop table and bar chart of hop latencies
- **Speed test tab** — HTTP throughput test (Cloudflare or Mullvad CDN) with donut gauges showing avg and peak Mbps
- **Dark / light theme toggle** — full re-theme including all matplotlib charts

---

## Screenshots

> Run the tool and add screenshots here — contributions welcome!

---

## Requirements

- Python 3.8 or newer
- The following packages (all installable via pip):

```
matplotlib>=3.7.0
seaborn>=0.12.0
pandas>=2.0.0
numpy>=1.24.0
folium>=0.14.0
pystray>=0.19.0   # optional — only needed for system tray mode
Pillow>=10.0.0    # optional — only needed for system tray mode
```

> **Linux:** If tkinter is missing, install it via your package manager:
> ```bash
> sudo apt install python3-tk       # Debian / Ubuntu
> sudo dnf install python3-tkinter  # Fedora
> sudo pacman -S python-tk          # Arch
> ```

---

## Installation

### Option A — GUI Installer (recommended)

Download `MullvadPingInstaller.exe` from the [Releases](../../releases) page and run it. No prior Python installation required — the installer handles everything:

- Detects whether Python 3 is installed; offers to download it if not
- Checks and installs all required packages via pip
- Lets you choose an install directory
- Creates a launcher script (`.bat` on Windows, `.sh` on macOS/Linux)
- Optionally creates a desktop shortcut and Start Menu entry
- Launches the app automatically when done

### Option B — Manual install

```bash
# 1. Clone the repo
git clone https://github.com/YOUR_USERNAME/mullvad-wg-monitor.git
cd mullvad-wg-monitor

# 2. Install dependencies
pip install matplotlib seaborn pandas numpy folium pystray Pillow

# 3. Run
python mullvad_ping.py
```

### Building the installer exe yourself

```bash
pip install pyinstaller
pyinstaller --onefile --windowed --name "MullvadPingInstaller" installer.py
```

The exe will be in `dist/`. Place `mullvad_ping.py` in the same folder before distributing, or users can select "Download from GitHub" inside the installer.

---

## Usage

1. Launch the app — a dark-themed window opens.
2. Select a **Region** from the header dropdown (default: Los Angeles).
3. Optionally adjust the **Pings** slider (default: 4 packets per server).
4. Set an **Alert** threshold if you want rows highlighted when latency exceeds it.
5. Click **▶ Run** — servers are pinged one at a time; the Live column animates per packet.
6. Once complete, explore the tabs:

| Tab | Contents |
|-----|----------|
| **Results** | Full table with avg/min/max/jitter/loss, sortable by any column |
| **Statistics** | Five auto-generated Seaborn/Matplotlib charts |
| **Map** | Static datacenter scatter map + Open Interactive Map button |
| **History** | Time-series latency and best-15 bar chart from SQLite history |
| **Traceroute** | Run traceroute to any server; live hop table + bar chart |
| **Speed Test** | HTTP throughput test with donut gauges |
| **Compare** | Side-by-side delta table for two saved runs |

### Toolbar actions

| Button | Action |
|--------|--------|
| **⎘ Copy Best** | Copies the best server hostname to clipboard |
| **↓ Export CSV / JSON** | Saves current results to a file |
| **⊞ Save as Run A / B** | Snapshots current results for comparison |
| **⇄ Compare A vs B** | Opens the Compare tab with a delta table |
| **⚡ Auto-Connect** | Runs `mullvad relay set hostname <best>` via CLI |

### Auto-ping schedule

Check **Auto-ping every** in the header and drag the slider to set the interval (0.5–60 minutes). The test repeats automatically and saves each run to history.

### System tray

Click **⊟ Tray** to minimize to the system tray. Requires `pystray` and `Pillow` (`pip install pystray Pillow`). Right-click the tray icon for Show, Run Ping Now, and Quit.

---

## Interpreting results

| Colour | Meaning |
|--------|---------|
| 🟢 Green | Excellent — under 30 ms |
| 🔵 Cyan | Good — 30–70 ms |
| 🟡 Yellow | Slow — 70–120 ms |
| 🔴 Red | High latency — over 120 ms |
| ⚫ Grey | Unreachable / 100% packet loss |
| 🔴 Highlighted row | Exceeds configured threshold |

---

## Regions and datacenters

| Region | Groups | Approx. facilities |
|--------|--------|--------------------|
| Los Angeles | LAX-A–E | CoreSite LA1, Equinix LA3/LA4, Zayo Burbank, CoreSite LA2 |
| New York | NYC-A–D | Equinix NY9, Zayo NYC, Equinix NY5, Corelink Brooklyn |
| Seattle | SEA-A–C | Equinix SE2, Sabey Bellevue, Westin Redmond |
| Chicago | CHI-A–C | Equinix CH4, CyrusOne Elk Grove, Flexential Westmont |
| Dallas | DAL-A–C | Equinix DA2, CyrusOne Carrollton, Stream Global Irving |
| Atlanta | ATL-A–C | Equinix AT2, QTS Suwanee, Corelink College Park |
| Miami | MIA-A–C | Equinix MI1, CyrusOne North Miami, Datasite Doral |

> Datacenter assignments are inferred from Mullvad's public infrastructure and may not reflect their exact internal routing.

---

## Notes

- This tool uses the system `ping` command and does not require root/administrator privileges.
- No data leaves your machine — all pings go directly from your system to Mullvad's servers.
- Location detection uses [ip-api.com](https://ip-api.com) (IP-based) or [Nominatim/OpenStreetMap](https://nominatim.openstreetmap.org) (address search). No API key is required for either.
- History is stored locally at `~/.mullvad_ping/history.db` (SQLite).
- This project is not affiliated with or endorsed by Mullvad VPN AB.

---

## License

MIT — see [LICENSE](LICENSE) for details.
