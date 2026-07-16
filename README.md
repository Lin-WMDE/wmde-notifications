# WMDE Notifications

Layer Shell notifications daemon which integrates with WMDE (a fork of
pop-os/cosmic-notifications).

# Building

Part of the WMDE desktop stack. `Cargo.toml` redirects libcosmic (and the
crates it vendors) plus cosmic-panel-config to the sibling WMDE forks via
`[patch]` path dependencies, so the checkout must sit next to `../libcosmic`
and `../wmde-panel`. The WMDE build harness arranges this layout; a standalone
build needs the same siblings present.

Some build dependencies:
```
  cargo, just, clang, lld, libxkbcommon, pkgconf
```

## Build Commands

For a typical install from source:
```sh
just
sudo just install
```

# Debugging & Profiling

## Profiling async tasks with tokio-console

To debug issues with asynchronous code, install
[tokio-console](https://github.com/tokio-rs/console) and run it within a
separate terminal. Then kill the **wmde-notifications** process a couple times
in quick succession to prevent **wmde-session** from spawning it again. Then
start **wmde-notifications** with **tokio-console** support either by running
`just tokio-console` from this repository to test code changes, or
`env TOKIO_CONSOLE=1 wmde-notifications` to enable it with the installed
version.
