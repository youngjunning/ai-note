# ai-note

Embedded firmware scaffolded with the IDE built-in stub (SDK not yet installed) into `source/embedded/`. Install the SDK from the Home tab, then `cd source/embedded && tos.py build`.

## Layout

```
ai-note/
├── .tuyaopen/              # IDE + AI metadata (read by AI agents)
│   ├── project.json        # canonical project descriptor
│   ├── status.json         # lifecycle / build / flash state
│   └── architecture.json   # per-surface architecture
├── source/
│   ├── embedded/           # firmware (`tos.py build` runs here)
│   └── miniapp/            # mini-app / panel (Panel SDK scaffolds here)
├── tuyaopen.project.ini    # legacy human-readable mirror
└── README.md
```

## Surfaces

- **Embedded firmware** — `source/embedded/`. Build & flash via `tos.py` once the TuyaOpen SDK is installed.
- **Mini app (panel)** — `source/miniapp/`. Scaffold the panel project there with the Tuya Panel App SDK; see `source/miniapp/README.md`.
