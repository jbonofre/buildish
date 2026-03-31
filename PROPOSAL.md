<!--
  Licensed to the Apache Software Foundation (ASF) under one
  or more contributor license agreements.  See the NOTICE file
  distributed with this work for additional information
  regarding copyright ownership.  The ASF licenses this file
  to you under the Apache License, Version 2.0 (the
  "License"); you may not use this file except in compliance
  with the License.  You may obtain a copy of the License at

   http://www.apache.org/licenses/LICENSE-2.0

  Unless required by applicable law or agreed to in writing,
  software distributed under the License is distributed on an
  "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
  KIND, either express or implied.  See the License for the
  specific language governing permissions and limitations
  under the License.
-->

# Apache Buildish (Incubating) Proposal

## Abstract

Buildish develops and provides build automation, continuous integration (CI) integrations, and supporting tooling for Apache and Open Source projects — outside of the Apache Maven ecosystem.

The project offers a vendor-neutral, community-governed collection of Gradle plugins, GitHub Actions, Docker images, and build utilities. Its goal is to consolidate fragmented build infrastructure across the ASF into a single, well-maintained project that any Apache or Open Source project can depend on with confidence.

## Background

A large and growing number of Apache projects have moved beyond Apache Maven as their sole build system. Projects such as Apache Airflow, Apache Polaris, and many others rely on Gradle (and Gradle plugins), GitHub Actions workflows, custom shell scripts, and purpose-built Docker images to build, test, and release their software.

Today, the vast majority of these resources fall into one of two categories:

* **Vendor-provided** — maintained by commercial entities whose governance, roadmap, and licensing terms are outside the control of the ASF. This raises concerns about long-term sustainability: a vendor may change license terms, discontinue a product, or introduce features that conflict with Apache policies. Projects depending on these resources have limited recourse when changes occur.
* **Project-local** — developed and maintained independently within each project's own repository. This leads to significant duplication of effort: multiple projects solve the same problems (e.g., license header checks, release signing workflows, reproducible build environments) in slightly different and often incompatible ways. Bug fixes and improvements in one project do not benefit others. There is no consolidation, no shared maintenance, and no consistent quality standard.

This fragmentation imposes a real cost on the ASF ecosystem. Contributors spend time writing and maintaining build tooling that already exists elsewhere. New projects must bootstrap their CI/CD pipelines from scratch or copy patterns from other projects without ongoing maintenance support. Security patches and best-practice updates must be applied independently to each project.

Buildish aims to address this gap by serving as a well-governed, community-driven home for shared build resources — with clear Apache License 2.0 compliance enforced from the start. By providing a single, trusted source for common build infrastructure, Buildish lowers the barrier to entry for new projects, reduces maintenance overhead for existing ones, and raises the overall quality of build practices across the Foundation.

## Rationale

Buildish provides a central place to develop, maintain, and distribute high-quality build resources for the broader Apache and Open Source ecosystem. The project addresses a concrete and widely-felt need: Apache projects share many common build requirements, but today there is no ASF project dedicated to meeting them outside the Maven ecosystem.

Specifically, Buildish is the home for:

* **Gradle plugins** — common build logic, dependency management patterns, release tooling, and convention plugins tailored to ASF project needs. For example, plugins that enforce Apache license headers, configure reproducible builds, manage multi-module project conventions, integrate with Apache release processes, and generate NOTICE/LICENSE files. These plugins encode community-agreed best practices so that individual projects do not have to rediscover and re-implement them.

* **GitHub Actions** — reusable workflows and composite actions for CI/CD pipelines. This includes actions for license header verification, reproducible build validation, GPG release signing, SBOM (Software Bill of Materials) generation, dependency vulnerability scanning, and automated Apache release candidate staging. By centralizing these workflows, Buildish ensures that security updates and process improvements propagate to all consuming projects simultaneously.

* **Docker images** — standardized build environments and CI runner images that ensure reproducible builds across projects. These images provide pre-configured toolchains (JDK versions, Gradle distributions, native build tools) so that builds produce identical results regardless of where they run — whether on a developer's laptop, in GitHub Actions, or in a self-hosted CI environment. Images are versioned, tested, and published to a trusted registry.

* **Build tools and utilities** — scripts, Java-based CLI applications, and other utilities that support building, testing, packaging, and releasing software. This may include tools for validating release candidates against ASF policies, generating changelogs from commit history, managing multi-repository release coordination, and automating common maintenance tasks such as dependency updates.

By consolidating these resources under a single project with shared governance, Buildish reduces duplication, raises the quality bar, and makes it easier for projects to adopt best practices without reinventing the wheel. It also creates a natural gathering point for build engineering expertise within the ASF, fostering cross-project collaboration and knowledge sharing.

## Current Status

### Meritocracy

Buildish was initiated from discussions between several Apache committers who recognized the need for shared build infrastructure. As long-standing Apache committers, we deeply understand the Apache Way and are especially committed to meritocratic governance.

We are fully committed to open, transparent, and merit-based interactions with our community. All technical decisions will be made on the public mailing list. We intend to actively invite contributors from across the ASF and beyond, and we will follow the standard Apache process for granting commit access: contributors who demonstrate sustained, high-quality contributions and alignment with the project's goals will be nominated for committership by existing committers.

We recognize that build tooling touches many different domains — from JVM build systems to container orchestration to CI/CD automation — and we welcome expertise in all of these areas. Merit in Buildish is not limited to code contributions: documentation, testing, community support, and design discussions are all valued equally.

### Community

We recognize that cultivating a diverse and vibrant community is the single most important factor for the long-term success of Buildish. Because build tooling cuts across nearly every project in the ASF, we anticipate contributions from a uniquely wide range of projects and individuals.

Buildish is inherently a cross-cutting concern: improvements to a shared Gradle plugin or GitHub Action benefit dozens of downstream projects simultaneously. This creates a strong incentive for projects to contribute back, as their investment is amplified across the ecosystem. We expect this dynamic to drive organic community growth.

We plan to actively grow the community through several channels:
* Outreach to existing ASF projects that maintain their own build tooling, inviting them to contribute their solutions upstream to Buildish.
* Presentations at ApacheCon and other community events to raise awareness and attract contributors.
* Clear and comprehensive documentation, including contributor guides and tutorials, to lower the barrier to entry.
* Responsive and welcoming engagement on the dev mailing list and GitHub issues.

We believe that incubation at the ASF will help us attract contributors and users from a broad spectrum of Open Source projects — both within and outside the Foundation — and build a healthy, self-sustaining community around Buildish.

### Core Developers

All initial developers are committed to open source and have extensive experience across multiple ASF projects. The core team brings deep expertise in build systems, CI/CD automation, and Apache governance:

* **JB Onofré** — ASF Member, PMC member and committer on numerous Apache projects including Apache Karaf, Apache Camel, Apache ActiveMQ, and the ASF Incubator. Extensive experience with Gradle, Maven, and CI/CD systems.
* **François Papon** — ASF Member, committer on Apache Karaf and related projects. Strong background in build automation and Java ecosystem tooling.
* **Jarek Potiuk** — ASF Member, committer and PMC member on Apache Airflow. Deep experience with GitHub Actions, Docker-based builds, and CI/CD at scale.

This combination of expertise spans the full range of Buildish's intended scope — from JVM build systems to container-based CI/CD pipelines — and ensures that the project has the technical depth to deliver on its goals from day one.

### Alignment

Buildish is well-aligned with the Apache ecosystem. It both leverages various Apache projects (e.g., Apache Log4j for logging) and provides resources that directly benefit Apache projects throughout their build and release lifecycle.

The project occupies a complementary niche to existing ASF efforts:
* **Apache Maven** and the **Maven ecosystem** focus on the Maven build system and its plugin architecture. Buildish explicitly targets the non-Maven space (Gradle, GitHub Actions, Docker, etc.).
* **ASF Infra** provides foundational infrastructure services (Git hosting, CI runners, mailing lists). Buildish builds on top of this infrastructure by providing higher-level build resources and tooling that projects consume directly.
* **ASF Tooling** develops internal tools for ASF operations. Buildish focuses on build-time resources rather than operational tooling, though there may be natural areas of collaboration (e.g., release validation tools).

We plan to collaborate closely with ASF Infra and the ASF Tooling teams to ensure that Buildish complements existing infrastructure, follows established best practices, and does not duplicate effort. We welcome their guidance and will actively seek their input on design decisions that intersect with their domains.

## Known Risks

### Project Name

We performed a thorough name search for "Buildish" across major software registries, package managers, and search engines. The only result of note is a Progressive Web Application framework, which is unrelated to build automation or CI tooling. The name does not appear in the Maven Central, Gradle Plugin Portal, or Docker Hub registries in a conflicting context.

We also verified that the name does not conflict with any existing Apache project or podling. We believe Buildish is a distinctive, descriptive, and appropriate name for this project — it clearly conveys the project's focus on build-related tooling while remaining memorable and easy to spell.

### Orphaned Products

The risk of abandonment is low. Buildish was born from concrete, recurring needs across multiple ASF projects. These projects — including Apache Airflow, Apache Karaf, Apache Camel, and others — will be direct consumers of Buildish resources, providing a natural and ongoing incentive for continued development and maintenance.

Furthermore, the cross-cutting nature of build tooling means that Buildish serves a broad user base rather than a narrow niche. As long as ASF projects use Gradle, GitHub Actions, or Docker for their builds, there will be demand for the resources Buildish provides. The initial committers are also long-tenured ASF contributors with a track record of sustained involvement in the projects they maintain.

### Inexperience with Open Source

This risk does not apply. Buildish gathers exclusively veteran Apache members — including committers, PMC members, and ASF Members — all of whom have deep, hands-on experience with open source governance, community building, and the Apache Way.

The initial committers collectively have decades of experience participating in and leading Apache projects through incubation, graduation, and ongoing development. They are well-versed in the processes, expectations, and cultural norms of the ASF, including IP management, release processes, community decision-making, and conflict resolution.

### Homogeneous Developers

The initial committers are acting as individuals and represent a variety of ASF projects, technical backgrounds, and organizational affiliations. While the founding team is small, it spans different areas of the build tooling landscape — JVM build systems (Gradle), CI/CD automation (GitHub Actions), and containerized builds (Docker).

We are committed to broadening the committer base by actively welcoming contributors from different organizations, geographies, and areas of expertise. Because Buildish addresses needs shared across the entire ASF, we expect the contributor pool to naturally diversify as projects begin consuming and contributing to Buildish resources. We will also proactively reach out to underrepresented communities and projects to encourage participation.

### Reliance on Salaried Developers

Initial development will occur primarily on volunteer time. The core committers are contributing to Buildish based on their own initiative and their experience with the problems it aims to solve.

However, we expect that as Buildish matures and gains adoption, some contributors may receive employer support for their work. This is a natural and healthy pattern in the ASF — many successful Apache projects benefit from a mix of volunteer and employer-sponsored contributions. Importantly, no single employer currently directs or funds Buildish development, and we will actively work to maintain a diverse funding base to avoid single-vendor dependency.

### Relationship with Other Apache Products

Buildish has a symbiotic relationship with the broader Apache ecosystem:

* **As a consumer**: Buildish leverages Apache products as dependencies (e.g., Apache Log4j for logging) and builds upon the infrastructure provided by ASF Infra.
* **As a provider**: Buildish produces resources — Gradle plugins, GitHub Actions, Docker images, and utilities — that are consumed by other Apache projects to improve their build and release processes.

This bidirectional relationship creates a positive feedback loop: as more projects adopt Buildish resources, the incentive to contribute improvements back to Buildish grows, which in turn benefits all consuming projects.

We plan to collaborate with ASF Infra and the ASF Tooling teams, with their guidance, to ensure alignment and avoid duplication of effort. We see Buildish as a natural extension of the ASF's infrastructure ecosystem, filling a gap that currently exists between low-level CI infrastructure and project-specific build scripts.

### Excessive Fascination with the Apache Brand

While we expect the Apache brand to increase Buildish's visibility and help attract contributors, our decision to propose this project is driven by the technical and community needs described in the Rationale section — not by brand association.

The primary motivation for bringing Buildish to the ASF is governance, not branding. The ASF provides a proven framework for building vendor-neutral, community-driven projects with clear IP policies — exactly what is needed for build tooling that other projects depend on. Projects that depend on Buildish resources need confidence that those resources will remain freely available, well-governed, and Apache-licensed indefinitely. The ASF's governance model provides that assurance in a way that a personal GitHub repository cannot.

We believe Buildish will benefit greatly from the ASF's collaborative model and governance framework. We hope it will be embraced by Apache projects and the wider Open Source community alike.

## Documentation

Documentation is currently hosted alongside the source code in the project's GitHub repository:

https://github.com/jbonofre/buildish

As the project matures, we plan to develop comprehensive documentation including:
* A project website with getting-started guides and reference documentation.
* Per-component documentation (usage guides for each Gradle plugin, GitHub Action, and Docker image).
* Contributor guides and development setup instructions.
* Architecture and design decision records.

## Initial Source

The initial source code is hosted at:

https://github.com/jbonofre/buildish

The repository contains the initial project structure, build configuration, and early implementations of core components. All code has been developed by the initial committers and is ready for contribution to the ASF.

## Source and Intellectual Property Submission Plan

All source code currently hosted in the GitHub repository will be contributed to the ASF under the Apache License 2.0. The initial committers will sign Individual Contributor License Agreements (ICLAs), and any contributing organizations will sign Corporate Contributor License Agreements (CCLAs) as needed.

There are no encumbered dependencies or IP concerns. All code was written from scratch by the initial committers, and no third-party code with incompatible licenses has been incorporated.

Upon acceptance into the Incubator, the source code will be migrated to the Apache GitHub organization at https://github.com/apache/buildish.

## External Dependencies

All external dependencies are compatible with the Apache License 2.0. The project strives to minimize external dependencies and to prefer Apache-licensed libraries where possible.

Current dependencies:

| Dependency | License | Purpose |
|---|---|---|
| Apache Log4j | Apache License 2.0 | Logging framework |

Additional dependencies may be introduced as the project develops. All new dependencies will be reviewed for license compatibility with the Apache License 2.0 before inclusion, following the ASF's third-party licensing policy.

## Required Resources

### Mailing Lists

* **private@buildish.apache.org** — private PMC discussion, used only for matters that cannot be discussed publicly (e.g., security issues, personnel matters).
* **dev@buildish.apache.org** — primary development discussion list for technical design, code reviews, release planning, and community coordination.
* **commits@buildish.apache.org** — automated notifications for commits, pull requests, and CI activity.

### Git Repository

https://github.com/apache/buildish

The project will use a single mono-repository for all components (Gradle plugins, GitHub Actions, Docker images, and utilities). This simplifies cross-component development, testing, and versioning. Individual components will be published as separate artifacts.

### Issue Tracking

https://github.com/apache/buildish/issues

GitHub Issues will be used as the primary issue tracker, leveraging labels and milestones for organization. This aligns with the workflow that most contributors are already familiar with and integrates naturally with the pull request workflow.

## Initial Committers

* JB Onofré (jbonofre@apache.org)
* François Papon (fpapon@apache.org)
* Jarek Potiuk (potiuk@apache.org)

## Sponsors

### Champion

* JB Onofré (jbonofre@apache.org)

### Nominated Mentors

* JB Onofré (jbonofre@apache.org)
* François Papon (fpapon@apache.org)
* Jarek Potiuk (potiuk@apache.org)

### Sponsoring Entity

The Apache Incubator
