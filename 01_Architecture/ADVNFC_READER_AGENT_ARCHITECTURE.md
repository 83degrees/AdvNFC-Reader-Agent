# AdvNFC Reader Agent Architecture

## 1. Status and authority

This document defines the approved target product boundary established through ASTV-318/ASTV-321 for the repository split.

ASTV-321 establishes the repository and boundary only. ASTV-322 is responsible for transferring authoritative reader-agent implementation, packaging, tests, deployment material and provider-contract ownership from `83degrees/AdvNFC`.

Until ASTV-322 is accepted, the current runtime/source authority remains the existing AdvNFC repository.

## 2. Product responsibility

AdvNFC Reader Agent owns the Raspberry Pi OS software boundary from physical NFC UID acquisition through publication of the governed reader event.

The intended runtime flow is:

```text
NFC reader hardware / libnfc
        |
        | UID acquisition
        v
advnfc-reader-agent
        |
        | existing suppression/reset and reader identity behaviour
        v
governed reader event
        |
        | MQTT transport
        v
AdvNFC / Home Assistant consumer path
```

The product does not own the downstream Home Assistant integration, tag mapping, intent mapping/routing, MQTT broker, Raspberry Pi OS, libnfc implementation or NFC reader hardware.

## 3. Deployment boundary

The single deployable unit is:

`rpi_os_software → deb`

Target authoritative runtime source:

`04_Implementation/rpi_os/source/**`

Target Debian packaging/build machinery:

`04_Implementation/rpi_os/packaging/deb/**`

Package-owned payload mirrors installed filesystem ownership. Node-local `/etc/advnfc/**` selectors, profiles, credentials and secrets remain outside package ownership.

## 4. Identity preservation

The repository split is not a runtime redesign.

The following identities are preserved through migration:

- Debian package: `advnfc-reader-agent`
- systemd service: `advnfc-reader-agent.service`
- existing reader-event semantics and compatibility
- existing package/runtime filesystem identities where currently package-owned

## 5. Contract boundary

The reader-event MQTT interface is provided by the reader-agent product and consumed by AdvNFC/Home Assistant.

Provider ownership of the existing contract transfers to this repository under ASTV-322 without changing its semantics merely because the repository boundary changes.

AdvNFC must consume the provider-owned contract rather than retain an authoritative duplicate after migration.

## 6. Release boundary

Future reader-agent GitHub Releases and `.deb` artefacts belong only to `83degrees/AdvNFC-Reader-Agent`.

Historical `reader-agent-*` Git tags remain in `83degrees/AdvNFC` for provenance. Historical GitHub Release objects in AdvNFC are retired only after ASTV-322 has preserved their provenance and ASTV-323 performs the verified cleanup.

## 7. Bootstrap state

ASTV-321 intentionally contains no migrated reader-agent runtime payload.

The canonical source and packaging paths may be represented by non-runtime bootstrap pointers, but the actual product transfer is gated to ASTV-322.
