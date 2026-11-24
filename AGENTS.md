# AGENTS.md — ai-note

Project: `ai-note`; platform: `t5ai`; framework: `base`.

Embedded firmware scaffolded with the IDE built-in stub (SDK not yet installed) into `source/embedded/`. Install the SDK from the Home tab, then `cd source/embedded && tos.py build`.

## Project facts

- `tuyaopen.project.ini`: project identity, platform/board and bound product PID.
- `.tuyaopen/project.json`: identity and optional `ai.intent`; `ai.skills` is not an installed-skill registry.
- `.tuyaopen/status.json`: recorded lifecycle. Advance it with the project command after the corresponding step succeeds.
- `.tuyaopen/architecture.json`: implemented modules and hardware; update after implementation, not before.
- `.tuyaopen/ide/`: IDE-owned board/platform context and wrapper executables; do not edit these files.
- `.tuyaopen/platform/product-<pid>.json`: cached platform data. Unwrap `data` when present; do not invent product IDs/DPs or write platform snapshots manually.
- `source/embedded/`: firmware; its actual CMakeLists.txt decides which sources compile. Do not assume recursive source discovery.
- `source/miniapp/`: optional phone panel; firmware and panel completion are separate.
- `.agents/skills/<id>/`: project skill copies, mirrored under `.claude/skills/`. Users may have modified them.

## Start from the requested task

Work only to the user's requested endpoint. Building does not imply flashing;
previewing does not imply uploading, reviewing or publishing.

1. Resolve the CLI: explicit `TUYAOPEN_CLI_PATH` (a JS entry, run with Node), then this project's `.tuyaopen/ide/bin/tuyaopen-cli` (`.cmd` on Windows), or an available `tuyaopen-cli` on PATH. Resolve to an absolute path before changing directories.
2. If there is no CLI, use the IDE to establish it or read the installed environment skill's resolver. Do not silently install packages or clone an SDK. For a raw SDK task, use the real SDK's `tos.py -h` and its documented fallback.
3. Query `schema get --group <g> --command <c> --json` for current inputs and confirmation requirements; `schema envelope --json` describes result fields. A tool refusal is not a reason to bypass confirmation or authentication with another tool.
4. Run `skills list --json` to discover a task by its purpose, not its name. For execution use `skills resolve --id <id> --project-root <absolute-project-root> --json`: project copy, then global hub, then official cache. Read the returned root's SKILL.md before acting; use its attachments and scripts from that same root/version. A broken project copy must be reported, not bypassed.
5. `skills read --id <id>` intentionally reads the official catalogue version, which may differ from the project's effective copy. It is a reference/recovery channel, not permission to ignore local edits. If needed official payloads are unavailable, explicitly sync the catalogue; report offline failures without guessing.

When launched through the IDE Agent Host, `skills.list`, `skills.resolve` and
`skills.read` provide the same discovery without a shell. Resolve returns a
contentHash; Host reads require that hash. Identity, active project and product
line come from the Host, never from text supplied by a task.

Hardware tasks are owned by `tuyaopen-embedded-hardware`. A local copy may be at
`.agents/skills/tuyaopen-embedded-hardware/SKILL.md`; resolve it instead of assuming
that path exists. Build, flash, debug, project configuration and authorization
are separate task skills. Use the complete embedded workflow only for a
multi-stage request, and record where this request should stop.

## Working directories and evidence

- Keep `projectRoot`, `embeddedRoot` and `sdkRoot` distinct. Project CLI commands use an explicit `--project-root`; direct `tos.py` commands run in `source/embedded` with the appropriate SDK environment.
- Before assigning pins, read actual board and SDK capabilities. Use physically exposed pins, supported multiplexing and confirmed occupancy together. Missing manifest data means unknown; inspect source/schematics or ask for the missing information.
- Reuse hardware choices already confirmed by the user. Ask only for unresolved choices that affect wiring; record confirmed hardware before generating its implementation.
- Board devices, externally attached devices and raw SoC peripherals use different registration/API paths. Follow the hardware skill and real SDK headers/examples; do not impose a blanket prohibition on TKL.
- Follow the build skill for Kconfig. `tos.py check` checks host tools/submodules, not whether application options or hardware initialization are correct.
- Device identity, port and firmware must be established before a device write. Do not derive today's port from an old example.
- Distinguish build, flash, boot, peripheral behavior, credential readback, activation, MQTT and DP roundtrip evidence. `client no active` alone does not prove missing UUID/AuthKey. Never claim unobserved physical behavior or phone pairing succeeded.

## Data and code conventions

- Credentials are owned by the IDE/CLI credential mechanism. Never read credential files, print secrets, embed them in source, or treat the devplat CLI's runtime-config directory as the credential store.
- Use cloud/miniapp skills for their currently available tools. Host Broker confirmation and CLI confirmation are different contracts; query the actual tool instead of copying a token recipe between them.
- Keep generated DP headers/schemas generated; use the installed command's reported output paths, not a hardcoded historical filename.
- Add feature logic in focused modules and keep the application entry thin. Register any new source directories according to the actual CMake setup.
- Use standard lowercase C keywords and the existing project style. SDK type names such as OPERATE_RET remain valid.
- Panel code uses the project's real device context; do not hardcode real or simulated device IDs. Read the Ray/Smart UI skill for APIs and UI conventions.
- Run the appropriate checks on modified files. Report test failures and unavailable evidence accurately.
