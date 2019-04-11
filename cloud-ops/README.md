# Pivotal PACE: Cloud Operations Workshop

This is the home for all things related to the Pivotal PACE Cloud Operations workshop. Here you will find the content being used by [pace-builder](https://github.com/Pivotal-Field-Engineering/pace-builder) to create Cloud Operations workshops.

The content is categorized into the three 'levels' of workshop to be delivered:
- **Level 1** - Cloud Operations Discovery
- **Level 2** - Cloud Operations Fundamentals
- **Level 3** - Cloud Operations Advanced

Content can be shared between levels, however progressing from Level 1 to Level 3 with a core group of customers will provide maximum value.

WHAT IS THE SYNOPSIS OF THIS WORKSHOP??

---
## Curators Notes

**TO CONSIDER:** Slides vs Whiteboard (credit RBM)

**TO CONSIDER:** POC Credits and Timing of a deal (credit RBM)

**TO CONSIDER:** Jumpbox vs OpsMan

**TO CONSIDER:** Ensure timing and success factor of slide/demo ratio

**TO DO:** Interview 2-3 customers for feedback

**TO DO:** Feedback from PA team on existing content

**TO DO:** Stand & Deliver during PA office hours

**TO DO:** Condense S1P video notes for customer insights

**TO DO:** Build Dev & start creating demos

**TO DO:** Create workshop pre-reqs punchlist

---

## Level 1 - Cloud Operations Discovery
### Table of Contents

#### Prerequisites
(((- **Pivotal PA Prep Material:** cloud-ops-l1-prereqs.en.md)))

#### Introduction
- **Concepts:** Setting the stage for who we are and why PCF.
- **Slides:** intro-and-value-prop-slides.en.md

#### Who is Pivotal
- **Concepts:** Overview of who we are.
- **Slides:** who-is-pivotal-slides.en.md

#### What is PCF
- **Concepts:** Workload abstractions and the PCF overview.
- **Slides:** what-is-pcf.en.md
- **Demo/Lab:** Push an App (Refer to Keep notes)

#### PCF components
- **Concepts:** BOSH, PAS, PKS, PFS, Marketplace, Concourse
- **Slides:** high-level-pcf-components.en.md

#### IaC concepts
- **Whiteboard:** Hey customer what does your IaaS provisioning process look like?
- **Concepts:** BOSH, Immutable Infrastructure, Concourse, Customer Efficiencies
- **Slides:** iac-concepts.en.md

#### Deployments
- **Whiteboard:** Hey customer what does your application deployment process look like?
- **Concepts:** BOSH Deployments / Concourse / Simplified path to prod
- **Slides:** (deployment-examples.en.md)

#### Supporting Services
- **Concepts:** Pivotal Network, Marketplace, & Ecosystem
- **Whiteboard:** Hey customer what services do you currently integrate with and support?
- **Demo:** Pivotal Network & Marketplace

#### Security Posture
- **Concepts:** R/R/R & Compliance Scanner
- **Demo:** Rotate (Service Credentials) & Repave (BOSH Recreate)

#### Recap
- **Concepts:** Customer Outcomes
- **Slides:** Customer Outcomes and Testimonials

---

## Level 2 - Cloud Operations Fundamentals
### Table of Contents

#### Prerequisites
(((- **Pivotal PA Prep Material:** cloud-ops-l2-prereqs.en.md)))

#### L1 Overview
- Complete or Pruned

Pre-Requisite Knowledge (Control-Plance / Foundations / Orgs / Spaces / IaaS components (certs, network, vpc) / OpsMan & BOSH Director)

Operations & Automation (IaC Concepts/ Immutable Infrastructure / BOSH (BOSH client) / Deployments / Upgrades / Platform Automation / Concourse)

Security (RRR / CredHub (Client Setup)/ Compliance Scanner)

Monitoring Platform Health (BOSH & Healthwatch)

#### Platform as a Product
- Concepts:

---

## Level 3 - Cloud Operations Advanced
### Table of Contents

#### Prerequisites
(((- **Pivotal PA Prep Material:** cloud-ops-l3-prereqs.en.md)))

_**DAY1**_
#### L1 Overview
- Complete or Pruned
#### Control-Plane concepts
- Concepts:
- Demo/Lab:
- **Concourse**
  - Concepts:
  - Demo/Lab:
- **Platform Automation**
  - Concepts:
  - Demo/Lab:
- **OpsMan & BOSH Director (some BOSH workshop material)**
  - Concepts:
  - Demo/Lab:
- **Tiles**
  - Concepts: Tiles & Marketplace
  - Demo/Lab: Install PAS tile
  - Demo/Lab: Pivotal Network

_**DAY2**_
- **Deployments**
  - Demo/Lab: Push an app
  - Concepts: BOSH deployments and
- **Apps & Health**
  - Demo/Lab: PCF Apps Manager
  - Demo/Lab: Provision PCF HealthWatch
- **Security Posture & Credhub**
  - Concepts: R/R/R & Compliance Scanner
  - Demo/Lab: Provision Compliance Scanner
  - Demo/Lab:
- **Upgrades**
  - Demo/Lab: Tile Upgrade
  - Concepts: Foundation Upgrade Planning (Upgrade Planner)
- **SRE Concepts & Platform as a Product**
  - Concepts:
- **Wrap Up & Retro**
