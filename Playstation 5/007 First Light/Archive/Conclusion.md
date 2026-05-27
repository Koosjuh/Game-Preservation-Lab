# 007 First Light - Corrected Preservation Status Analysis

## Scope

This document consolidates all completed testing scenarios for the PlayStation 5 version of 007 First Light.

This revision corrects the earlier timeline and clearly separates:

* fully online baseline installation/startup
* offline booting of an already-installed game
* fully fresh offline reinstall-from-disc testing

All conclusions below are based strictly on:

* directly observed gameplay/install behavior
* packet captures
* DNS activity
* TCP connection behavior
* observed installation state
* observed playable content state

No unsupported assumptions are made.

---

# Test Timeline

| Test   | Scenario                                                  | Internet | Installed Before Test |
| ------ | --------------------------------------------------------- | -------- | --------------------- |
| Test 1 | Fully online install + startup baseline                   | Yes      | No                    |
| Test 2 | Previously installed game booted offline with future date | No       | Yes                   |
| Test 3 | Fresh reinstall from disc fully offline with future date  | No       | No                    |

---

# Test 1 - Fully Online Install and Startup Baseline

## Capture

```text id="nvtfg7"
2026_05_27_007FirstLight_RegularStartup_Online.pcap
```

---

# Test 1 - Conditions

| Field         | Value                                     |
| ------------- | ----------------------------------------- |
| Internet      | Available                                 |
| PS5 Date      | Normal                                    |
| Install State | Fresh install                             |
| Goal          | Establish normal online behavior baseline |

---

# Test 1 - Packet Capture Findings

## Confirmed Domains Contacted

Observed directly:

```text id="u2vbr9"
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

---

# Test 1 - Confirmed Network Behavior

Observed:

* successful outbound TCP handshakes
* successful TLS establishment
* PlayStation infrastructure communication
* IO Interactive cloud communication
* CDN communication
* telemetry infrastructure communication

This proves:

```text id="zjiy9g"
During fully online installation/startup, the game communicates with both PlayStation infrastructure and IO Interactive infrastructure.
```

---

# Test 1 - Installation State

Observed:

* full game installation completed
* all missions playable
* game functioned normally online

Version observed:

```text id="kkz4d9"
1.0
```

---

# Test 2 - Previously Installed Game Booted Offline With Future Date

## Captures

```text id="kjlwm7"
2026_05_27_007FirstLightOfflinechangedate.pcap
2026_05_27_007FirstLightOfflinechangedate2.pcap
```

---

# Test 2 - Conditions

| Field                      | Value                                                        |
| -------------------------- | ------------------------------------------------------------ |
| Game Installed Before Test | Yes                                                          |
| Install Origin             | Fully online install from Test 1                             |
| Internet                   | Blocked                                                      |
| Gateway Access             | Available                                                    |
| DNS                        | Available                                                    |
| PS5 Date                   | Advanced approximately +1 year                               |
| Goal                       | Determine survivability of previously installed game offline |

---

# Test 2 - Observed Startup Behavior

Observed directly:

* splash screen displayed
* “Please Wait” loading bar displayed
* prolonged apparent stall
* no entitlement denial
* no internet-required popup
* eventual successful startup

Approximate observed startup delay:

```text id="3mlybt"
~15 minutes
```

No exact packet-derived startup duration was measured in this test.

---

# Test 2 - Gameplay State

Critically important:

```text id="t9hjhx"
ALL missions were playable offline in Test 2.
```

This is because:

```text id="hrd7g7"
The game had already been fully installed online during Test 1 before internet was removed.
```

This materially changes the preservation interpretation.

---

# Test 2 - Packet-Level Findings

## Confirmed DNS Queries

```text id="i4czvq"
ps5.np.playstation.net
qgve.dl.playstation.net
telemetry-console.api.playstation.com
appinfo.dl.playstation.net
envelope2.np.dl.playstation.net
ppr-crl.rnps.dl.playstation.net
```

---

# Test 2 - Confirmed Connection Behavior

Observed:

* repeated outbound TCP SYN packets
* repeated retry attempts
* repeated DNS lookups
* NO successful outbound TCP handshakes
* NO successful TLS sessions

This proves:

```text id="k6xy8h"
The PlayStation 5 repeatedly attempted internet communication but never established successful external connectivity.
```

---

# Test 2 - Most Important Finding

Despite:

* unavailable internet
* future-date manipulation
* failed external connectivity

the previously installed game:

```text id="sfkzdy"
Eventually fully booted offline and all missions remained playable.
```

This is a major preservation-positive finding.

---

# Test 2 - Preservation Interpretation

The evidence supports:

```text id="q1xj4v"
A previously fully installed copy of 007 First Light remains fully playable offline after prolonged retry/timeout behavior.
```

However:

Observed startup degradation remained severe.

---

# Test 3 - Fresh Offline Reinstall From Disc

## Captures

```text id="jhn10n"
2026_05_27_PS5BootUp_007FirstLightNoInternetInstall.pcap
2026_05_27_PS5BootUp_007FirstLightNoInternetInstall2.pcap
```

---

# Test 3 - Conditions

| Field                      | Value                                         |
| -------------------------- | --------------------------------------------- |
| Game Installed Before Test | No                                            |
| Disc Ejected Prior         | Yes                                           |
| PS5 Rebooted               | Yes                                           |
| Internet                   | Blocked                                       |
| Gateway Access             | Available                                     |
| DNS                        | Available                                     |
| PS5 Date                   | Future date still active                      |
| Goal                       | Determine fresh offline install survivability |

---

# Test 3 - Exact Procedure

Sequence:

1. Game fully uninstalled
2. Disc ejected
3. PlayStation 5 rebooted
4. Local/offline login used
5. Network capture started
6. Disc inserted
7. Internet remained unavailable

This simulates:

```text id="8j8grl"
A future scenario where a user attempts to install the game years later on a fresh PlayStation 5 without PlayStation infrastructure access.
```

---

# Test 3 - Initial Install Behavior

Observed directly:

* disc icon continuously spun
* installation did NOT immediately begin
* no install progress initially visible
* prolonged waiting occurred

This behavior persisted for an extended period.

---

# Test 3 - Critical Install Finding

At the end of the first capture:

```text id="t2p8dg"
The install/copy process finally began.
```

This is extremely important.

The earlier conclusion that offline installation “failed” was incorrect.

The evidence actually demonstrates:

```text id="8lx3d9"
Fresh offline installation eventually proceeds after prolonged retry/timeout behavior.
```

---

# Test 3 - Installation State

Observed:

```text id="gm3t8t"
45.9 GB installed/copied from disc.
```

This directly confirms:

```text id="i4o0m4"
Substantial game data exists on the physical disc itself.
```

---

# Test 3 - Startup Timeline After Install

Observed:

| Time        | Event             |
| ----------- | ----------------- |
| ~22:02      | Startup initiated |
| ~22:18      | Game booted       |
| ~16 minutes | Startup delay     |

This timing was directly observed by the tester.

---

# Test 3 - Gameplay State After Fresh Offline Install

Critically important:

```text id="uyzqvn"
ONLY the first mission was playable.
```

The game indicated:

```text id="hs3qnu"
The full game needed to finish downloading/installing.
```

This is the single most important preservation limitation identified in the project.

---

# Test 3 - Version Observation

Observed directly:

| Scenario              | Version |
| --------------------- | ------- |
| Fully online install  | 1.0     |
| Fresh offline install | 0.1.0   |

No unsupported interpretation should be made regarding:

* metadata state
* partial installation state
* placeholder versioning
* disconnected version logic

The captures cannot determine this.

---

# Test 3 - Packet-Level Findings

## Confirmed DNS Queries

```text id="iglrk6"
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

# Test 3 - Confirmed Connection Behavior

Observed:

* repeated SYN retries
* repeated DNS lookups
* NO successful external TCP handshakes
* NO successful TLS sessions

This proves:

```text id="v9b0kp"
The fresh install process repeatedly attempted PlayStation infrastructure communication while internet remained unavailable.
```

---

# Most Important Overall Findings

## Confirmed

The combined evidence definitively shows:

* the game strongly prefers internet connectivity
* startup/install retry behavior is extremely aggressive
* PlayStation infrastructure is heavily contacted
* offline fallback behavior exists
* fully installed copies remain fully playable offline
* fresh offline installation eventually succeeds
* fresh offline installation results in incomplete gameplay availability

---

# Critical Preservation Distinction

The evidence now clearly supports TWO different preservation outcomes:

## Scenario A - Previously Installed Game

```text id="lf1jlwm"
Fully playable offline after prolonged startup delay.
```

---

## Scenario B - Fresh Offline Install Years Later

```text id="ztcjlwm"
Installation eventually succeeds, but only partial gameplay availability exists.
```

This distinction is critically important.

---

# Preservation Interpretation

## Preservation-Positive Findings

Confirmed:

* substantial game data exists on disc
* previously installed copies remain fully playable offline
* no hard always-online DRM lockout observed
* offline fallback behavior exists
* startup/install eventually proceed offline

---

# Preservation-Negative Findings

Observed:

* severe retry/timeout delays
* internet-first startup/install assumptions
* heavy PlayStation infrastructure dependency attempts
* degraded offline usability
* fresh offline install only exposes first mission

---

# Final Preservation Classification

| Area                                     | Assessment            |
| ---------------------------------------- | --------------------- |
| Previously Installed Offline Playability | Fully functional      |
| Fresh Offline Installation               | Eventually functional |
| Fresh Offline Full Game Availability     | NOT confirmed         |
| Fresh Offline Partial Game Availability  | Confirmed             |
| Hard Always-Online Requirement           | Not observed          |
| Startup/Install Degradation              | Severe                |
| Consumer Friendliness                    | Poor                  |
| Preservation Risk                        | Medium-High           |

---

# Final Technical Conclusion

The packet captures consistently show both startup and installation repeatedly attempting communication with PlayStation platform infrastructure before eventually transitioning into degraded offline-capable behavior.

The captures definitively prove:

* repeated DNS activity
* repeated outbound HTTPS retry attempts
* persistent startup/install timeout behavior
* no successful internet connectivity during offline tests

Most importantly:

```text id="xzwk7r"
A previously fully installed copy of the game remains fully playable offline.
```

However:

```text id="ckm8tk"
A fresh offline reinstall years later currently only exposes the first mission after installation.
```

The evidence therefore supports the conclusion that:

```text id="hjlwm1"
007 First Light is not strictly always-online, but fresh long-term offline reinstall survivability appears degraded and currently does not demonstrate confirmed full-game offline accessibility from disc alone.
```
