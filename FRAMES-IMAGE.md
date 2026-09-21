# Frames development image

This fork publishes Nethermind's `eip8141-frame-txs-devnet7` implementation to
`ghcr.io/wevm/nethermind-frames` for client-library integration testing.
The initial upstream revision is `d5bb1ad7d2c955925fb38708f2fee915144fe46a`.

## Publish

Run the **Publish frames image** workflow manually on
`eip8141-frame-txs-devnet7`. It builds `linux/amd64` and `linux/arm64` using the
upstream Dockerfile, checks `--version` on each native runner, and publishes
the full source commit SHA and `latest` tags after both checks pass.

```sh
gh workflow run publish-frames.yml --repo wevm/nethermind --ref eip8141-frame-txs-devnet7
```

The workflow uses `GITHUB_TOKEN` with `packages: write`; no registry secret is
required. The GHCR package must be public for anonymous pulls.

## Use

Pin the full commit tag or image digest in CI. `latest` changes on each successful
manual publication. A tag can be republished; a digest pins the exact image.

```sh
docker run --rm ghcr.io/wevm/nethermind-frames:latest --version
```

The image contains the client binary and upstream configuration files. Integration
tests must supply a frame-enabled chainspec, funded accounts, and block production.
The smoke check verifies startup, not transaction execution or spec conformance.

## Update

Merge the upstream `eip8141-frame-txs-devnet7` branch into this fork's matching
branch, review the changes, and dispatch the publishing workflow. Upstream changes
do not publish automatically. The fork's other inherited workflows are disabled.
