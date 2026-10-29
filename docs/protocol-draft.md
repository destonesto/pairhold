# Pairhold sync protocol: DRAFT v0

> **DRAFT v0.** This is an early sketch for discussion, not a specification. It has not been reviewed or implemented, and it makes no security claims. Details will change.

## Goals

- Sync personal tasks and notes between a user's devices.
- Use storage the user already owns, and treat it as untrusted.
- Use only existing primitives from libsodium. Define no new cryptography.

## Data model

Each collection of tasks or notes is an [Automerge](https://automerge.org/) document. Devices exchange Automerge changes and merge them locally. Conflict resolution follows Automerge's merge semantics.

## Encryption

All data is encrypted on the device before it is written to storage. Each document has a random symmetric document key. Every file is encrypted and authenticated with XChaCha20-Poly1305, as provided by libsodium. Storage only ever receives ciphertext.

## Storage layout

Storage is "dumb": it only has to read, write, list and delete files. It runs no Pairhold code.

- **Change files.** Every write is a new, append-only file. Existing files are never modified.
- **Content addressing.** Each file is named by the hash of its encrypted contents. The hash function is not chosen yet; libsodium's `crypto_generichash` (BLAKE2b) is a candidate.
- **Snapshots.** From time to time a device compacts many change files into one encrypted snapshot. Change files that the snapshot covers can then be deleted.

## Device pairing and recovery

- **Pairing.** A device that already holds the document key shows it in a QR code. The new device scans it. Anyone who can see or photograph the QR code also gets the key, so pairing should only happen in person.
- **Recovery kit.** The user can print the document key as a recovery kit. Anyone who holds the kit can read the data, so it must be stored like a house key.

## Metadata leakage

Encryption hides contents, but not file sizes, counts or timing. To reduce what storage can infer:

- **Size padding.** Plaintext is padded to fixed size buckets before encryption, for example with libsodium's `sodium_pad`.
- **Batched writes.** Changes are grouped and written together instead of one file per edit.

These measures reduce leakage; they do not remove it. Storage still sees when a device syncs and roughly how much data exists.

## Open questions

1. **Merging recurrences.** How should concurrent edits to a recurring task merge, for example when one device edits a single occurrence and another changes the whole series?
2. **Compaction with offline devices.** When is it safe to delete change files that a snapshot covers, if a device has been offline and may not have seen them yet?
3. **Key rotation after losing a device.** A lost device still holds the document key. How do we rotate to a new key and re-encrypt, and what can the lost device still read afterwards?
