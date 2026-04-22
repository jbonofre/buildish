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

Buildish develops and provides build automation, continuous integration (CI) integrations, and supporting tooling for Apache and Open Source projects.

The project offers a vendor-neutral, community-governed collection of build tool plugins (Maven, Gradle, npm, pip/poetry/uv, etc.), CI provider integrations (GitHub Actions, Jenkins, GitLab CI, etc.), Docker images, and build utilities. The goal is to consolidate fragmented build infrastructure across the ASF into a single, well-maintained project that any Apache or Open Source project can depend on.

## Background

Apache projects use a wide variety of build systems and CI tooling. Projects such as Apache Airflow, Apache Polaris, and many others rely on Gradle, npm, pip, CI workflows (GitHub Actions, Jenkins, etc.), custom shell scripts, and purpose-built Docker images to build, test, and release their software.

Today, the vast majority of these resources fall into one of two categories:

* Vendor-provided: maintained by commercial entities whose governance, roadmap, and licensing terms are outside the control of the ASF. A vendor may change license terms, discontinue a product, or introduce features that conflict with Apache policies. Projects depending on these resources have limited recourse when changes occur.
* Project-local: developed and maintained independently within each project's own repository. Multiple projects solve the same problems (e.g., license header checks, release signing workflows, reproducible build environments) in slightly different and often incompatible ways. Bug fixes and improvements in one project do not benefit others.

This fragmentation also has security implications. Supply chain security is an increasing concern across the software industry. When each project independently maintains its own build scripts, CI pipelines, and release tooling, there is no single place to apply security fixes, audit dependencies, or enforce best practices. A vulnerability in a common build pattern may go unpatched in dozens of projects simply because there is no shared codebase to fix.

Buildish aims to address this gap by serving as a community-driven home for shared build resources, with Apache License 2.0 compliance and a security-first mindset from the start. A single, trusted source for common build infrastructure lowers the barrier to entry for new projects, reduces maintenance overhead for existing ones, and makes it possible to apply security improvements across the ecosystem in one place.

## Rationale

Apache projects share many common build requirements, but today there is no ASF project dedicated to providing shared, cross-project build infrastructure. Buildish fills that role. Specifically, it is the home for:

* Build tool plugins: plugins and extensions for build tools such as Maven, Gradle, npm, pip/poetry/uv, and others. For example, plugins that enforce Apache license headers, configure reproducible builds, manage multi-module project conventions, integrate with Apache release processes, and generate NOTICE/LICENSE files.

* CI provider integrations: reusable workflows and actions for CI/CD pipelines, targeting providers such as GitHub Actions, Jenkins, GitLab CI, and Codeberg/Forgejo. This includes integrations for license header verification, reproducible build validation, GPG release signing, SBOM (Software Bill of Materials) generation, dependency vulnerability scanning, supply chain verification, and automated release candidate staging.

* Docker images: standardized build environments and CI runner images. These provide pre-configured, hardened toolchains (JDK versions, build tool distributions, native build tools) so that builds produce identical results regardless of where they run. Images are versioned, scanned for vulnerabilities, tested, and published to a trusted registry.

* Build tools and utilities: scripts, Java-based CLI applications, and other utilities that support building, testing, packaging, and releasing software. This may include tools for validating release candidates against ASF policies, generating changelogs, managing multi-repository release coordination, and automating dependency updates.

* Reusable staging pipeline for single- or multi-repository documentation web-sites, renderer agnostic.

Consolidating these resources under shared governance also creates a natural gathering point for build engineering and supply chain security expertise within the ASF, fostering cross-project collaboration and knowledge sharing. When a security issue is found in a build pattern or dependency, the fix lands in one place and reaches all consuming projects through normal version updates.

## Current Status

### Meritocracy

Buildish was initiated from discussions between several Apache committers who recognized the need for shared build infrastructure. All technical decisions will be made on the public mailing list. We will follow the standard Apache process for granting commit access: contributors who demonstrate sustained, high-quality contributions and alignment with the project's goals will be nominated for committership by existing committers.

Merit in Buildish is not limited to code contributions: documentation, testing, community support, and design discussions are all valued equally.

### Community

Build tooling cuts across nearly every project in the ASF, so improvements to a shared build plugin or CI workflow benefit dozens of downstream projects at once. This creates a strong incentive for projects to contribute back, and we expect this dynamic to drive organic community growth.

We plan to grow the community through:
* Outreach to existing ASF projects that maintain their own build tooling, inviting them to contribute upstream.
* Presentations at ApacheCon and other community events.
* Clear documentation, including contributor guides and tutorials.
* Responsive engagement on the dev mailing list and GitHub issues.

### Core Developers

All initial developers have extensive experience across multiple ASF projects:

* JB Onofre: ASF Member, PMC member and committer on numerous Apache projects including Apache Karaf, Apache Camel, Apache ActiveMQ, and the ASF Incubator. Extensive experience with Gradle, Maven, and CI/CD systems.
* Francois Papon: ASF Member, committer on Apache Karaf and related projects. Strong background in build automation and Java ecosystem tooling.
* Jarek Potiuk: ASF Member, committer and PMC member on Apache Airflow. Deep experience with CI/CD systems (GitHub Actions, Jenkins), Docker-based builds, and CI/CD at scale.
* Robert Stupp: PMC member on Polaris and committer on Cassandra.  Deep experience with build systems (Gradle, Maven, et al), CI/CD systems (GitHub Actions, Jenkins), Docker-based builds, and CI/CD at scale.

### Alignment

Buildish is well-aligned with the Apache ecosystem. It both leverages various Apache projects (e.g., Apache Log4j for logging) and provides resources that directly benefit Apache projects throughout their build and release lifecycle.

The project occupies a complementary niche to existing ASF efforts:
* Apache Maven focuses on the Maven build system itself and its core plugin architecture. Buildish is not a build system; it provides higher-level, cross-project build resources (plugins for various build tools, CI integrations, Docker images, etc.).
* ASF Infra provides foundational infrastructure services (Git hosting, CI runners, mailing lists). Buildish builds on top of this infrastructure by providing higher-level build resources that projects consume directly.
* ASF Tooling develops internal tools for ASF operations. Buildish focuses on build-time resources rather than operational tooling, though there may be natural areas of collaboration (e.g., release validation tools).
* ASF Security handles vulnerability reports and coordinates fixes across Apache projects. Buildish can help on the preventive side by embedding supply chain security practices (dependency scanning, SBOM generation, signature verification) directly into the build tooling that projects already use.

We plan to collaborate closely with ASF Infra, ASF Tooling, and ASF Security to ensure Buildish complements existing infrastructure and does not duplicate effort.

## Known Risks

### Project Name

We searched for "Buildish" across major software registries, package managers, and search engines. The only result of note is a Progressive Web Application framework, which is unrelated to build automation or CI tooling. The name does not appear in Maven Central, Gradle Plugin Portal, or Docker Hub in a conflicting context, and does not conflict with any existing Apache project or podling.

### Orphaned Products

The risk of abandonment is low. Buildish was born from concrete, recurring needs across multiple ASF projects. Projects such as Apache Airflow, Apache Karaf, and Apache Camel will be direct consumers, providing an ongoing incentive for continued development. The initial committers are long-tenured ASF contributors with a track record of sustained involvement.

### Inexperience with Open Source

This risk does not apply. All initial committers are veteran Apache members, including PMC members and ASF Members, with deep experience in open source governance, community building, and the Apache Way.

### Homogeneous Developers

The initial committers are acting as individuals and represent a variety of ASF projects, technical backgrounds, and organizational affiliations. While the founding team is small, it spans build systems (Maven, Gradle, npm), CI/CD automation (GitHub Actions, Jenkins, etc.), and containerized builds (Docker). We are committed to broadening the committer base as the project gains adoption.

### Reliance on Salaried Developers

Initial development will occur primarily on volunteer time. No single employer currently directs or funds Buildish development. As the project matures, some contributors may receive employer support, which is a natural pattern in the ASF. We will work to maintain a diverse contributor base to avoid single-vendor dependency.

### Relationship with Other Apache Products

Buildish has a symbiotic relationship with the broader Apache ecosystem. It leverages Apache products as dependencies (e.g., Apache Log4j) and produces resources (build tool plugins, CI integrations, Docker images, utilities) that other Apache projects consume to improve their build and release processes.

### Excessive Fascination with the Apache Brand

The primary motivation for bringing Buildish to the ASF is governance, not branding. The ASF provides a proven framework for vendor-neutral, community-driven projects with clear IP policies. Projects that depend on Buildish resources need confidence that those resources will remain freely available, well-governed, and Apache-licensed. The ASF's governance model provides that assurance in a way that a personal GitHub repository cannot.

## Documentation

Documentation is currently hosted alongside the source code in the project's GitHub repository:

https://github.com/jbonofre/buildish

As the project matures, we plan to develop:
* A project website with getting-started guides and reference documentation.
* Per-component documentation (usage guides for each build tool plugin, CI integration, and Docker image).
* Contributor guides and development setup instructions.
* Architecture and design decision records.

## Initial Source

The initial source code is hosted at:

https://github.com/jbonofre/buildish

The repository contains the initial project structure, build configuration, and early implementations of core components. All code has been developed by the initial committers and is ready for contribution to the ASF.

## Source and Intellectual Property Submission Plan

All source code will be contributed to the ASF under the Apache License 2.0. The initial committers will sign ICLAs, and any contributing organizations will sign CCLAs as needed.

There are no encumbered dependencies or IP concerns. All code was written from scratch by the initial committers, and no third-party code with incompatible licenses has been incorporated.

Upon acceptance into the Incubator, the source code will be migrated to https://github.com/apache/buildish.

## Required Resources

### Mailing Lists

* private@buildish.apache.org: private PMC discussion (security issues, personnel matters).
* dev@buildish.apache.org: primary development discussion.
* commits@buildish.apache.org: automated notifications for commits, pull requests, and CI activity.

### Git Repository

https://github.com/apache/buildish

The project will initially use this repository. Individual components will be published as separate artifacts.

### Issue Tracking

https://github.com/apache/buildish/issues

## Initial Committers

* JB Onofre (jbonofre@apache.org)
* Francois Papon (fpapon@apache.org)
* Jarek Potiuk (potiuk@apache.org)
* Robert Stupp (snazy@apache.org)

## Sponsors

### Champion

* JB Onofre (jbonofre@apache.org)

### Nominated Mentors

* JB Onofre (jbonofre@apache.org)
* Francois Papon (fpapon@apache.org)
* Jarek Potiuk (potiuk@apache.org)

### Sponsoring Entity

The Apache Incubator
