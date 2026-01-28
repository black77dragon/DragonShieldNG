# Dragon Shield – Personal Asset Management 🐉🛡️

**Version 4.7.1** | September 13th, 2025

Dragon Shield is a native macOS application for private investors to track, analyze and document all assets entirely offline. Every byte of financial data remains on your Mac, encrypted in a local database—no cloud, no telemetry.

The app follows Apple's best-in-class UX conventions while embracing ZEN-minimalism for clarity and focus.
For detailed interface guidelines, see the [Dragon Shield UI/UX Design Guide](DragonShield/docs/UX_UI_concept/dragon_shield_ui_guide.md).

## ✨ Feature Overview

- **Local-first & private**: Data stays on-device and is encrypted with SQLCipher (AES-256). All analytics run locally via Swift and Python helpers.
- **Configurable dashboard**: Drag, drop, and persist custom tile layouts (accounts needing updates, upcoming alerts, strict unused/unthemed instruments, top positions, crypto top 5, themes overview, currency exposure, risk buckets, etc.) with a shared tile style (`View+DashboardTileStyle`).
- **Alerts cockpit**: The Alerts & Events area combines a severity-filtered timeline (Charts-based), trigger-type chips, rich detail table, and a startup popup that surfaces near-term deadlines.
- **Portfolio & instrument management**: Maintain institutions, accounts, instruments, portfolio themes, and tags using the new floating search pickers and autosaving table layouts; column widths persist across sessions.
- **Transactions journal**: Record buy/sell trades with the two-leg `Trade` + `TradeLeg` schema, validation, holdings preview, and history view (Release 4.7.1).
- **Import & maintenance tooling**: Python utilities parse CSV/XLSX/PDF statements (e.g. Credit Suisse), refresh FX & instrument prices, and perform encrypted database backup/restore. Settings export also generates the read-only SQLite snapshot used by the iOS viewer prototype.

### In Design / Planned
- Alerts timeline narrative mode and AI-assisted summaries (`docs/design/new_alerts_timeline_tab.md`).
- Ichimoku Dragon momentum scanner pipeline (`docs/IchimokuDragon/`).
- Touch ID-secured key management & notarised macOS build.
- Options valuation models and what-if rebalancing.
- Signed public beta and iOS companion (Phase 1 read-only viewer via snapshot import).


## 🐉 Ichimoku Dragon Market Scanner

The new **Ichimoku Dragon** module automates daily scans of S&P 500 and Nasdaq 100 constituents using full Ichimoku Kinko Hyo analytics.

- Pulls up to 300 days of history per ticker from Yahoo Finance (Finnhub fallback planned).
- Computes Tenkan, Kijun, cloud spans and regression-based slopes to rank top momentum candidates.
- Tracks open positions, raises sell alerts when price closes below the Kijun Sen and logs every pipeline run.
- Provides a dedicated SwiftUI dashboard with watchlist, alert center, history browser and configurable schedule (default 22:00 Europe/London).
- Generates a daily CSV report stored under `~/Library/Application Support/DragonShield/IchimokuReports`.

See `docs/IchimokuDragon/user_guide.md` for the workflow and screen tour.

## 🚧 Current Status & Roadmap

Highlights from the latest iterations:

### Delivered
- Dashboard tile system with autosaved layouts, FX/price update actions, and upcoming-alert popups on launch.
- Alerts settings + timeline rebuild (Charts-based) with severity filters, trigger chips, and detailed table view.
- Portfolio themes workspace, strict unused/unthemed instrument reports, and floating search pickers across forms.
- Transactions journal (Trade/TradeLeg schema) with holdings preview, validation, and history view (Release 4.7.1).
- Database tooling: dbmate migrations, encrypted backup/restore, import sessions, and Settings → Data Export snapshot for the iOS viewer prototype.

### In Progress
- CSV/XLSX import refinement and alert timeline UX follow-up (`docs/design/new_alerts_timeline_tab.md`).
- Research spikes for the Ichimoku Dragon scanner (models/services under `DragonShield/Ichimoku`).

### Next Up
- Touch ID-secured key storage and notarised macOS build pipeline.
- Options pricing experiments and rebalancing workflow.
- Expanded PDF import coverage & automation.
- Signed public beta and packaged iOS read-only client via snapshot export.

## 🛠️ Technology Stack

- **Frontend**: Swift / SwiftUI (+ Charts)
- **Backend Logic**: Python 3.11 (parsing, analytics)
- **Database**: Encrypted SQLite (SQLCipher v4)
- **Build Tools**: Xcode 15+, SwiftPM, Python venv
- **Swift⇄Python Bridge**: CLI invocation (Swift spawns Python scripts)

## 📁 Project Structure

```
DragonShield/
├── DragonShield/                    # macOS app sources (SwiftUI views, helpers, db access, python bridge)
│   ├── Views/                       # Dashboard, alerts, portfolio, trades, settings
│   ├── helpers/                     # Palette, modifiers, shared UI components (e.g. dashboard tile style)
│   ├── db/migrations/               # dbmate SQL migrations (e.g. Trade/TradeLeg, ichimoku schema)
│   ├── python_scripts/              # Offline import, analytics, and backup tooling
│   ├── Ichimoku/                    # Experimental scanner models, repositories, services
│   └── (additional feature modules)
├── DragonShield iOS/                # Designed-for-iPad viewer target (snapshot import prototype)
├── DragonShieldTests/               # XCTest target (floating picker, autosave table, data access)
├── Model/                           # Shared Swift model definitions
├── docs/                            # Repo-level docs (design/specs/process, releases)
│   ├── design/                      # UI/UX designs, mockups, proposals
│   ├── specs/                       # Feature and system specs
│   ├── process/                     # Release/build/checklist notes
│   ├── IchimokuDragon/              # Ichimoku momentum scanner documentation
│   └── releases/                    # Release briefs
├── DragonShield/docs/               # Tooling-pinned docs (manifested)
├── scripts/                         # Repo tooling (release sync, git info, CI, ds helper)
├── CHANGELOG.md
├── README.md
└── requirements.txt
```

Note: runtime Python scripts used by the app are bundled under `DragonShield/python_scripts/`.
Repo tooling (release/CI/helpers) lives under `scripts/`.

Additional references:
- `docs/README.md` is the index for repo-level documentation.
- `docs/releases/` retains release briefs (latest: `4.7.1.md`).
- `docs/design/`, `docs/specs/`, and `docs/process/` organize repo-level notes by intent.
- `DragonShield/docs/UX_UI_concept/dragon_shield_ui_guide.md` covers detailed UI/UX conventions.
- `new_features.md` (a.k.a. "New_Feature.Md") tracks the backlog of planned changes and upcoming work.

## 🚀 Getting Started / Setup

1. **Clone the repository**
   ```bash
   git clone <your-repository-url>
   cd DragonShieldNG
   ```

2. **Create Python virtual environment (outside of the `DragonShield` app folder)**
   ```bash
   python3 -m venv python_scripts_venv
   ```

3. **Activate virtual environment**
   ```bash
   source python_scripts_venv/bin/activate
   ```

4. **Install Python dependencies**
   ```bash
   pip install -r requirements.txt
   ```
   All required packages are listed in `requirements.txt`.

5. **Run database migrations**
   ```bash
   dbmate --migrations-dir DragonShield/db/migrations --url "sqlite:DragonShield/dragonshield.sqlite" up
   ```
6. **Open the Xcode project**
   ```bash
   open DragonShield.xcodeproj
   ```
7. **Build & run**
   - Select DragonShield → My Mac and press ⌘R

> **⚠️ Important**: The current build is developer-only. Replace the default DB key with a Keychain-managed key before storing real data.

## 🧪 Testing

- Unit tests live in the `DragonShieldTests` target (e.g. floating search picker, autosave table helpers).
- The bundled schemes (`DragonShield`, `DragonShield iOS`) do not include a Test action by default. Add one via **Product → Scheme → Edit Scheme…** (or duplicate a scheme) before running `xcodebuild test`.
- Once the Test action is enabled you can exercise the suite from Xcode or the CLI, for example:
  ```bash
  xcodebuild test -scheme DragonShield -destination 'platform=macOS,arch=arm64'
  ```
- Tests rely on macOS 14+/Xcode 15+ for SwiftUI + Charts APIs.

## 💾 Database Information

- **Type**: SQLite
- **Production Folder**: `/Users/renekeller/Library/Containers/com.rene.DragonShield/Data/Library/Application Support/DragonShield`
  - Primary database: `dragonshield.sqlite`
  - Test database: `dragonshield_test.sqlite`
  - Backup directory: `Dragonshield DB Backup` (full, reference and transaction backups)
  - Apply migrations with `dbmate --migrations-dir DragonShield/db/migrations --url "$DATABASE_URL" up`.
  - Full database backup/restore via `python3 python_scripts/backup_restore.py`
- **Encryption**: SQLCipher (AES-256)
- **Migrations**: `DragonShield/db/migrations`
- **Dev Key**: Temporary; do not use for production data

### Export Snapshot for iOS
Use Settings → **Export to iCloud Drive…** to generate a consistent read‑only `DragonShield_YYYYMMDD_HHMM.sqlite` file. Import the file in the iOS app via the Files app on your iPhone.

## Updating the Database

Run dbmate to apply migrations and copy the database to the container's Application Support folder. The command prints the applied versions and final path:

```bash
dbmate --migrations-dir DragonShield/db/migrations --url "$DATABASE_URL" up
```

### ZKB CSV Import
Drag a `Depotauszug*.csv` file onto the **Import ZKB Statement** zone in the Data Import/Export view or use *Select File* to choose it manually. The parser maps all rows to the PositionReports table.


### GPT Shell
A small CLI to experiment with OpenAI function calls. Requires `OPENAI_API_KEY` in the environment.

```bash
python3 python_scripts/gpt_shell.py list
python3 python_scripts/gpt_shell.py schema echo
python3 python_scripts/gpt_shell.py call echo '{"text": "hello"}'
```
## 💡 Usage

At present the application must be run from Xcode. Future releases will ship a signed & notarized .app bundle.

## 🛠 Troubleshooting

See [DragonShield/docs/troubleshooting.md](DragonShield/docs/troubleshooting.md) for solutions to common issues. This includes the harmless
`default.metallib` warning printed by the Metal framework on some systems.

## Thesis Management (v1.2) quick start
- Run: open the macOS app target in Xcode and navigate to Sidebar → Thesis Mgmt. From the dashboard, hit “Review” on any card to enter focus mode; detail tabs cover Trends, History, KPIs, and LLM import/export.
- Tests: `xcodebuild -scheme DragonShield -destination 'platform=macOS' test -only-testing:DragonShieldTests/ThesisManagementTests`

## 🤝 Contributing

This is a personal passion project, but issues and PRs are welcome. Please keep PRs focused and well-documented.

## 📜 License

-Dragon Shield is released under the MIT License. See LICENSE for full text.


## Version History
- 4.7.1: Added transactions journal (Trade/TradeLeg schema), new timeline UX, dashboard tile updates. See docs/releases/4.7.1.md for full notes.
- 2.24: Search for Python interpreter in Homebrew locations or env var.
- 2.23: Run parser via /usr/bin/python3 to avoid sandbox xcrun error.
- 2.22: Launch parser via /usr/bin/env and return exit codes.
- 2.21: Add source-path fallback for locating parser module.
- 2.20: Simplify parser invocation using module path and PYTHONPATH.
- 2.19: Expand parser search to PATH and parent directories.
- 2.18: Search Application Support and env var path for parser.
- 2.17: Display checked parser paths in import error messages.
- 2.16: Enhanced parser lookup and logging for easier debugging.
- 2.13: Added logging guidelines reference document.
- 2.12: Deleting an institution now removes it from the list immediately.
- 2.11: Improved Institutions maintenance UI with edit and delete actions.
- 2.10: Institutions screen now supports add, edit and delete with dependency checks.
- 2.9: Added Hashable conformance for InstitutionData.
- 2.8: Fixed compile issue in AccountsView.
- 2.7: Added Institutions table and management view.
- 2.6: Updated default database path to container directory.
- 2.5: Settings view shows database info and added `db_tool.py` utility.
- 2.4: Import script supports multiple files and shows summaries.
- 2.3: Import tool compatible with Python 3.8+.
- 2.2: Removed bundled database; added generation instructions and ignore rule; documented schema version 4.7 and added `db_version` configuration; added requirements file and clarified setup instructions; automated database build and deployment with version logging; added Python tests and CI workflow.
- 2.1: Documented database deployment script.
- 2.0: Initial project documentation.
