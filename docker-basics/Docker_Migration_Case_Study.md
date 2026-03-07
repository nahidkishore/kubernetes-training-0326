# Case Study: The Shift to Docker & Immutable Infrastructure

**Author:** Lead DevOps Architect
**Subject:** Justification for Containerization Strategy

---

## 1. The "Deployment Disaster" Case Study

### _The "Friday Night Library Mismatch"_

**Context:**
Before Docker, our "LegacyBilling" application was deployed manually via Ansible scripts to mutable VM instances.

**The Incident:**

- **Dev Environment:** Developers upgraded a local library (`requests` v2.28) to fix a bug. It worked perfectly on their MacBooks.
- **Production Environment:** The production servers were running CentOS 7. The deployment script simply ran `pip install -r requirements.txt`.
- **The Failure:** The new version of the library required a newer version of `OpenSSL` than what was available on the CentOS 7 system repositories.
- **The Result:** The application crashed on startup with a `Symbol not found` error.
- **The Panic:** Ops tried to manually compile a newer OpenSSL on production servers. This overwrote system libraries, breaking SSH access to 3 out of 5 nodes.
- **Downtime:** 4 Hours.
- **Root Cause:** **Environment Drift**. The code was deployed, but the _environment_ (OS dependencies) was not consistent.

**The Docker Solution:**
With Docker, the OS libraries (OpenSSL) are bundled _inside_ the container image. We build the image _once_. If it builds, it runs. The host OS version no longer matters.

---

## 2. Understanding "Immutable Infrastructure"

In the context of our migration, we are moving from **Mutable** to **Immutable** infrastructure.

### Mutable (The Old Way)

- **Concept:** Servers are "Pets". We name them (`web-prod-01`).
- **Updates:** We SSH into them and run updates (`apt-get update`, `git pull`).
- **Problem:** Over time, servers degrade. A manual change made by a SysAdmin 6 months ago to fix a hot issue is forgotten, making the server unique and impossible to reproduce.

### Immutable (The Docker Way)

- **Concept:** Containers are "Cattle". We number them.
- **Updates:** We **never** patch a running container. We build a **new** image (v2.0), deploy it, and kill the old one (v1.0).
- **Benefit:**
  - **Predictability:** The artifact deployed to Production is bit-for-bit identical to the one tested in QA.
  - **Rollbacks:** If v2.0 fails, we don't "undo" changes. We simply redeploy the v1.0 image which we know works.

---

## 3. CTO Justification Checklist

Use this checklist to validate our architectural shift:

| Metric         | Manual / VM Deployment                                | Docker / Containerization                           | Business Impact                                             |
| :------------- | :---------------------------------------------------- | :-------------------------------------------------- | :---------------------------------------------------------- |
| **Parity**     | Low. "It works on my machine" is common.              | **Guaranteed**. The container _is_ the environment. | Eliminates 30% of bug reports related to config.            |
| **Onboarding** | **Slow**. New devs spend 2 days setting up local env. | **Fast**. `docker compose up` takes 5 minutes.      | New hires push code on Day 1.                               |
| **Density**    | **Low**. 1 App per VM to avoid conflicts.             | **High**. Multiple apps per VM, isolated by kernel. | Reduces AWS EC2 bill by ~40%.                               |
| **Rollback**   | **Risky**. Reverting code doesn't revert system libs. | **Instant**. Switch image tag from `v2` to `v1`.    | Reduces MTTR (Mean Time To Recovery) from hours to seconds. |
| **Security**   | **Complex**. Patching OS affects all apps.            | **Targeted**. Update base image and redeploy.       | Faster patch cycles for CVEs.                               |

---

_Internal Note: This document serves as the preamble for the Q3 Infrastructure Modernization Initiative._
