# Crab — Release Binaries

This repository hosts pre-built binaries and installers for [Crab](https://crab.build), a serverless git remote helper for large files.

## What's here

GitHub Releases in this repo contain platform-specific installers and update manifests for:

- **Crab Desktop** — Electron desktop client (macOS DMG/ZIP, Linux AppImage/deb, Windows NSIS)
- **Crab CLI** — standalone command-line binary

## Auto-update

Crab Desktop uses `electron-updater` pointed at this repo. The `latest-mac.yml`, `latest-linux.yml`, and `latest.yml` manifests in each release drive the in-app update flow.

## Source code

This is a **binary distribution repo only**. Source code lives in a separate private repository. The auto-generated source archives attached to releases by GitHub contain only this README.

## Links

- Website: [crab.build](https://crab.build)
- Documentation: [crab.build/docs](https://crab.build/docs)
