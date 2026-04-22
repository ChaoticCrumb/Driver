# Driver — Grey Hack Hacktool Installer

**Driver** is a Grey Hack in-game installer that deploys **5hell** and **HEX** directly to `/root/` from a local source tree.

---

## Prerequisites

- [Visual Studio Code](https://code.visualstudio.com/)
- [greybel-vs](https://marketplace.visualstudio.com/items?itemName=ayecue.greybel-vs) VS Code extension (provides the Messagehook import feature)
- Grey Hack running with **Messagehook** enabled (BepinEx Plugin)
-- visit [doom.redit0.com -> Hex -> Getting Started] for more instructions on setting these up

---

## Setup

### 1. Clone the repository

clone the repo or download it as a zip from https://github.com/ChaoticCrumb/Driver
extract/save the Driver folder to an empty folder so that there is a Parent folder '/Parent/Driver/src/...'


### 2. Open the correct folder in VS Code

Open the **parent directory** of the `Driver/` tool folder — i.e., the repository root that *contains* the `Driver/` folder — not the `Driver/` folder itself.

```
Parent/           ← open THIS folder in VS Code
├── Driver/       ← tool source (do not open this directly)
│   ├── driver.src
│   └── src/
│       ├── 5hell/
│       └── hex/
└── README.md
```

In VS Code: **File → Open Folder…** → select the cloned `Driver` repository root.

---

## Import into Grey Hack

### 3. Import the `Driver/` folder via Messagehook

Make sure Grey Hack is running with Messagehook enabled.

In VS Code's **Explorer** sidebar, right-click the **`Driver`** folder (the inner one containing `driver.src`) and select:

> **Greybel: Import into game**

Upload the entire `Driver/` folder — including all subdirectories — into `/root/` in the game.

After the import completes, the following structure will exist in-game:

```
/root/Driver/
├── driver.src
└── src/
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

## Build

### 4. Build `driver.src` to `/root/`

In the Grey Hack terminal, run:

```
build /root/Driver/driver.src /root/
```

This compiles `driver.src` and places the `driver` binary at `/root/driver`.

---

## Usage

Run the installer from the Grey Hack terminal:

```
/root/driver (or just 'driver' if at root)
```

Driver presents an interactive menu listing available tools (**5hell** and **HEX**). Select a tool by its number to install it. Each tool is built from source and placed in `/root/`.

| Tool  | Output path | Description                     |
|-------|-------------|---------------------------------|
| 5hell | `/root/5hell` | Modular hacktool by Plu70        |
| HEX   | `/root/hex`   | Penetration testing framework    |

---

## Notes

- Driver expects its source tree to remain at `/root/Driver/` while running. Do not move the folder after importing.
- If a tool is already installed, Driver will prompt for confirmation before reinstalling.
- The `src/hex/` installers must be built and run sequentially — Driver handles this automatically.
- The `src/hex/src/` folder contains the actual hex source code if you would like to modify the env_vars before installing. you can either build it straight into Grey Hack or replace the installers and import Driver as instructed.
- 5hell can be configured prior to importing. modify the top portion of 5hell.src to adjust HOME and security settings.
