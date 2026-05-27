# 007 First Light - Offline Disc Install Attempt Analysis

## Test Summary

| Field | Value |
|---|---|
| Scenario | Disc inserted after uninstall, PS5 booted locally, internet blocked |
| Capture | `2026_05_27_PS5BootUp_007FirstLightNoInternetInstall.pcap` |
| PS5 IP | `192.168.x.x` |
| Duration | ~5m 50s |
| Packets | 546 |
| DNS queries | 100 |
| TCP packets | 428 |
| TCP behavior | 100% SYN-only |
| Successful TCP handshakes | 0 |
| Successful TLS sessions | 0 |
| Result | Disc spun, stopped, no install started |

## Confirmed Packet Behavior

The PS5 could reach the local gateway and DNS resolver.

DNS resolution worked.

The PS5 repeatedly attempted HTTPS connections to PlayStation/CDN infrastructure, but every TCP packet was SYN-only. No SYN-ACK responses were observed, meaning no outbound internet sessions completed.

This means the console was trying to reach external PlayStation infrastructure, but internet access was blocked.

## Domains Queried

| Domain | Query Count | Observed Purpose |
|---|---:|---|
| `ps5.np.playstation.net` | 52 | Core PSN/platform service |
| `telemetry-console.api.playstation.com` | 22 | PlayStation console telemetry |
| `envelope2.np.dl.playstation.net` | 6 | PlayStation download/update infrastructure |
| `ps5cel.np.dl.playstation.net` | 4 | PlayStation download/content infrastructure |
| `image.api.playstation.com` | 4 | PlayStation image/API content |
| `ppr-crl.rnps.dl.playstation.net` | 4 | Certificate revocation/check infrastructure |
| `sgst.prod.dl.playstation.net` | 2 | PlayStation download/CDN service |
| `feature-discovery-assets-amd.dl.playstation.net` | 2 | Feature/discovery asset download |
| `feature.api.playstation.com` | 2 | PlayStation feature API |
| `psnobj.prod.dl.playstation.net` | 2 | PlayStation object/download content |

## Resolved CDN / Infrastructure

The domains resolved mainly to Akamai and CloudFront infrastructure:

| Domain Chain | Resolved IPs |
|---|---|
| `ps5.np.playstation.net` → Akamai | `2.16.106.x` |
| `telemetry-console.api.playstation.com` → Akamai | `184.30.157.48` |
| `sgst.prod.dl.playstation.net` → CloudFront | `18.239.50.x` |
| `envelope2.np.dl.playstation.net` → Akamai | `23.222.48.204` |
| `image.api.playstation.com` → Akamai | `2.16.106.136`, `2.16.106.141`, `2.16.6.222`, `2.16.6.227` |
| `feature.api.playstation.com` → Akamai | `2.16.6.204`, `2.16.6.221` |
| `ppr-crl.rnps.dl.playstation.net` → Akamai | `104.109.143.x` |
| `psnobj.prod.dl.playstation.net` → Akamai | `23.222.43.97` |

## Timeline

| Time | Event |
|---:|---|
| 0s | Capture starts with existing outbound HTTPS attempts |
| 2.6s | `ps5.np.playstation.net` queried |
| 13.6s | `sgst.prod.dl.playstation.net` queried |
| 21.8s | `telemetry-console.api.playstation.com` queried |
| 35.4s | `ps5cel.np.dl.playstation.net` and `envelope2.np.dl.playstation.net` queried |
| 64.2s | `image.api.playstation.com` and `feature-discovery-assets-amd.dl.playstation.net` queried |
| 220.9s | `feature.api.playstation.com` queried |
| 306.6s | `ppr-crl.rnps.dl.playstation.net` queried |
| 341.4s | `psnobj.prod.dl.playstation.net` queried |

## What The Packets Show

The PS5 was not downloading game data.

The PS5 was not successfully authenticating.

The PS5 was not reaching IOI cloud services.

The PS5 was repeatedly trying to reach PlayStation infrastructure over HTTPS and failing before a connection could be established.

Because every TCP packet was SYN-only, the capture does not contain application-layer instructions, HTTP requests, TLS payloads, or install commands. The failure occurs before that layer.

## What The Console Appears To Need For Install

Based on the capture, the PS5 attempted to contact these service categories before installation continued:

| Required/Attempted Category | Evidence |
|---|---|
| PSN core service | `ps5.np.playstation.net` |
| PlayStation download/CDN services | `sgst.prod.dl.playstation.net`, `ps5cel.np.dl.playstation.net`, `envelope2.np.dl.playstation.net`, `psnobj.prod.dl.playstation.net` |
| PlayStation metadata/API content | `image.api.playstation.com`, `feature.api.playstation.com` |
| Feature/discovery assets | `feature-discovery-assets-amd.dl.playstation.net` |
| Certificate/revocation infrastructure | `ppr-crl.rnps.dl.playstation.net` |
| Console telemetry | `telemetry-console.api.playstation.com` |

## Interpretation

The evidence points to the PS5 attempting to retrieve PlayStation-side metadata, download service information, certificate/revocation data, feature assets, and possibly disc/title presentation metadata before proceeding.

The disc spinning and then stopping without installation strongly suggests the PS5 install flow was waiting on PlayStation platform services or metadata retrieval and failed silently when those services were unreachable.

## Important Limitation

This capture does not prove which specific endpoint is mandatory.

It proves that during the failed offline install attempt, the PS5 attempted to reach PlayStation infrastructure and never established a successful outbound session.

## Preservation-Relevant Conclusion

This is a stronger preservation concern than the previous offline boot test.

The game appears playable offline after installation, but this test suggests the disc install process may depend on PlayStation platform connectivity or online metadata/update infrastructure.

Current classification for this specific test:

| Area | Assessment |
|---|---|
| Offline install from disc | Failed / did not proceed |
| Offline boot after prior install | Eventually successful |
| Game-specific backend dependency during install | Not observed |
| PlayStation platform dependency during install | Observed |
| Preservation risk | Medium-High for fresh offline installation |

## Final Note

The current evidence suggests a split behavior:

- Once installed, the game can eventually boot and play offline.
- Fresh offline installation from disc did not proceed in this test.
- The blocking point appears to be PlayStation platform/download/metadata infrastructure, not confirmed IOI game-server infrastructure.

That means the main preservation risk may not be the game runtime itself, but the PS5 disc installation and platform service dependency path.
