<img width="2048" height="2048" alt="image" src="https://github.com/user-attachments/assets/9f871d8f-262a-4356-8f56-a381a847b9db" />

---

# Docker Scout – Secure Your Images Like a Pro 🕵️🐳

> **Short pitch:** Native Docker tooling to analyze images, surface CVEs, generate SBOMs, and give fix suggestions — locally and in CI.

---

## 📚 Table of Contents

* [Scenario](#-scenario)
* [Where Docker Scout Comes In](#-where-docker-scout-comes-in)
* [Benefits](#-benefits)
* [Why Docker Scout if we already use Trivy/others?](#-why-docker-scout-if-we-already-use-trivyothers)
* [Prerequisites (Ubuntu)](#-prerequisites-ubuntu)
* [Install Docker Scout on Ubuntu](#-install-docker-scout-on-ubuntu)
* [Quick Start: Build → Scan → Fix](#-quick-start-build--scan--fix)
* [Common Workflows & Reports](#-common-workflows--reports)
* [GitHub Actions (CI) Examples](#-github-actions-ci-examples)
* [All Useful `docker scout` Commands](#-all-useful-docker-scout-commands)
* [Tips & Good Practices](#-tips--good-practices)

---

## 🎯 Scenario

You’re building a containerized app (say, a Node/Flask/Go service). Every PR builds a Docker image and pushes to a registry. Before promoting to staging/prod, you need to:

* Detect **vulnerabilities (CVEs)** in app deps and base image
* Get **actionable remediation** (e.g., base image updates)
* Produce **SBOM** & optional **SARIF** for code scanning dashboards
* Block merges if **critical/high** vulns are introduced

## 🧭 Where Docker Scout Comes In

Docker **Scout** plugs directly into Docker Desktop/CLI/Hub and CI. It analyzes images (local or registry), builds SBOMs/uses attestations, compares image versions, and recommends safer base images. You can run it on your laptop or wire it into GitHub Actions to break builds on new high/critical CVEs.

## ✅ Benefits

* **Native Docker integration** — works from `docker` CLI & Docker Hub/Scout dashboard
* **Actionable fixes** — `recommendations` suggests base image refreshes
* **SBOM aware** — view or attach SBOMs; supports SPDX output
* **Comparisons** — `compare` PR image vs. prod to see *delta* in vulns
* **Automations** — GitHub Action + automatic analysis of images in registries
* **Formats for tooling** — JSON/SARIF output you can stash in artifacts

## 🤔 Why Docker Scout if we already use Trivy/others?

**Use both when it makes sense.**

* **Scout strengths:** Seamless Docker UX, base-image remediation tips, automatic Hub analysis, first‑class PR/compare flows, policy gates.
* **Trivy strengths:** Broad OSS scanner (images + files + IaC + SBOMs), air‑gapped friendly, huge ecosystem.
* **Reality:** Many teams keep Trivy for broad scanning but add Scout for developer‑friendly UX and base image guidance. Pick what fits your pipeline; mixing is normal.

---

## 🧩 Prerequisites (Ubuntu)

* Docker Engine installed and running
* A Docker Hub account (for Hub/Scout integrations)

> If you need Docker on Ubuntu, install it first (Engine + CLI), then continue.

---

## 🛠️ Install Docker Scout on Ubuntu

Two easy options — **Script (recommended)** or **Manual**.

### Option A) One‑liner script

```bash
# installs the docker-scout CLI plugin under ~/.docker/scout
curl -sSfL https://raw.githubusercontent.com/docker/scout-cli/main/install.sh | sh -s --
```

### Option B) Manual install

```bash
# 1) Make a plugin directory
mkdir -p "$HOME/.docker/scout"

# 2) Download the latest release for your CPU (amd64 or arm64)
#    -> https://github.com/docker/scout-cli/releases
#    Example: docker-scout_${VERSION}_linux_amd64.tar.gz

# 3) Extract and move the binary
# (assumes you downloaded to ~/Downloads)
tar -xzf ~/Downloads/docker-scout_*_linux_*.tar.gz -C "$HOME/.docker/scout"
chmod +x "$HOME/.docker/scout/docker-scout"

# 4) Register plugin path in ~/.docker/config.json
# add (or merge) this key:
# {
#   "cliPluginsExtraDirs": ["/home/<USER>/.docker/scout"]
# }
```

> Restart your shell and run `docker scout version` to verify.

---

## ⚡ Quick Start: Build → Scan → Fix

```bash
# 1) Build your image
docker build -t myapp:local .

# 2) Quick overview (summary of CVEs + base-image status)
docker scout quickview myapp:local

# 3) Full CVE report
docker scout cves myapp:local

# 4) Base image fix suggestions
docker scout recommendations myapp:local

# 5) Compare with production image (see new CVEs introduced)
docker scout compare myapp:local --to myorg/myapp:prod
```

---

## 📊 Common Workflows & Reports

### Scan any artifact type

```bash
# local image tarball
docker scout cves ./myimage.tar

# local project directory (index and scan)
docker scout cves .

# SBOM for an image (human list)
docker scout sbom --format list alpine:latest

# SPDX SBOM to file
docker scout sbom --format spdx myapp:local > sbom.spdx.json
```

### Machine-readable outputs

```bash
# JSON output to file
docker scout cves myapp:local --format json > cves.json

# SARIF (upload to GitHub code scanning)
docker scout cves myapp:local --format sarif > scout.sarif
```

### Auto-analyze images you push

Enable Scout for your registry repos, then every new push gets analyzed and stays up-to-date as advisories change.




---

## 📖 All Useful `docker scout` Commands

> You can run most commands on images, tarballs, OCI dirs, or local dirs.

**Essentials**

* `docker scout quickview [REF]` – summary of image vulns + base-image status
* `docker scout cves [REF]` – detailed CVE report; `--only-severity`, `--exit-code`, `--format json|sarif`
* `docker scout sbom [REF]` – print/generate SBOM; `--format list|spdx`
* `docker scout recommendations [REF]` – base-image update/refresh suggestions
* `docker scout compare [A] --to [B]` – compare two images (PR vs prod)

**Repositories & orgs**

* `docker scout repo list [ORG]`
* `docker scout repo enable [REPO]` / `disable` – turn analysis on/off for a repo

**Environments & streams**

* `docker scout environment list|record` – track images by env (e.g., `--to-env production`)
* `docker scout stream` – record/list deployment streams; use with `compare`

**Pushing & watching**

* `docker scout push [IMAGE]` – push an image or analysis result to Scout
* `docker scout watch [REGISTRY/NS]` – watch registry repos and push to Scout

**Cache & versioning**

* `docker scout cache df|prune` – view or clear local SBOM cache
* `docker scout version` – show CLI version

**(Experimental) Policies & VEX**

* `docker scout policy` – evaluate policies against an image
* `docker scout vex` – manage VEX attestations

---

## 🧠 Tips & Good Practices

* Keep base images **fresh**; often fixes dozens of CVEs instantly
* Generate & **attach SBOMs at build time** (via BuildKit attestations)
* Capture **SARIF** in CI so GitHub code scanning shows findings inline
* Use `--ignore-unchanged` with `compare` to focus on *new* issues only
* Fail fast on **critical/high** in PRs; allow lower severities but track



---
