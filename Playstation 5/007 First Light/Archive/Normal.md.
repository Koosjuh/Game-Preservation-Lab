# 007 First Light - Startup Network Analysis Baseline

## Test Metadata

| Field          | Value                                                 |
| -------------- | ----------------------------------------------------- |
| Game Title     | 007 First Light                                       |
| Platform       | PlayStation 5                                         |
| Test Scenario  | Regular startup while connected to the internet       |
| Install State  | Fresh installation                                    |
| Capture Date   | 2026-05-27                                            |
| Capture File   | `2026_05_27_007FirstLight_RegularStartup_Online.pcap` |
| Capture Method | Gateway-level tcpdump capture                         |
| PS5 IP Address | `192.168.x.x`                                         |
| Capture Scope  | Full PlayStation 5 traffic during startup             |
| Network State  | Fully online                                          |
| Test Objective | Establish online startup baseline behavior            |

---

# Test Methodology

## Environment

The PlayStation 5 was connected normally to the internet through a segmented home lab environment. Traffic was captured directly on the network gateway using `tcpdump`.

The game had been freshly installed prior to testing.

No blocking, DNS manipulation, packet filtering, or offline simulation was active during this baseline capture.

---

## Capture Command

```bash id="iea7lu"
tcpdump -i [Redacted] host 192.168.x.x -s 0 -w /tmp/2026_05_27_007FirstLight_RegularStartup_Online.pcap
```

### Capture Parameters

| Parameter          | Purpose                                    |
| ------------------ | ------------------------------------------ |
| `-i [Redacted]`    | Capture on PlayStation network bridge/VLAN |
| `host 192.168.x.x` | Restrict capture to PlayStation traffic    |
| `-s 0`             | Capture full packets                       |
| `-w`               | Write directly to pcap file                |

---

# High-Level Traffic Summary

| Metric                           | Observation |
| -------------------------------- | ----------- |
| Total Packets                    | 1363        |
| Unique IPs                       | 24          |
| Dominant Protocol                | HTTPS/TLS   |
| DNS Activity                     | Present     |
| PlayStation Network Connectivity | Confirmed   |
| Third-Party Cloud Infrastructure | Confirmed   |

The majority of traffic observed consisted of encrypted HTTPS communications over TCP 443.

---

# Observed Domains and Services

## PlayStation Network Infrastructure

Observed domains:

```text id="lk0x9v"
ena.net.playstation.net
ps5.np.playstation.net
sgst.prod.dl.playstation.net
uef.np.dl.playstation.net
uds-zpln-ps5.np.rds.s0.playstation.net
ps5cel.np.dl.playstation.net
asm.np.community.playstation.net
image.api.playstation.com
```

### Assessment

Confirmed behavior:

* PlayStation Network communication occurs during startup.
* PlayStation CDN and backend infrastructure are contacted.
* Community and API-related traffic was observed.

Reasonable inference:

These services likely support:

* entitlement validation
* patch/update management
* account/session services
* backend API communication
* social/community functionality

Confidence Level: High

---

## Game Backend Infrastructure

Observed domains:

```text id="uoh9v7"
kconf.ioi-cloud.com
kauth.ioi-cloud.com
iokntprod01.ioi-cloud.com
knteventsprod.ioi-cloud.com
```

### Assessment

Confirmed behavior:

The game communicates with external backend infrastructure operated under the `ioi-cloud.com` domain.

Reasonable inference based on naming conventions:

| Domain                        | Likely Function                |
| ----------------------------- | ------------------------------ |
| `kauth.ioi-cloud.com`         | Authentication / authorization |
| `kconf.ioi-cloud.com`         | Runtime configuration          |
| `knteventsprod.ioi-cloud.com` | Telemetry / analytics          |
| `iokntprod01.ioi-cloud.com`   | Backend API/service layer      |

Important limitation:

TLS payloads were encrypted and not decrypted during analysis. Exact API functionality cannot be confirmed from this capture alone.

Confidence Level: Medium-High

---

## Azure Blob Storage Infrastructure

Observed domains:

```text id="1spg2z"
iokntprod01storage.blob.core.windows.net
kntprodconfigstorage.blob.core.windows.net
```

### Assessment

Confirmed behavior:

The game accesses Microsoft Azure Blob Storage infrastructure during startup.

Reasonable inference:

These storage systems may host:

* configuration files
* feature flags
* runtime metadata
* event configuration
* live-service content

Potential preservation relevance:

If startup logic depends on remotely hosted configuration data, long-term server availability may influence future playability.

Confidence Level: Medium

---

# Technical Analysis

## HTTPS/TLS Dependency

The startup sequence relied heavily on encrypted HTTPS communications over TCP 443.

Confirmed behavior:

* multiple outbound TLS sessions established
* repeated backend communication observed
* startup involved cloud infrastructure interaction

Important limitation:

The encrypted payload contents were not visible within the packet capture.

Therefore:

* exact request contents
* account data
* entitlement payloads
* telemetry contents

could not be confirmed.

---

## Authentication Indicators

Observed:

```text id="4zrf5n"
kauth.ioi-cloud.com
ps5.np.playstation.net
```

Reasonable inference:

The startup sequence likely performs:

* account validation
* entitlement verification
* backend session establishment
* online service initialization

Important scientific distinction:

This capture DOES NOT prove the game requires these services to function.

It only proves they are contacted during normal online startup.

Offline dependency still requires dedicated offline testing.

Confidence Level: Medium

---

## Telemetry and Analytics Indicators

Observed:

```text id="akl7nr"
knteventsprod.ioi-cloud.com
```

Reasonable inference:

Telemetry/event analytics systems appear active during startup.

Important distinction:

Telemetry presence alone does not indicate mandatory online dependency.

Many games transmit analytics while remaining fully playable offline.

Confidence Level: Medium-High

---

## Remote Configuration Architecture

Observed:

```text id="98cg9k"
kconf.ioi-cloud.com
kntprodconfigstorage.blob.core.windows.net
```

Reasonable inference:

The game may use server-side runtime configuration systems.

Potential examples include:

* feature toggles
* event scheduling
* live tuning
* startup configuration
* online feature management

Potential preservation concern:

If startup functionality depends on remote configuration availability, future service shutdown may impact playability.

This remains unconfirmed until offline testing is completed.

Confidence Level: Medium

---

# Developer / Publisher Motivation Analysis

## Confirmed Technical Behavior

The game communicates with:

* PlayStation infrastructure
* cloud-hosted backend systems
* authentication-related endpoints
* telemetry/event systems
* remote configuration systems

during normal startup.

---

## Reasonable Inference

Potential motivations may include:

| Potential Motivation               | Evidence Basis                |
| ---------------------------------- | ----------------------------- |
| Telemetry / KPI collection         | `knteventsprod.ioi-cloud.com` |
| Runtime configuration management   | `kconf.ioi-cloud.com`         |
| Authentication/session handling    | `kauth.ioi-cloud.com`         |
| Cloud-hosted service architecture  | Azure Blob Storage usage      |
| Patch/update ecosystem integration | PlayStation CDN traffic       |

---

## Important Neutrality Statement

Current evidence does NOT prove:

* hard always-online DRM
* mandatory continuous connectivity
* gameplay lockout without internet

Those conclusions require additional testing.

---

## Preservation Implications

Current observations suggest the game uses a modern cloud-connected startup architecture.

Potential long-term preservation risks may include dependency on:

* authentication services
* remote configuration services
* publisher backend APIs
* cloud-hosted runtime data

However:

No conclusion can yet be made regarding whether gameplay itself remains functional offline.

---

# Final Notes

This packet capture should be retained as the baseline reference capture for:

* standard online startup behavior
* endpoint comparison
* offline differential analysis
* future blocked-endpoint testing
* preservation dependency mapping

Future offline captures should be compared directly against this baseline to identify:

* missing connections
* failed requests
* retry logic
* timeout behavior
* startup degradation patterns
