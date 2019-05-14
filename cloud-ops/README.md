# Pivotal PACE: Cloud Operations Workshop

This is the home for all things related to the Pivotal PACE Cloud Operations workshop. Here you will find the content being used by [pace-builder](https://github.com/Pivotal-Field-Engineering/pace-builder) to create Cloud Operations workshops.

The content is categorized into the three 'levels' of workshop to be delivered:
- **Level 1** - Cloud Operations Discovery
- **Level 2** - Cloud Operations Fundamentals
- **Level 3** - Cloud Operations Advanced

Content can be shared between levels, however progressing from Level 1 to Level 3 with a core group of customers will provide maximum value.

Platform Architects should review the **Prerequisites** documents for each workshop level for tips and notes on content and workshop flow.

---

## Level 1 - Cloud Operations Discovery
### Table of Contents

#### PA Prerequisites
- **Pivotal PA Prep Material:** cloud-ops-l1-prereqs.en.md

#### Introduction
- **Concepts:** Setting the stage for who we are and why PCF.
- **Slides:** intro-and-value-prop-slides.en.md

#### Who is Pivotal
- **Concepts:** Overview of who we are.
- **Slides:** who-is-pivotal-slides.en.md

#### What is PCF
- **Concepts:** Workload abstractions and the PCF overview.
- **Whiteboard:** Hey customer what types of workloads are you supporting?
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
- **Concepts:** Concourse / Simplified path to prod
- **Slides:** deployment-examples.en.md

#### Supporting Services
- **Concepts:** Pivotal Marketplace & Ecosystem
- **Whiteboard:** Hey customer what services do you currently integrate with and support?
- **Slides:** supporting-services.en.md
- **Demo:** Pivotal Network

#### Security Posture
- **Concepts:** R/R/R & Compliance Scanner
- **Whiteboard:** Hey customer how are you patching and protecting your assets?
- **Slides:** (security-posture.en.md)
- **Demo:** Rotate (Runtime Encryption Keys) & Repave (BOSH Recreate)

#### Platform Operations Team & Platform as a Product
- **Concepts:** Platform as a product and operational efficiencies
- **Slides:** (platform-product.en.md)

#### Recap
- **Concepts:** Customer Outcomes & Benefits
- **Slides:** (customer-outcomes.en.md)

---

## Level 2 - Cloud Operations Fundamentals
### Table of Contents

#### Prerequisites
- **Pivotal PA Prep Material:** ((- **Pivotal PA Prep Material:** cloud-ops-l2-prereqs.en.md))

#### L1 Overview
- Complete or Pruned

#### A Day in the Life of the Platform Team
- **Concepts:**
- **Slides:**

#### Making Your Platform a Product
- **Concepts:** What makes a makes up a platform product and what is the journey?
- **Slides:**

#### Deploying Apps on PCF
- **Concepts:** The pieces, parts, and steps of a `cf push`
- **Slides:**
- **Demo:** Push an App (UI & CLI)

#### The Platform Components
- **Concepts:** Layer cake build up to 'value line'
- **Slides:**

#### Platform Installation and Setup
- **Concepts:** Bootstrapping, Ops Manager, and Tiles
- **Slides:**
- **Demo:** Ops Manager functionality

#### Automating the Operations
- **Concepts:** Platform Automation & BOSH
- **Slides:**
- **Demo:** BOSH Basics

#### BOSH Availability Concepts
- **Concepts:** Deployment HA, Canaries, and BOSH Internals HA
- **Slides:**

#### Logging & Observability
- **Concepts:** APM discussion, Logging, and Nozzles
- **Slides:**

#### Security Concepts
- **Concepts:** Stemcell hardening, CVE, Credhub, and IsoSeg
- **Slides:**

#### Supporting Services
- **Concepts:** What is a Service and Service Brokering
- **Slides:**

#### Summary & Next Steps
- **Concepts:** Wrap up and next steps discussion
- **Slides:**

##### TO BE ADDED:
Prerequisite Knowledge
- Control-Plane
- Foundations
- Orgs & Spaces
- IaaS Components (certs, networks, vpc)
- OpsMan & BOSH Director

Operations & Automation
- Concourse

Platform Internals

Logging & Observability (BOSH Diagnostics & Healthwatch)

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

#### OpsMan & BOSH Director (some BOSH workshop material)
- Concepts:
- Demo/Lab:

#### Concourse
- Concepts:
- Demo/Lab:

#### Platform Automation
- Concepts:
- Demo/Lab:

#### Tiles
- Concepts: Tiles & Marketplace
- Demo/Lab: Install PAS tile
- Demo/Lab: Pivotal Network

_**DAY2**_
#### Deployments
- Demo/Lab: Push an app
- Concepts: BOSH deployments and

#### Apps & Health
- Demo/Lab: PCF Apps Manager
- Demo/Lab: Provision PCF HealthWatch

#### Security Posture & Credhub
- Concepts: R/R/R & Compliance Scanner
- Demo/Lab: Provision Compliance Scanner
- Demo/Lab:

#### Upgrades
- Demo/Lab: Tile Upgrade
- Concepts: Foundation Upgrade Planning (Upgrade Planner)

#### SRE Concepts & Platform as a Product
- Concepts:

#### Wrap Up & Retro

---
## Curators Notes

**TO CONSIDER** Platform Team & Platform as a Product who & where?

**TO CONSIDER:** POC Credits and Timing of a deal (credit RBM)

**TO CONSIDER:** Jumpbox vs OpsMan

**TO CONSIDER:** Ensure timing and success factor of slide/demo ratio

**TO CONSIDER:** I'M GOING TO MAKE YOU FAMOUS!

**TO DO:** Integrate OpsMan deep dive (credit TF)

**TO DO:** Integrate into Level 1 some Platform as a Product and benefits into Recap

**TO DO:** Integrate outcomes and benefits discussion that covers platform as a product earlier in discovery and find what the customer thinks is success for a platform team.

**TO DO:** Interview 2-3 customers for feedback

**TO DO:** Condense S1P video notes for customer insights

**TO DO:** Build Dev & start creating demos

**TO DO:** Create workshop pre-reqs punchlist

**TO DO:** Check Themes and Branding across all decks
