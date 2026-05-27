# 007 First Light - Offline Startup Analysis With Internet Isolation and Future Date Manipulation

## Test Metadata

| Field              | Value                                                                              |
| ------------------ | ---------------------------------------------------------------------------------- |
| Game Title         | 007 First Light                                                                    |
| Platform           | PlayStation 5                                                                      |
| Test Scenario      | Offline startup with gateway access but no internet                                |
| Install State      | Fresh installation                                                                 |
| System Clock       | Advanced approximately 1 year into the future                                      |
| Network State      | Local gateway reachable, outbound internet blocked                                 |
| Capture Files      | `2026_05_27_007FirstLightOfflinechangedate.pcap`                                   |
| Additional Capture | `2026_05_27_007FirstLightOfflinechangedate2.pcap`                                  |
| Capture Method     | Gateway-level tcpdump capture                                                      |
| Test Objective     | Determine startup behavior and offline survivability without internet connectivity |

---

# Test Conditions

The PlayStation 5 remained connected to the local network infrastructure while outbound internet access was intentionally blocked at the gateway level.

This allowed:

* DHCP communication
* local gateway communication
* DNS resolution attempts
* packet capture visibility

while preventing:

* PlayStation Network connectivity
* telemetry uploads
* authentication communication
* external HTTPS communication

Additionally:

* the PlayStation 5 system clock was manually advanced approximately one year into the future prior to startup testing

The game box and in-game messaging indicate:

```text
Internet required for installation.
Game can be played offline.
Better experience online.
```

---

# Observed User-Facing Behavior

## Startup Sequence

The game launched and displayed:

* splash/startup screen
* PlayStation loading environment
* “Please Wait” loading bar

The game then appeared stalled for approximately 15 minutes.

No immediate:

* error message
* disconnect prompt
* entitlement denial
* “internet required” popup

was observed.

---

## Final Outcome

After approximately 15 minutes:

* the startup process completed
* the game successfully booted
* gameplay became available
* offline play was possible

This is a major preservation-relevant finding.

---

# Packet Capture Findings

## Capture Summary

| Capture                            | Observation                        |
| ---------------------------------- | ---------------------------------- |
| `...changedate.pcap`               | Initial offline startup attempts   |
| `...changedate2.pcap`              | Offline retry/startup continuation |
| Packets observed                   | Hundreds of packets                |
| DNS activity                       | Confirmed                          |
| HTTPS connection attempts          | Confirmed                          |
| Successful outbound HTTPS sessions | Not observed                       |

---

# Directly Observed DNS Queries

The PlayStation 5 resolved the following domains during startup:

```text
ps5.np.playstation.net
qgve.dl.playstation.net
telemetry-console.api.playstation.com
appinfo.dl.playstation.net
envelope2.np.dl.playstation.net
ppr-crl.rnps.dl.playstation.net
```

These are confirmed observations directly from the packet captures.

---

# Directly Observed Connection Attempts

The PlayStation 5 repeatedly attempted outbound TCP 443 HTTPS communication to multiple external systems, including Akamai and Microsoft-hosted infrastructure.

Observed examples include:

```text
e40436.api8.akamaiedge.net
e274.d.akamaiedge.net
e7320.dscb.akamaiedge.net
e16671.d.akamaiedge.net
a1092.d.akamai.net
20.105.216.1
```

---

# Critical Technical Observation

## No Successful External HTTPS Sessions Observed

The captures show:

* repeated outbound SYN packets
* repeated retry attempts
* failed outbound connection establishment

The captures do NOT show:

* successful TCP handshakes
* completed TLS sessions
* successful external HTTPS communication

This confirms:

```text
The PlayStation 5 repeatedly attempted to reach online infrastructure but could not establish internet connectivity.
```

---

# What The Game Was Actually Doing

Based directly on the packet captures:

## Confirmed Behavior

The startup process was repeatedly attempting to:

* resolve PlayStation-related domains
* establish outbound HTTPS connectivity
* reach PlayStation CDN/API infrastructure
* reach telemetry-related endpoints

while internet access remained unavailable.

---

# Most Likely Cause Of The 15-Minute Delay

The packet captures strongly indicate:

```text
The game and/or PlayStation startup environment was waiting for repeated online connection attempts and timeout/retry logic to complete before transitioning into offline-capable operation.
```

Evidence supporting this:

| Observation                  | Relevance                   |
| ---------------------------- | --------------------------- |
| Repeated DNS lookups         | Startup dependency attempts |
| Repeated SYN retransmissions | Retry behavior              |
| No successful TCP handshake  | Internet unavailable        |
| Eventual successful boot     | Offline fallback exists     |

---

# Important Correction Regarding Telemetry

Earlier analysis suggested telemetry/privacy-policy systems may have caused the delay.

The captures do NOT directly prove this.

What IS directly visible:

```text
telemetry-console.api.playstation.com
```

This confirms:

* telemetry-related PlayStation infrastructure was contacted during startup attempts

However:

The captures do NOT prove:

* telemetry caused the delay
* privacy-policy synchronization blocked startup
* analytics systems were mandatory

Those conclusions would exceed the evidence.

---

# Impact Of The Future System Date

The system clock being advanced approximately one year into the future is highly relevant.

Potentially affected systems include:

| System Type                | Potential Impact   |
| -------------------------- | ------------------ |
| Cached session tokens      | Expired            |
| TLS certificate validation | Date mismatch      |
| Authentication sessions    | Invalidated        |
| Entitlement caching        | Timeout/expiration |
| Retry behavior             | Extended           |

However:

The packet captures alone cannot determine exactly which subsystem reacted to the future date.

What CAN be concluded:

```text
The game still eventually entered an offline-playable state despite future-date conditions and blocked internet connectivity.
```

---

# Preservation-Relevant Conclusions

## Extremely Important Finding

The game does NOT appear to enforce an immediate hard online lockout.

Instead:

* startup first attempts online communication
* repeated retry/timeout behavior occurs
* eventual offline fallback becomes available

This is significantly different from:

* always-online DRM enforcement
* server-authoritative lock systems
* hard entitlement denial models

---

# Preservation Assessment

## Positive Findings

Confirmed:

* the game eventually boots offline
* gameplay becomes possible without internet
* no permanent startup lockout occurred
* offline fallback behavior exists

---

## Negative Findings

Observed concerns:

* approximately 15-minute startup delay
* online-first startup architecture
* heavy dependency retry behavior
* poor offline startup experience

This indicates:

```text
The game was designed expecting internet availability during startup, even though offline fallback exists.
```

---

# Developer / Publisher Behavior Assessment

## Confirmed Technical Behavior

During startup, the PlayStation environment and/or game attempted to contact:

* PlayStation Network infrastructure
* PlayStation telemetry infrastructure
* CDN infrastructure
* certificate-related infrastructure

before eventually transitioning into offline-capable operation.
