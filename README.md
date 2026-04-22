# Driver 2.0 — Grey Hack Hacktool Harness

**Driver 2.0** is a Grey Hack in-game harness that installs, launches, and bridges **5hell** and **HEX** from a single persistent REPL. It uses `get_custom_object` as shared memory between tools — sessions, acquired shells, and the exploit database are all synchronized automatically across tool launches.

---

## What's New in 2.0

- **Cross-tool bridge** — sessions and exploit databases sync between 5hell and HEX automatically via `get_custom_object`
- **Hacking commands in Driver's own REPL** — `scan`, `hack`, `probe`, `ssh`, `zap`, `meta`, `rsi`, `db` without entering a sub-tool first
- **`target` command** — set a target once, it is passed to both tools on launch
- **`sessions` command** — view all acquired shells from any tool in one place
- **Bidirectional database sync** — HEX's exploit database is written back to `/root/rkit/database.csv` after every session
- **Two-step installer** — `install.src` builds all modules and the `driver` binary in one shot; no manual step-by-step building

---

## Prerequisites

- [Visual Studio Code](https://code.visualstudio.com/)
- [greybel-vs](https://marketplace.visualstudio.com/items?itemName=ayecue.greybel-vs) VS Code extension (provides the Messagehook import feature)
- Grey Hack running with **Messagehook** enabled (BepinEx Plugin)  
  → visit [doom.redit0.com → Hex → Getting Started] for setup instructions

---

## Step 1 — Clone the Repository

Clone or download the repo from https://github.com/ChaoticCrumb/Driver

The repository root contains a `Driver2.0/` folder. Open the **repository root** in VS Code — not the `Driver2.0/` folder itself.

```
Driver/                ← open THIS folder in VS Code (repo root)
├── Driver2.0/         ← tool source (imported into game as-is)
│   ├── install.src
│   ├── driver.src
│   └── src/
│       ├── 5hell/
│       ├── hex/
│       └── commands/
└── README.md
```

In VS Code: **File → Open Folder…** → select the repository root (`Driver/`).

---

## Step 2 — Import into Grey Hack

Make sure Grey Hack is running with Messagehook enabled.

In VS Code's **Explorer** sidebar, right-click the **`Driver2.0`** folder and select:

> **Greybel: Import into game**

This uploads the entire `Driver2.0/` folder — including all subdirectories — into in the game at your configured directory location `(configure this to /root/)`.

After the import, the following structure should exist in-game:

```
/root/Driver2.0/
├── install.src
├── driver.src
└── src/
    ├── core.drvr.src
    ├── ui.drvr.src
    ├── commands/
    │   ├── bridge.cmd.drvr.src
    │   ├── install.cmd.drvr.src
    │   ├── hackcmds.cmd.drvr.src
    │   └── launch.cmd.drvr.src
    ├── 5hell/
    │   ├── 5hell.src
    │   ├── 5hell.5pk.src
    │   ├── kore.5pk.src
    │   ├── net.5pk.src
    │   ├── help.5pk.src
    │   ├── dtools.5pk.src
    │   ├── contrib.5pk.src
    │   └── 5phinx.5pk.src
    └── hex/
        ├── installer0.src
        ├── installer1.src
        ├── ...
        └── installer8.src
```

---

## Step 3 — Build the Installer

**Build `install.src` first** — this is the bootstrap step that compiles all Driver modules and produces the `driver` binary.

In the Grey Hack terminal:

```
build /root/Driver2.0/install.src /root/
```

This places the `install` binary at `/root/install`.

---

## Step 4 — Run the Installer

```
/root/install
```

The installer performs the following steps automatically:

1. Creates a temporary build folder at `/root/.driver_build/`
2. Builds each Driver module (`core.drvr`, `ui.drvr`, `bridge.cmd.drvr`, `install.cmd.drvr`, `hackcmds.cmd.drvr`, `launch.cmd.drvr`) into it
3. Builds `driver.src` — which `import_code`s those module binaries — into `/root/driver`
4. Deletes the `/root/.driver_build/` temp folder

On success:

```
  Driver 2.0 installed  ->  /root/driver
```

---

## Step 5 — Run Driver

```
/root/driver
```

Or, if `/root` is in your shell path:

```
driver
```

Driver starts its REPL with a two-line prompt showing your public IP, local IP, user, hostname, and current path.

---

## Step 6 — Install Tools

From the Driver REPL, install 5hell and HEX:

```
install 5hell
install hex
```

Driver builds each tool from the source tree at `/root/Driver2.0/src/` and places the binary at `/root/`:

| Command         | Output binary | Description                          |
|-----------------|---------------|--------------------------------------|
| `install 5hell` | `/root/5hell` | Modular hacktool — 5hell v4.3.6      |
| `install hex`   | `/root/hex`   | Penetration testing framework — HEX  |

The installer also creates all directories the tools expect before first run:

| Directory            | Required by |
|----------------------|-------------|
| `/root/rkit/`        | 5hell       |
| `/root/rkit/tables/` | 5hell       |
| `/root/Config/`      | 5hell       |
| `/root/SecureLibs/`  | HEX         |
| `/root/InsecureLibs/`| HEX         |

`Note: you will need to populate the lib folders accordingly...`

If a tool is already installed, Driver will prompt for confirmation before reinstalling.

`Note: Because Driver comes loaded with the 5hell source code, it's possible to modify the .src files to customize the 5hell configuration. just reinstall after madking modifications...`

---

## Driver REPL Commands

| Command                   | Description                                                          |
|---------------------------|----------------------------------------------------------------------|
| `install <tool>`          | Build and install `5hell` or `hex` from the Driver source tree       |
| `status`                  | Show which tools are installed and their paths                       |
| `launch <tool>`           | Launch an installed tool; bridge syncs state on exit                 |
| `target <ip> {port}`      | Set the active target — passed to both tools automatically on launch |
| `sessions`                | List all acquired shells harvested from 5hell and HEX                |
| `probe <ip> {port}`       | Port scan and whois via 5hell (single-shot)                          |
| `db <-r\|-l\|-m> <args>`  | Exploit database management via 5hell                                |
| `ssh <user@pass> <ip>`    | SSH connect via 5hell                                                |
| `zap <addr> <vuln>`       | Single overflow exploit via 5hell                                    |
| `meta <link\|scan> <args>`| Metaxploit library management via 5hell                              |
| `rsi {flags}`             | Reverse shell interface via 5hell                                    |
| `scan <ip> {port}`        | Find exploitable addresses via HEX (requires `launch hex` first)     |
| `hack <ip> {port}`        | Exploit target and stack session via HEX                             |
| `help`                    | Show the full command list                                           |
| `quit` / `exit`           | Exit Driver                                                          |

**5hell-dispatched commands** (`probe`, `db`, `ssh`, `zap`, `meta`, `rsi`) launch 5hell in single-shot mode and exit immediately — no sub-tool session **retained for use**.

**HEX-dispatched commands** (`scan`, `hack`) call directly into HEX's `g.tools` dispatch map — requires `launch hex` to have been run at least once in the current game session.

---

## Cross-Tool Bridge

Driver uses `get_custom_object` as a persistent shared memory bus between tools. The bridge runs automatically around every `launch` call — no manual steps required.

### Session Bridge

- When 5hell exits, any shells it acquired (`mys*` exports) are harvested into `g.driver.bridge.shells`
- When HEX exits, any sessions in `g.stack` are harvested into `g.driver.bridge.shells`
- On the next launch of either tool, bridged shells are seeded back in so they are available immediately

### Exploit Database Bridge

- When HEX exits, its `g.database` is written back to `/root/rkit/database.csv` (5hell's database format)
- When HEX starts, `/root/rkit/database.csv` is merged into `g.database` — so 5hell's manually built database entries are available to HEX's `hack` command without re-scanning

### Target Passthrough

Setting `target <ip> <port>` in Driver seeds the target into both tools automatically:
- 5hell receives it as a launch argument
- HEX receives it via `g.command` (HEX's internal first-run directive mechanism)

---

## Notes

- The `Driver2.0/` source tree must remain at `/root/Driver2.0/` while Driver is running. Do not move it after importing.
- `driver.src` has a `DEBUG` flag at the top — set it to `true` before building for a more robust REPL output.
- 5hell can be configured before importing: edit the top of `Driver2.0/src/5hell/5hell.src` to adjust `HOME`, security settings, and color palette.
- The `Driver2.0/src/hex/src/` folder contains the actual HEX source code. To modify HEX before installing, edit the source there, rebuild the HEX installers, and re-import Driver.
- HEX-dispatched commands (`scan`, `hack`) require HEX to have been launched at least once in the current Grey Hack session so that `g.tools` is populated in `get_custom_object`.
