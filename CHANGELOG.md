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
