# 007 First Light - Comprehensive Preservation Status Analysis

## Project

007 First Light Game Preservation Analysis

---

# Executive Summary

This analysis investigates whether the PlayStation 5 version of 007 First Light can be installed, launched, and played without internet connectivity, and whether long-term preservation risk exists if online infrastructure disappears.

Testing consisted of:

* fully online startup baseline captures
* offline startup testing
* future-date startup testing
* offline reinstall-from-disc testing
* repeated boot testing after installation
* packet capture analysis during all major phases

The evidence shows:

* the game DOES eventually become playable offline
* the game DOES NOT immediately hard-fail offline
* startup behavior is heavily internet-first
* fresh offline disc installation initially stalled for an extended period
* startup retry/timeout behavior is extremely aggressive
* the game repeatedly attempts PlayStation platform connectivity before transitioning into offline-capable operation

Most importantly:

```text
Fresh offline installation did eventually begin copying/installing after prolonged timeout behavior.
```

This substantially changes the preservation interpretation versus the earlier assumption that offline installation fully failed.

However:

The offline user experience remains severely degraded. With only the first mission playable.

---

# Test Environment

| Field                    | Value                               |
| ------------------------ | ----------------------------------- |
| Platform                 | PlayStation 5                       |
| Distribution             | Physical Disc                       |
| Network State            | Gateway reachable, internet blocked |
| Capture Method           | Gateway-level tcpdump               |
| DNS Availability         | Yes                                 |
| Outbound Internet        | Blocked                             |
| Install State            | Multiple scenarios tested           |
| System Time Manipulation | Approximately +2 years               |
| PS5 Login State          | Local/offline account session       |

---

# Test Sequence

## Test 1 - Fully Online Baseline Startup

### Capture

```text
2026_05_27_007FirstLight_RegularStartup_Online.pcap
```

### Observed Behavior

The game launched normally while fully connected to the internet.

### Observed Domains

Confirmed directly from packet captures:

```text
ena.net.playstation.net
ps5.np.playstation.net
sgst.prod.dl.playstation.net
uef.np.dl.playstation.net
uds-zpln-ps5.np.rds.s0.playstation.net
ps5cel.np.dl.playstation.net
asm.np.community.playstation.net
image.api.playstation.com
kconf.ioi-cloud.com
kauth.ioi-cloud.com
iokntprod01.ioi-cloud.com
knteventsprod.ioi-cloud.com
```

### Observed Infrastructure

Confirmed:

* PlayStation Network communication
* PlayStation CDN communication
* IO Interactive cloud infrastructure communication
* Azure Blob Storage usage
* telemetry/event infrastructure communication

### Important Technical Observation

Successful outbound HTTPS communication occurred during this baseline online startup.

Unlike later offline tests:

* TCP handshakes succeeded
* TLS sessions established successfully

---

# Test 2 - Offline Startup With Future Date Manipulation

## Test Conditions

* internet blocked
* gateway reachable
* DNS available
* system clock advanced approximately one year
* previously installed game

---

## Capture Files

```text
2026_05_27_007FirstLightOfflinechangedate.pcap
2026_05_27_007FirstLightOfflinechangedate2.pcap
```

---

# Observed Startup Behavior

## Timeline

| Time        | Event                        |
| ----------- | ---------------------------- |
| ~22:02      | Startup initiated            |
| ~22:18      | Game successfully booted     |
| ~16 minutes | Total observed startup delay |

### User-Facing Behavior

Observed directly:

* splash screen displayed
* “Please Wait” loading bar displayed
* no immediate error message
* no hard entitlement denial
* prolonged apparent stall
* eventual successful startup

After boot:

```text
Only the first mission was available.
The game stated the full game needed to finish downloading/installing.
```

This is critically important.

---

# Packet-Level Findings

## Directly Observed DNS Queries

```text
ps5.np.playstation.net
qgve.dl.playstation.net
telemetry-console.api.playstation.com
appinfo.dl.playstation.net
envelope2.np.dl.playstation.net
ppr-crl.rnps.dl.playstation.net
```

---

# Directly Observed Connection Attempts

Observed repeated outbound HTTPS attempts to:

```text
e40436.api8.akamaiedge.net
e274.d.akamaiedge.net
e7320.dscb.akamaiedge.net
e16671.d.akamaiedge.net
a1092.d.akamai.net
20.105.216.1
```

---

# Critical Packet Observation

The captures showed:

* repeated SYN packets
* repeated retry behavior
* repeated DNS lookups
* NO successful outbound TCP handshakes
* NO successful TLS session establishment

This proves:

```text
The PlayStation 5 repeatedly attempted online communication while internet access remained unavailable.
```

---

# What The Packet Captures Prove

The captures directly demonstrate:

* startup repeatedly attempts PlayStation infrastructure communication
* startup repeatedly attempts CDN/download infrastructure communication
* startup repeatedly retries unavailable services
* startup eventually falls back into offline-capable behavior

The captures do NOT prove:

* hard always-online DRM
* mandatory telemetry lockouts
* mandatory account validation
* mandatory IOI cloud validation

because:

* successful IOI cloud communication did not occur in offline tests
* the game eventually became playable offline

---

# Test 3 - Fresh Offline Disc Install Attempt

## Test Procedure

Sequence:

1. Game uninstalled
2. Disc ejected
3. PS5 fully powered down
4. Logged in locally
5. Network capture started
6. Disc inserted
7. Internet remained blocked

---

# Initial Install Behavior

Observed:

* disc icon continuously spun
* installation did NOT immediately begin
* no visible download progress
* no visible installation progress
* no immediate error

After prolonged waiting:

```text
The install/copy process eventually began.
```

This is extremely important.

---

# Packet Capture

## Capture File

```text
2026_05_27_PS5BootUp_007FirstLightNoInternetInstall.pcap
```

Additional capture:

```text
2026_05_27_PS5BootUp_007FirstLightNoInternetInstall2.pcap
```

---

# Directly Observed Domains During Install

```text
ps5.np.playstation.net
telemetry-console.api.playstation.com
envelope2.np.dl.playstation.net
ps5cel.np.dl.playstation.net
image.api.playstation.com
ppr-crl.rnps.dl.playstation.net
sgst.prod.dl.playstation.net
feature-discovery-assets-amd.dl.playstation.net
feature.api.playstation.com
psnobj.prod.dl.playstation.net
```

---

# Critical Installation Observation

The install captures again showed:

* repeated DNS resolution
* repeated outbound SYN retries
* NO successful outbound HTTPS sessions
* prolonged timeout/retry behavior

However:

```text
The game eventually began copying/installing from disc despite no successful internet connectivity.
```

---

# Installation State

Eventually observed:

```text
45.9 GB installed/copied
```

This strongly indicates:

```text
Substantial game content DOES exist on the disc itself.
```

This is preservation-positive.

---

# Versioning Observation

Observed:

| Scenario        | Reported Version |
| --------------- | ---------------- |
| Online install  | 1.0              |
| Offline install | 0.1.0            |

This is directly observed behavior.

No conclusion can currently be made regarding:

* whether this is placeholder versioning
* partial install state
* pre-release numbering
* disconnected metadata state

because packet captures alone cannot determine this.

---

# Post-Install Reboot Testing

After installation:

* game closed
* game reopened

Observed:

* “Please Wait” loading screen returned
* startup again appeared stalled
* game again attempted prolonged startup

This is critically important because it demonstrates:

```text
The prolonged startup behavior is NOT limited to first boot only.
```

---

# Preservation-Relevant Technical Interpretation

## What The Captures Definitively Show

### Confirmed

* PlayStation platform services are heavily contacted during startup/install
* repeated retry logic occurs when services unavailable
* startup/install continue only after prolonged timeout behavior
* offline fallback paths exist
* installation from disc eventually succeeds offline
* gameplay eventually becomes available offline

---

# What The Captures Do NOT Prove

The captures do NOT prove:

* mandatory always-online DRM
* mandatory telemetry lockouts
* mandatory IOI cloud authentication
* permanent internet requirement

because:

* successful internet sessions never occurred during offline tests
* game eventually became installable/playable offline

---

# Preservation Implications

## Positive Preservation Findings

Confirmed:

* substantial game data exists on disc
* offline installation eventually succeeds
* offline gameplay eventually succeeds
* no permanent online lockout observed
* no hard entitlement failure observed

These are highly preservation-positive findings.

---

## Negative Preservation Findings

Observed:

* approximately 15-16 minute startup delays
* approximately 15+ minute install delays before copying begins
* aggressive retry/timeout behavior
* internet-first startup assumptions
* degraded offline user experience
* repeated PlayStation platform dependency attempts

This is not consumer-friendly behavior.

---

# Most Important Preservation Finding

The strongest preservation conclusion from all captures combined is:

```text
007 First Light currently supports degraded offline operation rather than hard online enforcement.
```

Meaning:

* the game strongly prefers online connectivity
* startup/install logic heavily expects internet access
* but offline fallback behavior DOES exist

This fundamentally differs from:

* true always-online games
* server-authoritative games
* online entitlement lock systems

---

# Final Classification

| Area                           | Assessment                        |
| ------------------------------ | --------------------------------- |
| Offline Disc Installation      | Eventually successful             |
| Offline Boot Capability        | Confirmed                         |
| Offline Gameplay Capability    | Confirmed                         |
| Immediate Offline Usability    | Poor                              |
| Hard Always-Online Requirement | Not observed                      |
| Timeout/Retry Dependency       | Severe                            |
| Consumer Friendliness          | Poor                              |
| Preservation Risk              | Medium                            |
| Long-Term Survivability        | Currently acceptable but degraded |

---

# Final Conclusion

The packet captures consistently show the PlayStation 5 and game repeatedly attempting to contact PlayStation platform/CDN/telemetry infrastructure during startup and installation.

During offline testing:

* no successful outbound internet sessions occurred
* repeated retry behavior was observed
* startup/install entered prolonged waiting states
* eventual offline fallback behavior occurred

Most importantly:

```text
The game eventually installs from disc and eventually becomes playable offline despite unavailable internet connectivity. However only the first mission is playable. According to the game "Only the first mission is available. You need to download the full game to play."
```

The approximately 15-16 minute delays before installation/startup progression represent extremely degraded offline behavior and strongly indicate an internet-first startup/install architecture. Further backed by the 49.5GB installation for only the first mission in offline play.

Current evidence therefore supports the conclusion that:

```text
007 First Light is NOT strictly always-online, but its offline fallback implementation is severely degraded and highly dependent on lengthy timeout/retry behavior before functioning.
```
