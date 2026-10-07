# Historical Reader-Agent Release Provenance

## Purpose

This record preserves immutable provenance for reader-agent releases created in `83degrees/AdvNFC` before release authority moved to `83degrees/AdvNFC-Reader-Agent` under ASTV-322.

Historical `reader-agent-*` Git tags remain in `83degrees/AdvNFC`. Future reader-agent tags, GitHub Releases and Debian artefacts are owned only by this repository. Removal of the historical GitHub Release objects from AdvNFC is deferred to ASTV-323 after this migration is accepted.

## Recorded historical releases

| Release | Historical repository | Target branch at publication | Release state | Debian artefact | Size | Recorded SHA-256 |
| --- | --- | --- | --- | --- | --- | --- |
| `reader-agent-v0.1.0` | `83degrees/AdvNFC` | `main` | stable | no attached asset recorded | — | — |
| `reader-agent-v0.1.0-beta.2` | `83degrees/AdvNFC` | `beta` | prerelease | `advnfc-reader-agent_0.1.0-beta.2_all.deb` | 6438 bytes | `e3461f304e6cd9bc5fd42722759615d5c47279efca98530920d1b5cb2f2955c2` |
| `reader-agent-v0.1.0-beta.1` | `83degrees/AdvNFC` | `beta` | prerelease | `advnfc-reader-agent_0.1.0-beta.1_all.deb` | 6436 bytes | `41148e819d859059eaa7ba89467dcff0c91fa7c82080b4222f82613f717e5835` |

Historical release-object IDs were 395809996, 395890745 and 395818990 respectively. Historical beta asset IDs were 586400102 and 586238047.

This record preserves provenance only. It does not republish old package bytes in the new repository and does not authorise deletion of historical tags.
