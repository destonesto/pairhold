# Pairhold

Usable, accessible, end-to-end encrypted sync for personal tasks and notes, over storage you already own (local folder, WebDAV/Nextcloud, Google Drive). No vendor server.

> **Status: planning.** There is no code yet. Everything below describes what we intend to build, not what exists today.

## Why

Most task and note apps sync your data to the vendor's cloud, where it is readable by the vendor. You are asked to trust their policies and their security.

Privacy should be a property of the system, not a promise. With Pairhold, data is encrypted on your devices before it leaves them, and the storage you sync through only ever sees ciphertext.

## What

Pairhold is planned as two parts:

1. **A Kotlin library**, Android first. It is built on [Automerge](https://automerge.org/) for conflict-free sync and [libsodium](https://libsodium.org/) for encryption. Pairhold introduces no new cryptography: it combines existing, reviewed primitives.
2. **An open, accessible UX kit**, targeting [WCAG 2.2 AA](https://www.w3.org/TR/WCAG22/). It covers the flows where encrypted apps usually lose people:
   - pairing devices via QR code
   - a recovery kit
   - a device list
   - sync status
   - consent

See [docs/ROADMAP.md](docs/ROADMAP.md) for the planned milestones.

## Status

Planning. No code has been written yet. The design is not reviewed and nothing here is ready to protect real data.

## Security

See [SECURITY.md](SECURITY.md).

## License

[Apache-2.0](LICENSE). See also [NOTICE](NOTICE).
