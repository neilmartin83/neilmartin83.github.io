---
layout: post
title: "JNUC 2026 - Managing the Jamf Platform at Scale with Infrastructure as Code"
date: 2026-09-29
---

![](/assets/2026/09/29/iac.png)

I had the honour of presenting alongside my colleague [Kyle Hoare](https://www.linkedin.com/in/kyle-hoare) at this year's Jamf Nation User Conference (JNUC) in Kansas City. Our session continued Jamf's [Infrastructure as Code](https://en.wikipedia.org/wiki/Infrastructure_as_code) journey, focusing on managing the Platform at scale. We shared the year's fast-moving developments of Jamf's Terraform provider ecosystem, its convergence onto the Jamf Platform, and wider tooling before diving into the practical workflows we've built to manage multiple Jamf Protect environments for customers in an MSP context.

[Download the slides here](/assets/2026/09/29/managing_the_jamf_platform_at_scale_with_infrastructure_as_code.pdf). The session video is available in the [JNUC Session Catalog](https://reg.jnuc.jamf.com/flow/jamf/jnuc2026/home26/page/sessioncatalog/session/1775690693172001UgbJ) for registered attendees, and will be released publicly in the coming months. I will update this post when it is available.

It's been a wild year for Infrastructure as Code! It was a key theme across the conference, following its recent rapid growth and maturation. It featured in both the [Opening Keynote](https://youtu.be/bGtU5hoLh00?si=bDbXu_DJMsG8SvO4) and [Platform State of the Union](https://youtu.be/hrMK0wwgx_M?si=fc7pNixUyrIv4QsL), with several other sessions covering the topic. Some highlights were:

- [Taming Jamf at Scale: Infrastructure as Code with AutoPkg, ServiceNow & Jira Integration](https://reg.jnuc.jamf.com/flow/jamf/jnuc2026/home26/page/sessioncatalog/session/1775569168477001gOCn) - Jason Phegley - Eli Lilly

- [From Zero to GitOps: Training a Mac Team to Work Like DevOps Engineers](https://reg.jnuc.jamf.com/flow/jamf/jnuc2026/home26/page/sessioncatalog/session/1775739355896001DzqX) - Gordon Deacon, Dafydd Watkins, Joseph Little - Lloyds Banking Group

- [From ClickOps to GitOps: Migrating Jamf Pro at Enterprise Scale to Terraform](https://reg.jnuc.jamf.com/flow/jamf/jnuc2026/home26/page/sessioncatalog/session/1775732050756001Wih3) - Gordon Deacon, Dafydd Watkins, Joseph Little - Lloyds Banking Group

- [From ClickOps to Code: Refactoring Jamf Workflows into Terraform Modules](https://reg.jnuc.jamf.com/flow/jamf/jnuc2026/home26/page/sessioncatalog/session/1775849497542001ML0G) - Rob Lee - Jamf

## Resources

- **Terraform MSP Reference Project** (a multi-environment implementation with workflows): [https://github.com/Jamf-Concepts/terraform-jamf-platform/tree/ref-jamfprotect-msp](https://github.com/Jamf-Concepts/terraform-jamf-platform/tree/ref-jamfprotect-msp)

- **Infrastructure as Code guides** (Jamf Concepts): [https://concepts.jamf.com/en/guides/infrastructure-as-code/](https://concepts.jamf.com/en/guides/infrastructure-as-code/)

- **Jamf Platform Terraform Provider Documentation** (now in the `jamf` namespace!): [https://registry.terraform.io/providers/jamf/jamfplatform/](https://registry.terraform.io/providers/jamf/jamfplatform)

- **Jamf Protect Terraform Provider Documentation**: [https://registry.terraform.io/providers/jamf-concepts/jamfprotect/latest/docs](https://registry.terraform.io/providers/jamf-concepts/jamfprotect/latest/docs)

- **Jamf Pro Community Terraform Provider Documentation**: [https://registry.terraform.io/providers/deploymenttheory/jamfpro/latest/docs](https://registry.terraform.io/providers/deploymenttheory/jamfpro/latest/docs)

- **jamformer** (Export your Jamf configuration to Terraform): [https://github.com/Jamf-Concepts/jamformer](https://github.com/Jamf-Concepts/jamformer)

- **jamf-cli** (Unified command line tool for the Jamf Platform): [https://github.com/Jamf-Concepts/jamf-cli](https://github.com/Jamf-Concepts/jamf-cli)

Join the discussion on the [MacAdmins Slack](https://www.macadmins.org/):

[#terraform-provider-jamfplatform](https://macadmins.slack.com/archives/C09HP9V1K5H)

[#terraform-provider-jamfpro](https://macadmins.slack.com/archives/C06R172PUV6)

Thank you to everyone who attended the conference and our sessions as well as the speakers and organisers that helped to make it a fantastic event.
