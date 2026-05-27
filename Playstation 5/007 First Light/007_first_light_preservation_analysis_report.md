# 007 First Light Game Preservation Analysis

## Executive Summary

This report documents a forensic-style preservation and network dependency analysis of the PlayStation 5 title "007 First Light" using packet captures, controlled offline testing, reproducible installation scenarios, and direct gameplay observations.

The primary research objective was to determine whether the game remains installable, launchable, and fully playable when PlayStation Network infrastructure and internet connectivity are unavailable.

Three distinct scenarios were tested:

1. Fully online installation and startup baseline.
2. Previously installed game booted fully offline with the PlayStation 5 date advanced approximately one year into the future.
3. Completely fresh offline reinstall from physical disc with no previously installed copy present.

The most significant finding of the project is that the preservation outcome differs dramatically depending on whether the game had previously completed a successful online installation.

Observed results:

| Scenario | Result |
|---|---|
| Previously installed game booted offline | All missions remained playable offline after prolonged startup delay |
| Fresh offline reinstall from disc | Only the first mission was playable after installation completed |

The fresh offline reinstall scenario represents the most preservation-relevant outcome because it simulates a future condition where platform infrastructure may no longer exist.

The packet captures demonstrate extensive communication with PlayStation infrastructure and multiple IO Interactive cloud endpoints during online startup and installation activity.

Observed behaviors included:

- DNS resolution against PlayStation infrastructure
- TLS sessions against Sony and IO Interactive services
- Azure Blob Storage usage
- CDN-backed content delivery infrastructure
- Retry and timeout behavior during offline startup
- Delayed installation behavior when offline
- Partial gameplay availability after fresh offline reinstall

The evidence indicates that the physical disc contains substantial game content, confirmed by a successful offline installation of approximately 45.9 GB. However, the fresh offline reinstall behavior strongly suggests that either:

- additional content normally arrives through online infrastructure, or
- installation state validation/activation logic prevents full gameplay availability without prior online completion.

This report intentionally distinguishes:

- directly observed behavior
- technically reasonable interpretation
- unknown or unproven conclusions

No unsupported DRM accusations or speculative claims are made.

---

# Research Goals

The purpose of the 007 First Light Game Preservation Analysis project is to evaluate long-term survivability of modern console software under degraded infrastructure conditions.

Primary questions:

- Can the game be installed entirely offline from physical media?
- Can the game boot without PlayStation Network availability?
- Can all gameplay content remain accessible without online infrastructure?
- Does the title exhibit startup degradation when services are unavailable?
- Does prior online installation materially alter long-term survivability?
- What infrastructure dependencies exist during startup and installation?
- What preservation risks emerge if PlayStation infrastructure disappears in the future?

The project focuses specifically on:

- physical media survivability
- offline installation capability
- offline startup capability
- dependency on cloud infrastructure
- retry and timeout behavior
- long-term consumer ownership implications
- degradation of usability over time

The project intentionally avoids:

- emotional framing
- activist rhetoric
- unsupported legal claims
- unsupported DRM accusations
- assumptions without packet-level evidence

---

# Test Environment

## Hardware

- PlayStation 5
- Physical disc edition of 007 First Light

## Network Analysis Tooling

- Packet capture environment
- PCAP-based network forensic analysis
- DNS observation
- TCP session analysis
- TLS SNI extraction

## Test Variables

Variables manipulated during testing:

| Variable | Values |
|---|---|
| Internet connectivity | Online / Offline |
| Installation state | Previously installed / Fresh reinstall |
| System date | Current / Future date (+~1 year) |
| PlayStation login state | Local offline login |

---

# Methodology

## Core Methodology

The methodology focused on reproducible behavioral testing under controlled connectivity conditions.

Each scenario was isolated and analyzed independently.

Packet captures were collected during:

- startup
- installation
- offline retry conditions
- infrastructure lookup behavior

Gameplay accessibility observations were documented separately from packet analysis.

This distinction is important because network behavior alone cannot prove gameplay availability.

---

# Packet Capture Methodology

Packet captures were analyzed for:

- DNS queries
- TCP session attempts
- TLS Client Hello behavior
- SNI values
- endpoint enumeration
- infrastructure categorization
- retry behavior
- startup sequencing
- installation sequencing

The captures were treated similarly to malware/network forensic investigations.

Observed indicators included:

- endpoint category
- infrastructure ownership
- cloud provider usage
- CDN involvement
- retry intervals
- failed connection behavior
- sequencing between infrastructure systems

TLS payload decryption was not performed.

Therefore:

- application-layer payloads remain unknown
- exact API operations remain unproven
- infrastructure categorization is based on endpoint naming and observed behavior

---

# Network Isolation Methodology

Offline testing used complete internet disconnection conditions.

The goal was to simulate:

- future infrastructure loss
- PlayStation Network unavailability
- abandoned platform ecosystems
- long-term preservation conditions

The PlayStation 5 system date was manually advanced approximately one year into the future during offline testing.

This was intended to evaluate:

- certificate validation behavior
- retry persistence
- startup degradation
- dependency on remote validation systems
- survivability under future-state conditions

---

# Test Timeline

## Test 1

### Fully Online Baseline Install + Startup

PCAP:

- 2026_05_27_007FirstLight_RegularStartup_Online.pcap

Observed behavior:

- Full online installation completed
- Game launched successfully
- All missions playable
- Version observed: 1.0
- Game Size 50.89GB
- Multiple PlayStation and IO Interactive endpoints contacted

Purpose:

Establish baseline infrastructure and intended online behavior.

---

## Test 2

### Previously Installed Game Booted Offline with Future Date

PCAPs:

- 2026_05_27_007FirstLightOfflinechangedate.pcap
- 2026_05_27_007FirstLightOfflinechangedate2.pcap

Observed behavior:

- Previously installed game booted fully offline
- Startup delayed approximately 15 minutes
- Extended “Please Wait” behavior observed
- Connection attempts failed
- Retry/timeout behavior observed
- All missions remained playable offline

Important preservation distinction:

The game had already completed a successful online installation during Test 1.

This scenario does NOT represent a fresh reinstall.

---

## Test 3

### Fresh Offline Reinstall from Disc

PCAPs:

- 2026_05_27_PS5BootUp_007FirstLightNoInternetInstall.pcap
- 2026_05_27_PS5BootUp_007FirstLightNoInternetInstall2.pcap

Procedure:

1. Game fully uninstalled
2. Disc ejected
3. PlayStation 5 rebooted
4. Offline/local login used
5. Internet remained unavailable
6. Packet capture started
7. Disc inserted

Observed behavior:

- Disc icon continuously spun
- Installation did not immediately begin
- Prolonged waiting occurred
- Repeated retry behavior occurred
- Installation eventually started from disc
- Approximately 45.9 GB copied/installed
- Startup eventually succeeded
- Version observed: 0.1.0
- Only first mission playable
- Game indicated additional content/download required

This is the single most preservation-relevant result in the project.

---

# Baseline Online Install/Startup Analysis

## Observed Domains

The online baseline scenario contacted both PlayStation infrastructure and IO Interactive infrastructure.

### Observed PlayStation Domains

| Domain | Likely Category |
|---|---|
| asm.np.community.playstation.net | Community/account services |
| ena.net.playstation.net | Network platform services |
| image.api.np.km.playstation.net | Metadata/image delivery |
| image.api.playstation.com | Media/metadata services |
| ps5.np.playstation.net | Core PlayStation platform services |
| ps5cel.np.dl.playstation.net | Download/update services |
| sgst.prod.dl.playstation.net | Download/CDN infrastructure |
| smetrics.aem.playstation.com | Telemetry/analytics |
| uds-zpln-ps5.np.rds.s0.playstation.net | Platform backend service |
| uef.np.dl.playstation.net | Download infrastructure |

### Observed IO Interactive Domains

| Domain | Likely Category |
|---|---|
| iokntprod01.ioi-cloud.com | Game backend platform |
| kauth.ioi-cloud.com | Authentication-related service |
| kconf.ioi-cloud.com | Remote configuration service |
| knteventsprod.ioi-cloud.com | Event/telemetry infrastructure |
| iokntprod01storage.blob.core.windows.net | Azure Blob Storage |
| kntprodconfigstorage.blob.core.windows.net | Azure configuration storage |

---

# TLS/SNI Analysis

TLS SNI values were successfully extracted during the online baseline scenario.

Observed SNI values matched DNS observations.

This confirms that the PlayStation 5 attempted direct TLS communication with:

- PlayStation infrastructure
- IO Interactive infrastructure
- Azure-hosted backend storage
- telemetry systems

Examples:

| SNI | Infrastructure Category |
|---|---|
| kauth.ioi-cloud.com | Authentication/backend |
| kconf.ioi-cloud.com | Remote configuration |
| knteventsprod.ioi-cloud.com | Event telemetry |
| iokntprod01storage.blob.core.windows.net | Azure Blob Storage |
| smetrics.aem.playstation.com | Telemetry/analytics |

The presence of Azure Blob Storage strongly indicates cloud-hosted configuration or content distribution components.

Direct content semantics remain unknown because TLS payloads were not decrypted.

---

# Offline Previously Installed Startup Analysis

## Key Behavioral Findings

During Test 2:

- internet remained unavailable
- outbound connectivity failed
- startup delay persisted for approximately 15 minutes
- all missions remained playable

This is critically important.

The previously installed copy retained full gameplay accessibility despite offline conditions.

However, startup degradation was substantial.

Observed indicators suggest repeated attempts to contact PlayStation infrastructure despite complete lack of internet connectivity.

Observed domains during offline startup included:

| Domain |
|---|
| envelope2.np.dl.playstation.net |
| feature.api.playstation.com |
| ppr-crl.rnps.dl.playstation.net |
| ps5.np.playstation.net |
| telemetry-console.api.playstation.com |

This indicates that offline startup does not immediately bypass network logic.

Instead, the system appears to:

- attempt connectivity
- wait for failures/timeouts
- eventually fall back to offline startup

Preservation relevance:

Even if a game remains technically playable offline, prolonged retry behavior materially degrades usability and future accessibility.

A title that requires extended timeout periods before launching may become increasingly unreliable if infrastructure behavior changes.

---

# Fresh Offline Reinstall Analysis

## Most Significant Preservation Finding

The fresh offline reinstall scenario produced materially different results compared to the previously installed offline startup scenario.

Observed behavior:

| Observation | Result |
|---|---|
| Fresh install from disc | Successful |
| Internet connectivity | Unavailable |
| Disc installation began immediately | No |
| Installation delay observed | Yes |
| Retry behavior observed | Yes |
| Installed size | ~45.9 GB |
| Startup eventually succeeded | Yes |
| All missions playable | No |
| Only first mission playable | Yes |

This result strongly suggests that a fresh offline reinstall does not produce the same functional game state as a previously online-initialized installation.

---

# Installation State Analysis

## Confirmed Behavior

Confirmed observations:

- substantial game data exists on physical media
- approximately 45.9 GB successfully copied from disc
- game startup eventually succeeded offline
- only the first mission was accessible
- game indicated additional installation/download requirements

## Reasonable Interpretation

Possible interpretations supported by observed behavior:

- additional content may normally be retrieved online
- online initialization may unlock additional game state
- background content installation may not complete offline
- entitlement/configuration synchronization may influence accessible content

## Unknown/Unproven

The captures do NOT prove:

- mandatory always-online DRM
- remote mission streaming
- online-only entitlement enforcement
- deliberate disabling of offline gameplay

Those conclusions would require additional evidence.

---

# DNS Analysis

## DNS Activity Overview

Observed DNS activity differed significantly between tests.

| Test | DNS Query Volume |
|---|---|
| Online baseline | Moderate |
| Offline previously installed | Moderate retry-oriented |
| Fresh offline reinstall | High retry-oriented |

The fresh reinstall scenario generated the highest observed DNS activity.

This suggests persistent infrastructure lookup behavior during installation/startup.

---

## Frequently Observed Domains

| Domain | Observed In |
|---|---|
| ps5.np.playstation.net | All scenarios |
| envelope2.np.dl.playstation.net | Offline scenarios |
| telemetry-console.api.playstation.com | Multiple scenarios |
| sgst.prod.dl.playstation.net | Online + reinstall |
| feature.api.playstation.com | Offline reinstall |
| smetrics.aem.playstation.com | Multiple scenarios |

---

# TCP Retry/Timeout Analysis

## Observed Behavior

Offline scenarios demonstrated repeated retry behavior.

Observed symptoms:

- repeated DNS lookups
- repeated outbound session attempts
- prolonged waiting before fallback behavior
- startup delay persistence
- delayed installation initiation

The previously installed offline startup scenario demonstrated approximately 15 minutes of startup degradation before gameplay became accessible.

This is preservation relevant because:

- usability degrades over time
- startup reliability depends on timeout logic
- infrastructure assumptions remain embedded in startup flow

---

# CDN / Cloud Provider Analysis

## Observed Infrastructure Providers

### Microsoft Azure

Observed endpoints:

| Endpoint |
|---|
| iokntprod01storage.blob.core.windows.net |
| kntprodconfigstorage.blob.core.windows.net |

These domains are directly associated with Microsoft Azure Blob Storage.

Reasonable interpretation:

- cloud-hosted configuration
- event data
- backend content
- remote metadata

Exact content remains unknown.

---

## CDN Indicators

Multiple observed IP ranges and domain patterns strongly indicate CDN usage.

Examples:

| Domain | Likely Provider |
|---|---|
| sgst.prod.dl.playstation.net | CDN-backed delivery infrastructure |
| ps5.np.playstation.net | Akamai-associated ranges observed |
| telemetry-console.api.playstation.com | CDN-distributed API routing |

Observed IP ranges included:

- 2.x.x.x
- 23.x.x.x
- 104.x.x.x
- 184.x.x.x

These are consistent with large-scale distributed cloud/CDN infrastructure.

---

# PlayStation Platform Dependency Analysis

## Confirmed Platform Dependency Indicators

Observed throughout all tests:

- repeated PlayStation endpoint lookups
- startup-related infrastructure contact attempts
- telemetry infrastructure activity
- metadata infrastructure access
- feature discovery infrastructure lookups

Observed domains included:

| Domain | Likely Role |
|---|---|
| feature.api.playstation.com | Feature discovery/configuration |
| telemetry-console.api.playstation.com | Telemetry |
| appinfo.dl.playstation.net | Application metadata |
| envelope2.np.dl.playstation.net | Download/update infrastructure |
| ppr-crl.rnps.dl.playstation.net | Certificate/CRL infrastructure |

---

# IO Interactive Backend Analysis

The online baseline demonstrated clear communication with IO Interactive infrastructure.

Observed endpoints:

| Endpoint | Likely Role |
|---|---|
| kauth.ioi-cloud.com | Authentication/backend |
| kconf.ioi-cloud.com | Remote configuration |
| knteventsprod.ioi-cloud.com | Event infrastructure |
| iokntprod01.ioi-cloud.com | Game backend platform |

## Important Distinction

The captures prove infrastructure contact.

The captures do NOT independently prove:

- mandatory online gameplay dependency
- server-authoritative gameplay
- mission streaming
- online DRM enforcement

Only observed behavior should guide conclusions.

---

# Gameplay Accessibility Analysis

## Scenario Comparison

| Scenario | Gameplay Result |
|---|---|
| Online install/startup | All missions playable |
| Previously installed offline startup | All missions playable |
| Fresh offline reinstall | Only first mission playable |

This distinction is the central preservation finding.

---

# Future-Date Manipulation Analysis

## Purpose

Future-date manipulation was used to evaluate:

- retry persistence
- certificate dependency behavior
- survivability under future-state conditions
- platform validation assumptions

## Observed Effects

Observed during offline future-date startup:

- extended “Please Wait” behavior
- persistent retry activity
- eventual fallback startup
- gameplay remained functional in previously installed scenario

The captures alone cannot prove whether date manipulation directly caused startup delay.

However, the future-date condition coincided with persistent retry/failure behavior.

---

# Differential Analysis Between Tests

## Most Important Differential

| Property | Test 2 | Test 3 |
|---|---|---|
| Previously installed | Yes | No |
| Fresh reinstall | No | Yes |
| Offline startup success | Yes | Yes |
| All missions playable | Yes | No |
| Only first mission playable | No | Yes |
| Significant startup delay | Yes | Yes |
| Retry behavior | Yes | Yes |

This strongly suggests that prior online initialization materially affects long-term offline survivability.

---

# Preservation Implications

## Physical Media Survivability

Confirmed:

- substantial game data exists on disc
- offline installation from disc is possible
- startup without internet is possible

However:

- fresh reinstall did not expose full gameplay
- startup degradation was substantial
- retry behavior remained embedded in startup flow

This creates a preservation gap between:

- owning the disc
- fully preserving the game experience

---

# Consumer Ownership Implications

## Evidence-Based Interpretation

The testing demonstrates that ownership outcomes differ depending on historical installation state.

A consumer with:

- an already initialized installation

experienced materially better offline survivability than a consumer attempting:

- a fresh reinstall from physical media.

This distinction is preservation relevant because physical media has historically implied:

- reproducible reinstall capability
- long-term standalone usability
- independence from temporary infrastructure

The observed behavior suggests that modern platform ecosystems may weaken those assumptions.

---

# EU Legal and Preservation Context

The preservation concerns observed in this project align with broader ongoing EU discussions surrounding digital ownership and long-term accessibility of video games.

The European Citizens’ Initiative "Stop Destroying Videogames" (commonly known as "Stop Killing Games") calls for regulations requiring publishers to avoid rendering purchased games permanently unplayable after support ends. ([citizens-initiative.europa.eu](https://citizens-initiative.europa.eu/initiatives/details/2024/000007_en?utm_source=chatgpt.com))

The initiative reportedly exceeded 1 million validated signatures and advanced into formal EU review procedures during 2026. ([en.wikipedia.org](https://en.wikipedia.org/wiki/Stop_Killing_Games?utm_source=chatgpt.com))

The campaign was heavily influenced by concerns around titles becoming unusable after infrastructure shutdowns, particularly following Ubisoft’s shutdown of The Crew. ([en.wikipedia.org](https://en.wikipedia.org/wiki/The_Crew_%28video_game%29?utm_source=chatgpt.com))

The findings in this project are relevant to that broader discussion because they demonstrate a measurable difference between:

- retaining a previously initialized installation
- performing a future fresh reinstall from physical media without infrastructure access

Importantly, this report does not claim that 007 First Light violates EU law.

The project only documents:

- observed technical behavior
- preservation implications
- dependency characteristics

Industry organizations have publicly argued that mandatory preservation obligations could increase operational and technical burden for publishers. ([vgfb.be](https://vgfb.be/statement-stop-killing-games/?utm_source=chatgpt.com))

Meanwhile, preservation advocates argue that consumers should retain functional access to purchased software even after infrastructure shutdowns. ([stopkillinggames.com](https://www.stopkillinggames.com/en?utm_source=chatgpt.com))

This report does not take a political position.

---

# Long-Term Survivability Assessment

## Scenario A

### Previously Installed Offline Copy

Observed survivability:

- startup eventually succeeded
- all missions playable
- significant delay remained

Assessment:

Moderate survivability with degraded usability.

---

## Scenario B

### Fresh Offline Reinstall from Disc

Observed survivability:

- installation eventually succeeded
- startup eventually succeeded
- only first mission playable

Assessment:

Partial survivability.

This is the more preservation-relevant condition because it represents future reinstall capability. Whilest a large portion of the final game is on disc, it is rendered useless.

---

# Technical Conclusions

## Confirmed Conclusions

The following statements are directly supported by observed evidence:

- The game communicates extensively with PlayStation infrastructure during online startup/install behavior.
- The game communicates with IO Interactive cloud infrastructure during online startup.
- Azure Blob Storage infrastructure is used by IO Interactive backend systems.
- Offline startup behavior includes substantial retry/timeout activity.
- A previously installed copy remained fully playable offline.
- A fresh offline reinstall from disc exposed only the first mission.
- Approximately 45.9 GB of data exists on physical media. Final product has 50.89GB of data as per version 1.0.
- Startup degradation occurred during offline future-date testing.

## Reasonable Interpretations

The following interpretations are technically reasonable but not conclusively proven:

- Prior online initialization materially affects long-term survivability.
- Additional content or installation state may normally arrive through online infrastructure.
- Startup logic assumes infrastructure availability and only later falls back offline.
- Cloud-hosted configuration or metadata likely influences startup/install behavior.

## Unknown/Unproven

The following claims are NOT proven by the captures:

- mandatory always-online DRM
- intentional anti-preservation design
- remote mission streaming
- deliberate disabling of offline play
- legal non-compliance

---

# Final Preservation Classification

## Classification

### Partial Offline Survivability

### Platform-Dependent Survivability

### Elevated Long-Term Preservation Risk

## Rationale

The game demonstrated:

- successful offline startup
- substantial offline disc installation capability
- partial offline gameplay survivability
- Version 1.0 being 50.89GB where as an offline installation of version 0.1.0 has 49.9GB meaning that more than the 1st mission is on disc.

However:

- startup reliability degraded significantly offline
- fresh offline reinstall did not expose the full game
- prior online initialization materially improved survivability
- infrastructure dependency remained observable throughout startup behavior

The most preservation-relevant scenario — a future fresh reinstall from physical media with no online infrastructure available — resulted in only partial gameplay accessibility.

That outcome materially increases long-term preservation risk. 

