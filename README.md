# HCA Healthcare — Network Threat ## Live

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

#
