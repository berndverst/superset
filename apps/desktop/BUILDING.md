# Development

Run the dev server without env validation or auth:

```bash
SKIP_ENV_VALIDATION=1 bun run dev
```

This skips environment variable validation and the sign-in screen, useful for local development without credentials.

# Release

When building for release, make sure `node-pty` is built for the correct architecture with `bun run install:deps`, then run `bun run release`.

# Windows (NSIS installer) local build

Must be run on a Windows machine with Visual Studio Build Tools installed (required for native modules such as `node-pty`).

From `apps/desktop`:

```bash
bun run install:deps
bun run clean:dev
bun run generate:icons
bun run compile:app
bun run package:win
```

Or as a single chain:

```bash
bun run install:deps && bun run clean:dev && bun run generate:icons && bun run compile:app && bun run package:win
```

Expected outputs in `apps/desktop/release/`:

- `*.exe` (NSIS installer, e.g. `Superset-1.3.1-x64.exe`)
- `latest.yml` (Windows auto-update manifest)

> **Note:** `@superset/macos-process-metrics` is a macOS-only workspace package. It is
> conditionally loaded at runtime and will not be invoked on Windows, but the package
> must still be present in the workspace during the build. Ensure you run `bun install`
> from the repository root before building.

# Linux (AppImage) local build

From `apps/desktop`:

```bash
bun run clean:dev
bun run compile:app
bun run package -- --publish never --config electron-builder.ts
```

Expected outputs in `apps/desktop/release/`:

- `*.AppImage`
- `*-linux.yml` (Linux auto-update manifest)

# Linux auto-update verification (local)

From `apps/desktop` after packaging:

```bash
ls -la release/*.AppImage
ls -la release/*-linux.yml
```

If both files exist, packaging produced the Linux artifact + updater metadata that `electron-updater` expects.