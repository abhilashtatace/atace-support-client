# ATACE Support — branding fork of RustDesk

This is a fork of [RustDesk](https://github.com/rustdesk/rustdesk) **1.5.3**,
rebranded as **ATACE Support**.

**Licence: AGPL-3.0-or-later**, inherited from upstream. This repository is public
because AGPL requires the modified source to be available to anyone who receives the
binary. See `LICENCE`.

ATACE Inc. is not affiliated with or endorsed by RustDesk / Purslane Ltd.

## What was changed

| File | Change |
|---|---|
| `src/core_main.rs` | Sets `APP_NAME` to `ATACE Support` at startup, before any config path, registry key, service name or window title is derived from it |
| `flutter/windows/runner/main.cpp` | Window title |
| `flutter/windows/runner/Runner.rc` | ProductName, FileDescription, CompanyName, InternalName, OriginalFilename, LegalCopyright |
| `Cargo.toml` | Package description |
| `flutter/pubspec.yaml` | App description |
| `res/icon.ico`, `res/*.png`, `flutter/windows/runner/resources/app_icon.ico` | ATACE mark |
| `.github/workflows/atace-windows.yml` | Windows x64 build, derived from upstream `flutter-build.yml` |

**Deliberately not changed:** the signature verification in
`src/common.rs::read_custom_client`. That function validates RustDesk's signed custom
client config against their public key. It is their commercial licensing control and
we do not circumvent it — we rebrand by editing source, which AGPL permits, and publish
this fork accordingly.

The Rust crate is still named `rustdesk` internally. Renaming it breaks build paths,
artefact names and the Flutter FFI binding for no user-visible benefit — the crate name
never appears in the UI.

## Building

Builds run on GitHub Actions Windows runners. Public repositories get free minutes.

1. Actions tab → **ATACE Support - Windows x64** → *Run workflow*
2. Expect roughly 40–70 minutes for a cold build (vcpkg compiles libvpx, libyuv, opus)
3. Download the artefact from the run summary

Local Windows builds need Visual Studio 2022 Build Tools, Rust 1.75, Flutter 3.24.5,
LLVM 15.0.6 and vcpkg. See upstream `docs/`.

## Known limitations

- **Unsigned.** Windows SmartScreen will warn on first run. An EV code-signing
  certificate (~AED 1,500/year) is needed to remove that.
- **No web console, device management, technician accounts or session logging.** Those
  are RustDesk Server Pro server-side features, not client features. Rebranding the
  client does not provide them.
- **Upstream updates require rebuilding this fork** and resolving any conflicts in the
  files listed above.

## Server configuration

The client still needs pointing at the ATACE server. Either:

- rename the built exe to `ATACE-Support-host=<host>,key=<pubkey>.exe`, or
- ship a config string, or
- configure manually under Settings → Network.
