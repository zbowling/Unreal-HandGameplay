# Agent Instructions — Hand Gameplay Showcase (Unreal)

The Hand Gameplay Showcase is an Unreal sample for Meta Quest with reusable C++ components for hand-tracked interactions (teleport, grab, throw, button push, punch, two-handed aim) packaged in the `OculusHandTools` plugin.

## Source-of-truth files (read these first, do not duplicate their contents in this file)

For setup, build steps, SDK versions, and project layout, read:

- `README.md` — official setup, editor paths, feature table, and Updates section with migration notes
- `HandGameplay.uproject` — engine association and enabled plugins
- `Plugins/OculusHandTools/README.md` and the per-feature `README_*.md` files (`README_HandInput.md`, `README_HandPoseRecognition.md`, `README_HandTrackingFilter.md`, `README_Interactable.md`, `README_ThrowAssist.md`, `README_OculusUtils.md`) — plugin module reference
- `.gitattributes` — Git LFS filters; run `git lfs install` before cloning
- `LICENSE` — license terms

## Quest / Horizon-specific notes

- The March 2025 update moved the project from OVRPlugin to **OpenXR with Meta vendor extensions**. If you switch back to OVRPlugin, the README provides explicit replacement transforms for grab poses on `Content/HandGameplay/Probs/Blocks/InteractableBrick` and `Content/HandGameplay/Probs/RingWeapon/InteractableArtifactHandle` — do not regenerate poses by hand without using those reference values.
- The recommended workflow for regenerating hand transforms is opening `HansCharacterHandsState` from `OculusHandTools/Content/Hands/` and reconnecting the Blueprint flow nodes — respect the README's instructions instead of editing pose transforms manually.
- The **Hand Movement Filtering** module requires the Oculus Unreal fork of UE; on a stock Epic engine it silently no-ops. Verify the editor build before chasing missing-stabilization bugs.
- Read the README "Updates" section before touching engine/SDK pins — recent updates include a jitter fix for grabbed objects tied to a specific engine + Meta SDK combination.

## Meta Quest tooling

This repository is part of the Meta Quest / Horizon OS ecosystem (a sample, library, template, or related project — the bespoke intro above describes which). Use that intro and the source-of-truth files it references for project-specific decisions; don't restate or invent facts from memory.

When the user asks anything about Quest device behavior, build / deploy / debug / capture flows, on-device performance, or Horizon OS APIs, reach for these tools instead of generic Unreal answers:

- **`hzdb`** — Quest-aware ADB wrapper (device list, install / launch / stop, logs, screenshots, Perfetto traces, on-device docs search). Already wired up as an MCP server via `.mcp.json`, `.vscode/mcp.json`, and `.cursor/mcp.json`. Also runnable directly: `npx -y @meta-quest/hzdb <subcommand>`.
- **Meta Quest Agentic Tools** — the full skill set, including Unreal-specific skills: [github.com/meta-quest/agentic-tools](https://github.com/meta-quest/agentic-tools). Install per your client (Claude Code: `/plugin install meta-vr@meta-quest`; Gemini CLI: `gemini extensions install https://github.com/meta-quest/agentic-tools`; Cursor / VS Code: install the **Meta Horizon** extension from the Marketplace).

A few behavior expectations:

- **Read this repo's files first.** Before answering anything project-specific, read `README.md` and whichever source-of-truth files the intro above points at. Don't restate their contents in chat — quote or link instead.
- **Use `hzdb` for device-side work.** Anything that touches an attached Quest (install, launch, logs, screenshot, capture, manifest inspection) goes through `hzdb`, not raw `adb`.
- **Check live Horizon OS docs before answering API questions.** `hzdb docs search "..."` queries the live docs; training data on Horizon OS APIs goes stale fast.
- **Don't fabricate SDK / engine versions.** If a version isn't visible in this repo's files, say so rather than guessing.
