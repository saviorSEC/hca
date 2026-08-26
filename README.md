# HCA Healthcare — Network Threat Map

**Passive OSINT attack-surface galaxy** of HCA Healthcare (Columbia/HCA), the largest hospital system in the United States (~180+ hospitals, 2,000+ care sites).

> 🏥 Built for the presentation **"You've Been Hacked by a Hacker: Five Ways Hospitals Get Breached"** — a visual of how a hospital's crown-jewel network actually looks from the outside, and why segmentation matters.

## Live

**https://saviorsec.github.io/hca/**

## What this is

A 3D force-graph galaxy of HCA's publicly visible infrastructure, mapped **passively**:

- **10 zones** — Corporate, Epic EHR/MyChart, Patient Portal (MyHealthONE), Enterprise (ehc.com), HR (Infor CloudSuite), Remote Access/VPN, Divisions, Email/Collab, Cloud, Dev/Staging
- **52 services** + 2 IP pools (AS14626 Columbia/HCA Nashville; Azure edge)
- **76 links** — how services connect, where identity/email/cloud converge

## Recon methods (all passive)

| Method | What it found |
|---|---|
| Certificate Transparency (certspotter) | 2,686+ unique names across hcahealthcare.com, ehc.com, myhealthone.com, careersathca.com, wearehcahealthcare.com |
| DNS resolution / CNAME chain | Epic MyChart → `*.app.medcity.net`, medialab → `cloudpowered.services`, careers → TTC Portals |
| ASN / whois | AS14626 Columbia/HCA Healthcare (Nashville) owns 165.214.41.39, 199.91.39.113, 199.91.40.155 |
| TXT / MX records | M365 (`MS=ms62915716`), Adobe IDP, LaunchDarkly, Proofpoint mail gateways |
| Benign HTTP classification | 301/302/403/404/200 fingerprints — no exploitation, no creds |

## Crown-jewel highlights

- **Epic MyChart clusters** — `mychart.hca-{south,west,southcentral}.com` → Epic-hosted `medcity.net` (patient EHR front door)
- **MyHealthONE patient portal** — Apache 302 → `/mh1/public/mh1` (Epic MyChart)
- **Infor CloudSuite HCM** — `hcahranswers.com` / `hcaghr.com` → `mingle-portal.inforcloudsuite.com/HCAHEALTHCARE_PRD` (HRMS: pay, benefits, identity)
- **Epic ePay** — `billpay.healthcare` → `hca.epayhealthcare.com` (AWS)
- **Juniper VPN installer** — `juniperinstaller.hcahealthcare.com` (200) — remote-access entry
- **F5 BigIP webmail** — `webmail.hcacollab.net` (199.91.40.155)
- **Enterprise fleet** — `secure.ehc.com` wildcard, `prod.ehc.com` services (oas, globalinc, fadmaa1-pro, alerting-monitoring huts)
- **Pre-prod surface** — `preprod.verify`, `axn.verify`, `doccapture`, `kba`, `stage-*`, `dc.*` data-center twins

## Why it matters (talk tie-in)

Hospitals don't get breached through one magic door — they get breached because the **flat network** lets an attacker walk from the coffee-shop Wi-Fi to the EHR. This galaxy shows the real convergence points: identity (Okta-style SSO, Infor, M365), email (Proofpoint → OWA), remote access (Juniper), and patient-facing web (MyChart → medcity.net). Segmentation = making every one of these hops cost an attacker another exploit.

## Data

- `data.json` — machine-readable graph `{zones, services, ipPools, links}`
- `hca_healthcare.js` — same data embedded for the renderer
- `index.html` — 3D force-graph viewer (Three.js + 3d-force-graph)

## Boundary

Passive/public artifacts only. No exploitation, no credentials, no active scanning. TLP:AMBER — do not re-identify or target.

---

**Church of Malware** 🖤 · passive OSINT · 2026-08-26
