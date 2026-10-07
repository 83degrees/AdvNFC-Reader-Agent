# AdvNFC Reader Agent Architecture

## 1. Status and authority

This document defines the current product boundary established through ASTV-318/ASTV-322. `83degrees/AdvNFC-Reader-Agent` is authoritative for the Raspberry Pi reader-agent implementation, Debian packaging, reader-specific tests, deployment guidance and the reader-event MQTT provider contract. The repository migration does not itself alter any deployed node.

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

Authoritative runtime source:

`04_Implementation/rpi_os/source/**`

Debian packaging/build machinery:

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

Provider ownership of the existing contract resides in this repository. ASTV-322 changed ownership only; it did not change the interface semantics. AdvNFC consumes the provider-owned contract and must not maintain an authoritative duplicate.

## 6. Release boundary

Future reader-agent GitHub Releases and `.deb` artefacts belong only to `83degrees/AdvNFC-Reader-Agent`.

Historical `reader-agent-*` Git tags remain in `83degrees/AdvNFC` for provenance. Historical GitHub Release objects in AdvNFC are retired only after ASTV-322 provenance capture is verified and ASTV-323 performs the governed cleanup.

## 7. Migration state

ASTV-322 transfers the existing reader-agent product without redesigning runtime behaviour. Existing production deployment remains unchanged until separately authorised deployment of a package released from this repository.
