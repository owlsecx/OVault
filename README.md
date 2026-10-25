# 🦉 OVault

<p align="center">
  <img src="https://img.shields.io/badge/Platform-Linux-0a0c10?style=flat-square"/>
  <img src="https://img.shields.io/badge/Category-OPass-blue?style=flat-square"/>
  <img src="https://img.shields.io/badge/License-MIT-green?style=flat-square"/>
  <img src="https://img.shields.io/badge/Part%20of-OwlSec%20Toolkit-7b5ea7?style=flat-square"/>
  <img src="https://img.shields.io/badge/Version-2.0-cyan?style=flat-square"/>
</p>

> **OVault** — Secure Password Manager

---

> ⚠️ **AUTHORISED USE ONLY** — Use only on systems you own or have explicit written permission to test. Unauthorised use is illegal.

---

## 📌 Overview


OVault is an offline, read-only HashiCorp Vault configuration auditor that parses HCL and JSON server configuration with stdlib-only parsers. It evaluates listener TLS, storage backend security, seal/auto-unseal settings, audit-device coverage, telemetry exposure and cluster communication. Findings are mapped to CWE and MITRE ATT&CK, scored and graded, and exported as TXT, Markdown, JSON, CSV or SARIF.

---

## ✨ Features


- HCL and JSON Vault config parsers (listener, storage, seal, telemetry, service_registration stanzas)
- Listener TLS checks: tls_disable, tls_min_version, cipher suites, client-cert auth and address binding
- Storage backend review (raft/consul/file) with TLS-to-backend and path-permission checks
- Seal / auto-unseal configuration, disable_mlock and audit-device coverage gaps
- API/cluster address exposure, UI enablement, telemetry endpoint and raw/pprof debug exposure
- CWE+ATT&CK mapping, offline --root trees, score/grade and TXT/MD/JSON/CSV/SARIF export

---

## ⚙️ Requirements

- **Linux**
- **No Python installation needed** — runs as a standalone executable

---

## 🚀 Usage


```bash
./OVault --scan -f sarif -o vault.sarif
./OVault --config /etc/vault.d/vault.hcl --min-severity medium
./OVault -r /mnt/image --fail-on high --json
./OVault --demo
./OVault --help
```

---

## 📦 Part of OwlSec Toolkit

This tool is part of the **OwlSec** suite — a collection of 300+ security and privacy tools.

🔗 [owlsec.org](https://owlsec.org)

---

## ©️ License

MIT License — © Khaled S. Haddad

*Tools are distributed as pre-built executables. Source code is proprietary.*
