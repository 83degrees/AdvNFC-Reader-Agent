# PROJECT_PROFILE: AdvNFC Reader Agent

## Profile conformance

This profile defines the governed product identity and current authority for the `AdvNFC Reader Agent` repository after the controlled reader-agent migration under ASTV-322.

## Document status

- Governance state: current
- Product runtime state: authoritative reader-agent source, packaging, tests, deployment guidance and provider contract are owned by this repository; deployed production state remains unchanged until a separately authorised package deployment

## Product identity

- Product name: AdvNFC Reader Agent
- Repository: `83degrees/AdvNFC-Reader-Agent`
- Repository visibility: public
- DDR origin code: `07`

## Linear work routing

- Default Linear team: `ASTV`

## Purpose

AdvNFC Reader Agent owns the Raspberry Pi OS software that acquires NFC UIDs from supported reader hardware and publishes the governed reader-event output consumed downstream by AdvNFC/Home Assistant.

## Scope

### In scope

- Raspberry Pi NFC reader-agent runtime software.
- NFC UID acquisition through the existing reader/platform interface.
- Existing same-card suppression/reset behaviour.
- Reader identity and construction/publication of the governed reader-event output.
- systemd/udev/runtime support required by the reader agent.
- Debian packaging, release, installation, upgrade and rollback for the reader agent.
- Reader-agent tests, deployment guidance and production evidence.
- Provider ownership of the reader-event MQTT interface after ASTV-322 migration completes.

### Out of scope

- Home Assistant AdvNFC custom integration.
- AdvNFC Home Assistant packages, automations, scripts or tag mapping.
- UID-to-intent mapping or downstream intent routing.
- MQTT broker/network infrastructure.
- Raspberry Pi OS ownership.
- libnfc implementation.
- NFC reader hardware.
- Redesign of the existing reader protocol, package identity or service identity as part of the repository split.

## Ownership and boundaries

| Boundary or capability | Relationship | Owner | Notes |
| --- | --- | --- | --- |
| Reader-agent runtime software | owned | AdvNFC Reader Agent | Authoritative reader-agent source is owned here following ASTV-322. |
| Debian package and release route | owned | AdvNFC Reader Agent | Package remains `advnfc-reader-agent`. |
| Reader-agent systemd service | owned | AdvNFC Reader Agent | Service identity remains `advnfc-reader-agent.service`. |
| Reader-event MQTT interface | provided | AdvNFC Reader Agent | Provider authority is owned here; ASTV-322 transferred ownership without changing interface semantics. |
| AdvNFC Home Assistant integration/configuration | external consumer | AdvNFC | Remains in `83degrees/AdvNFC`. |
| MQTT broker/network transport | external | Infrastructure owner | Product publishes configured topics but does not own transport. |
| Raspberry Pi OS / libnfc / ACR122U hardware | external | Platform/hardware owners | Product owns the agent software only. |
| Node-local `/etc/advnfc/**` state | external mutable state | Operator/environment | Profiles, selectors and secrets are not package-owned repository source. |

## Approved architecture location

- Approved architecture location: `01_Architecture/ADVNFC_READER_AGENT_ARCHITECTURE.md`
- Architecture state: current
- Material DDRs: None at bootstrap

## Contracts provided

| Contract | Status/version | Authoritative provider-owned location | Consumers | Notes |
| --- | --- | --- | --- | --- |
| `ADVNFC_READER_EVENT_MQTT_INTERFACE.md` | 3.0.0 candidate | `03_Contracts/ADVNFC_READER_EVENT_MQTT_INTERFACE.md` | AdvNFC / Home Assistant reader-event path | This repository is the authoritative provider location after ASTV-322. |

## Contracts consumed

None currently defined at the product boundary.

## Product dependencies

| Dependency | Type | Owner | Governed interface/evidence | Required state | Failure boundary |
| --- | --- | --- | --- | --- | --- |
| Raspberry Pi OS | platform | Platform owner | Deployment/runtime evidence | Supported Debian/Raspberry Pi OS runtime available | Reader agent cannot run. |
| libnfc / supported NFC reader | platform/hardware | Platform/hardware owners | Runtime evidence | Reader can acquire NFCID1 | UID acquisition fails. |
| MQTT broker / network transport | external service | Infrastructure owner | Reader-event MQTT contract plus runtime evidence | Configured broker/topic reachable | Reader event cannot be published. |
| AdvNFC | consuming product | AdvNFC | Reader-event MQTT contract | Consumer path available | Publication can succeed but downstream handling is unavailable. |

## Implementation namespace / naming identity

- Implementation namespace / naming identity: `advnfc-reader-agent`

Existing deployed identities are deliberately retained:

- Debian package: `advnfc-reader-agent`
- systemd service: `advnfc-reader-agent.service`
- runtime support naming beneath `/opt/advnfc/**`, `/usr/local/sbin/**`, `/usr/share/advnfc/**`, and applicable systemd/udev paths

Repository separation does not itself rename runtime identities.

## Deployable units

| Unit | Deployment type | Authoritative source | Target | Mechanism | Release/update route | Validation route | Rollback/recovery identity |
| --- | --- | --- | --- | --- | --- | --- | --- |
| AdvNFC Raspberry Pi reader agent | `rpi_os_software` | `04_Implementation/rpi_os/source/**` | Raspberry Pi OS package-owned filesystem paths | `deb` | versioned `.deb` GitHub Release from this repository | repository tests, package validation and target runtime evidence | prior known-good versioned `.deb` |

Debian build/package machinery is owned at `04_Implementation/rpi_os/packaging/deb/**`. Reader-agent tests are maintained under `05_Tests/**`, the deployment runbook under `08_Deployment/**`, and future reader-agent package releases originate only from this repository.

## Production and evidence route

- Production route: versioned Debian packages are built and released from this repository and installed on governed Raspberry Pi node(s) only with separate deployment authority. Existing deployed nodes are not changed by the repository migration itself.
- Evidence route: Git/GitHub for exact source/release identity plus target runtime evidence recorded against the governing Linear issue.
- Secrets and mutable-state boundary: credentials, selectors, profiles and other node-local `/etc/advnfc/**` state remain outside package ownership and governed repository source.
- Validation evidence route: repository checks, Debian package validation and deployment/runtime evidence recorded in Linear.
