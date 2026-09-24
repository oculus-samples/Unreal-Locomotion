# Agent Instructions — Locomotion and Interactions (Unreal)

An Unreal Engine sample for Meta Quest demonstrating a catalog of VR locomotion methods (teleport, stepped translation, grab-and-drag, arm swinging, dual-stick walking) and several physical-grab interactions (cubes, two-handed gun, two-handed pole, bow & arrow).

## Source-of-truth files (read these first, do not duplicate their contents in this file)

For setup, build steps, engine version, and project layout, read:

- `README.md` — official setup, in-app controls, locomotion/interaction inventory
- `Locomotion.uproject` — Unreal engine association, enabled plugins, target platforms
- `Config/` — UE config files
- `Content/Blueprints/` — locomotion logic (`MotionControllerPawn`, `BP_MotionController`, `BP_Pickup*`, etc.); this project is Blueprint-only
- `LICENSE` — license terms

## Quest / Horizon-specific notes

- This is a Blueprint-only project — there is no `Source/` C++ module. Changes go in `.uasset` Blueprints, not `.cpp`/`.h`. Agents asked to "modify locomotion logic" should open `MotionControllerPawn`'s Event Graph, not search C++.
- All grab interactions go through `PickupActorInterface` — new pickup types should implement that interface rather than reimplementing grab semantics.
- The `.uproject` lists `PS4` and `WindowsNoEditor` in `TargetPlatforms`, but only Quest (Android) is the supported deployment surface. Ignore the legacy entries when packaging.
- Dual Stick Walking is intentionally included with an in-README comfort warning; do not promote it as a default locomotion choice without flagging the caveat.
- This repo does not ship a `.gitattributes` LFS config at the root despite the README recommending `git lfs install`. Treat the README as authoritative if you encounter missing binary assets.

# Meta Quest tooling

This is a Meta Quest / Horizon OS sample. The bespoke intro above is the source of truth for what this project is and how it's built — use it (and the files it points at) instead of restating facts from memory.

When the user asks anything about Quest device behavior, build / deploy / debug / capture flows, on-device performance, or Horizon OS APIs, reach for these tools instead of generic Unreal answers:

- **`hzdb`** — Quest-aware ADB wrapper (device list, install / launch / stop, logs, screenshots, Perfetto traces, on-device docs search). Already wired up as an MCP server via `.mcp.json`, `.vscode/mcp.json`, and `.cursor/mcp.json`. Also runnable directly: `npx -y @meta-quest/hzdb <subcommand>`.
- **Meta Quest Agentic Tools** — the full skill set, including Unreal-specific skills: <https://github.com/meta-quest/agentic-tools>. Install per your client (Claude Code: `/plugin install meta-vr@meta-quest`; Gemini CLI: `gemini extensions install https://github.com/meta-quest/agentic-tools`; Cursor / VS Code: install the **Meta Horizon** extension from the Marketplace).

A few behavior expectations:

- **Read this repo's files first.** Before answering anything project-specific, read `README.md` and whichever source-of-truth files the intro above points at. Don't restate their contents in chat — quote or link instead.
- **Use `hzdb` for device-side work.** Anything that touches an attached Quest (install, launch, logs, screenshot, capture, manifest inspection) goes through `hzdb`, not raw `adb`.
- **Check live Horizon OS docs before answering API questions.** `hzdb docs search "..."` queries the live docs; training data on Horizon OS APIs goes stale fast.
- **Don't fabricate SDK / engine versions.** If a version isn't visible in this repo's files, say so rather than guessing.
