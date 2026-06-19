# Crab Release Binaries

This repository hosts release artifacts for [Crab](https://crab.build), a
serverless git remote helper for repositories with large files.

Crab stores repository data directly in cloud object storage such as S3, GCS, or
Azure Blob Storage. There is no separate Git server, database, or LFS endpoint to
operate. The CLI works as both a normal `crab` command and the
`git-remote-crab` helper that lets Git speak to `crab://...` remotes.

## What Crab Is For

Crab is built for teams that want Git workflows with object-storage economics:

- Store large binaries, model files, datasets, and generated artifacts outside a
  central Git server.
- Deduplicate large file content with content-defined chunking and compressed
  xorb storage.
- Clone and hydrate files on demand instead of downloading every large object up
  front.
- Keep normal Git operations recognizable: clone, add, commit, push, hydrate,
  dehydrate, fsck, gc, and status.

## Release Contents

GitHub Releases in this repository may contain two families of artifacts:

- **Crab CLI**: standalone command-line binaries.
- **Crab Desktop**: desktop installers and auto-update manifests.

The CLI release set is:

| Platform | CPU | Asset |
| --- | --- | --- |
| macOS | Apple Silicon | `crab-darwin-aarch64.tar.gz` |
| macOS | Intel | `crab-darwin-x86_64.tar.gz` |
| Linux | ARM64 | `crab-linux-aarch64.tar.gz` |
| Linux | x86_64 | `crab-linux-x86_64.tar.gz` |
| Windows | ARM64 | `crab-windows-aarch64.zip` |
| Windows | x86_64 | `crab-windows-x86_64.zip` |

Each CLI release also includes `SHA256SUMS.txt` for checksum verification.

## Install The CLI

Download the archive for your platform from the latest release, verify it, then
put `crab` on your `PATH`.

On macOS or Linux:

```bash
tar -xzf crab-<platform>-<arch>.tar.gz
chmod +x crab
sudo mv crab /usr/local/bin/crab
sudo ln -sf /usr/local/bin/crab /usr/local/bin/git-remote-crab
crab --version
```

On Windows PowerShell:

```powershell
Expand-Archive .\crab-windows-x86_64.zip -DestinationPath .\crab
.\crab\crab.exe --version
```

Add the extracted directory to `PATH` if you want `crab` available from every
shell. For Git remote helper support on Windows, make sure `crab.exe` is on
`PATH` as `git-remote-crab.exe` or through an equivalent command shim.

## Verify Downloads

On macOS:

```bash
shasum -a 256 -c SHA256SUMS.txt
```

On Linux:

```bash
sha256sum -c SHA256SUMS.txt
```

On Windows PowerShell:

```powershell
Get-FileHash .\crab-windows-x86_64.zip -Algorithm SHA256
```

Compare the hash with the matching line in `SHA256SUMS.txt`.

## First Commands

After installing, start with:

```bash
crab --help
crab doctor
```

Typical repository workflows use Crab as a Git remote helper:

```bash
git clone crab://<bucket-or-container>/<repo>
cd <repo>
crab track "*.bin"
git add .gitattributes model.bin
git commit -m "track large model"
git push
```

Files that are stored as Crab pointers can be materialized later with:

```bash
crab hydrate <path>
```

## Desktop Auto-Update

Crab Desktop uses this repository for update metadata. Release assets such as
`latest-mac.yml`, `latest-linux.yml`, and `latest.yml` are consumed by the
desktop auto-updater when desktop builds are published.

## Maintainer Notes

This repository is a binary distribution repo. Source code and release tooling
live in the main Crab monorepo.

The CLI release contract is the six-platform matrix listed above. The local
release script can build Windows MSVC artifacts from macOS with `cargo-xwin`, so
a macOS maintainer can produce the full CLI matrix when Docker, Rust targets,
LLVM, `cargo-xwin`, and release evidence are available.

For local archive generation from the monorepo:

```bash
cd crab
make release-list
make release-macos-full
```

Publishing to this repository is handled by the release workflow or by the
monorepo release script after the retained enterprise release evidence gate has
passed.

## Links

- Website: [crab.build](https://crab.build)
- Documentation: [crab.build/docs](https://crab.build/docs)
- Releases: [github.com/crabbuild/crab-release/releases](https://github.com/crabbuild/crab-release/releases)
