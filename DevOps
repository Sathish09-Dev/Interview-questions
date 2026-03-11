Selected 10 Key Skills (final — do not change):

Cloud Platform Proficiency (AWS / Azure / GCP fundamentals)
Infrastructure as Code (Terraform / CloudFormation / ARM)
CI/CD Pipeline Design & Implementation (Jenkins / GitLab CI / Azure DevOps)
Cloud Services & Serverless (Storage, Compute, Cloud Functions, App Engine, Azure Functions)
Kubernetes & Container Orchestration (EKS / AKS / GKE, manifests, Helm)
Security & Key Management (encryption, certificates, KMS, Vault)
Cloud Migration & Hybrid Cloud Networking (VPNs, VNet/VPC design, connectivity)
Advanced IaC & Tooling (modular Terraform, Packer, HashiCorp Vault)
Self-Hosted Tooling & Operations (self-hosted CI, artifact repos, monitoring)
Troubleshooting, Resiliency & Monitoring (logging, observability, DR, HA patterns)
Deliverables to generate for each skill:

Section header with the skill name.
Subsection A: Five Theory-Based Questions (numbered 1–5).
Subsection B: Five Real-Time Scenario-Based Questions (numbered 1–5).
Formatting rules to follow in your output:

Use clear headings and subheadings.
Number all questions.
For scenario questions, when appropriate include example inputs, logs, or short config snippets the candidate can reason about (keep snippets short).
After the 10 skill sections, provide the 2-day preparation timetable as a structured schedule with start and end times for each block, objectives for the block, and expected deliverables (e.g., “practice: implement Terraform module that provisions an S3 bucket with encryption”).
At the top of the schedule, assume candidate is available each day from 09:00 to 19:00 local time, with 1-hour lunch and 15-minute breaks every 90 minutes. If a block requires hands-on cloud resources, state that the candidate should have an active cloud account and access to a terminal and editor.
Make the timetable prioritize higher-weight skills earlier and allocate time for mock interviews and review.
End the prompt with a short checklist the interviewer AI should use to evaluate candidate readiness after the 2-day plan (5 concise evaluation criteria with brief scoring guidance).
Now produce the full interview content as specified above: the 10 skill sections with 5 theory and 5 scenario questions each, followed by the 2-day preparation timetable and the evaluation checklist.

Skill 1 — Cloud Platform Proficiency (AWS / Azure / GCP fundamentals) A. Theory-Based Questions

Explain the shared responsibility model for cloud providers and identify which responsibilities typically fall on the customer for IaaS, PaaS, and SaaS.
Compare and contrast AWS Availability Zones and Regions with Azure’s Regions and Availability Zones and GCP’s Regions and Zones. When would you choose multi-region vs multi-AZ deployment?
Describe key cost optimization strategies available on major cloud platforms (reserved instances/savings plans, autoscaling, right-sizing, lifecycle policies).
Explain identity and access management (IAM) basics: policies, roles, service principals, and temporary credentials.
Define the concepts of object storage vs block storage and typical use-cases for each on cloud platforms.
B. Scenario-Based Questions

You need to design a three-tier web application for low-latency users in Europe with high availability. Sketch the high-level deployment across cloud regions/AZs and justify your choices.
Given the following AWS cost spike: EC2 compute cost jumped 40% month-over-month. Describe the steps you would take to investigate and remediate.
A service account key for a GCP project was accidentally committed to a public Git repo. List immediate mitigation steps and long-term preventative measures.
Your team needs to enforce least-privilege access across Azure subscriptions. Draft an approach (roles, management groups, policies) and an example role assignment flow.
On a time-limited demo environment, you must provision resources in under 15 minutes. Provide CLI commands (for any cloud) to create a small compute instance, attach a block volume, and open SSH access securely.
Skill 2 — Infrastructure as Code (Terraform / CloudFormation / ARM) A. Theory-Based Questions

Explain the benefits of IaC and how it improves reproducibility and collaboration.
Compare declarative vs imperative provisioning models and give examples (Terraform vs Ansible).
Describe Terraform state: what it stores, why it matters, and best practices for managing remote state.
Explain modules in Terraform and modular design principles.
Describe CloudFormation/ARM template structure and how change sets (or deployment diffs) work.
B. Scenario-Based Questions

You have a Terraform configuration that fails with "Error: cycle: resource A depends on resource B ...". Explain how you would detect and resolve circular dependency issues.
Provide a short Terraform module (pseudo-code) that provisions an encrypted S3 bucket (or equivalent) with versioning enabled and a lifecycle rule to expire old objects after 90 days.
Your team wants to roll back to a previous infrastructure version but state drift exists. Outline the steps to reconcile drift and perform a safe rollback.
A CloudFormation stack gets stuck in UPDATE_ROLLBACK_FAILED. Describe diagnostic steps and commands you would run to recover or continue deployment.
Demonstrate (CLI or pseudo commands) how to securely store and access Terraform remote state using a backend and lock mechanism.
Skill 3 — CI/CD Pipeline Design & Implementation (Jenkins / GitLab CI / Azure DevOps) A. Theory-Based Questions

Explain the stages of a typical CI/CD pipeline and the role of pipeline-as-code.
Compare GitHub/GitLab branching strategies (GitFlow, trunk-based) and their CI implications.
Describe how to implement secrets management in pipelines securely.
Explain pipeline parallelism and caching strategies to speed up builds.
Discuss rollback strategies for deployments (blue-green, canary, immutable) and trade-offs.
B. Scenario-Based Questions

Write a concise GitLab CI snippet that builds a Docker image, runs unit tests, and pushes to a registry only on merge to main.
A Jenkins pipeline frequently fails due to flaky integration tests which pass locally. Outline debugging steps and pipeline changes to reduce noise.
Design a CI/CD flow to deploy a microservice to Kubernetes with canary releases and automated rollback on increased error rates.
Your artifact repository is intermittent during pipeline runs, causing failures. Describe steps to add resiliency to the pipeline (retries, caching, local mirrors).
Implement (pseudo-commands or YAML) how to integrate a security scanner into the pipeline that blocks merges on critical vulnerabilities.
Skill 4 — Cloud Services & Serverless (Storage, Compute, Cloud Functions, App Engine, Azure Functions) A. Theory-Based Questions

Explain differences between serverless functions and containerized microservices; include scaling, cold starts, and state management considerations.
Describe storage classes (e.g., S3 Standard, Infrequent Access, Glacier) and when to use each.
Explain how autoscaling works for managed compute services and key metrics used to scale.
Describe event-driven architecture basics and typical triggers for serverless functions.
Discuss limitations and best practices for using managed platform services like App Engine or Azure App Service.
B. Scenario-Based Questions

Design a serverless pipeline where an uploaded file triggers a function to process data and store results in a managed DB. Include error handling and retries.
A function in production is experiencing high cold-start latency. Propose immediate and long-term mitigations and quantify expected impact.
Given a storage bucket lifecycle policy snippet that’s not deleting objects as expected, show how you would debug (include sample CLI commands or checks).
You need to migrate a legacy batch process to cloud-native services with minimal downtime. Outline the migration strategy and orchestration method.
A compute autoscaling group is not scaling out under load. Provided metrics show CPU at 10% and request latency rising. Explain possible reasons and remediation steps.
Skill 5 — Kubernetes & Container Orchestration (EKS / AKS / GKE, manifests, Helm) A. Theory-Based Questions

Explain Kubernetes core components (control plane, kubelet, kube-proxy) and the lifecycle of a pod.
Describe Kubernetes Service types (ClusterIP, NodePort, LoadBalancer) and when to use each.
Explain declarative config via manifests and the purpose of Helm charts.
Discuss RBAC in Kubernetes and how to limit access to cluster resources.
Describe common pod scheduling features: node selectors, taints/tolerations, affinities.
B. Scenario-Based Questions

Given this pod manifest (short snippet) which keeps restarting, identify likely causes and commands to debug: apiVersion: v1 kind: Pod metadata: {name: app} spec: containers:
name: app image: myapp:latest env:
name: CONFIG valueFrom: secretKeyRef: name: app-secret key: config
Design an approach to deploy a stateful database on Kubernetes (or recommend managed alternative), including backup, scaling, and HA considerations.
Create a Helm values example and explain how you would use Helm to manage multiple environment overlays (dev, staging, prod).
A Deployment reports desired=5, available=2. Show step-by-step kubectl commands and checks to identify the root cause.
Explain how you would implement a multi-tenant cluster isolation model (namespaces, network policies, resource quotas) and give a simple NetworkPolicy example.
Skill 6 — Security & Key Management (encryption, certificates, KMS, Vault) A. Theory-Based Questions

Explain encryption at rest vs in transit and typical mechanisms used in cloud platforms.
Describe certificate lifecycle management and tools to automate issuance and rotation (ACME, cert-manager).
Compare cloud-native KMS (AWS KMS, Azure Key Vault, GCP KMS) vs HashiCorp Vault in terms of features and use-cases.
Explain secrets injection patterns for applications running in containers and serverless.
Describe principles of least privilege and how they apply to service accounts and IAM policies.
B. Scenario-Based Questions

A compliance audit requires all S3 buckets to be encrypted with customer-managed keys. Provide Terraform (or ARM/CloudFormation) pseudo-code to enforce this and steps to remediate non-compliant buckets.
You detect that an old API key with broad privileges was used in a production incident. Describe immediate steps and how to rotate keys safely with minimal service disruption.
Provide commands or config snippets showing how to configure an application in Kubernetes to fetch secrets from HashiCorp Vault via an injector or CSI driver.
A TLS certificate expired on a public-facing load balancer. You have certificate automation but it's failing. Show diagnostic steps and a recovery plan to restore secure traffic quickly.
Design a key rotation policy for KMS keys used to encrypt databases, including automation, versioning, and rollback considerations.
Skill 7 — Cloud Migration & Hybrid Cloud Networking (VPNs, VNet/VPC design, connectivity) A. Theory-Based Questions

Explain the key considerations when designing hybrid connectivity (latency, bandwidth, redundancy, security).
Compare site-to-site VPN, Direct Connect/ExpressRoute, and SD-WAN for hybrid cloud connectivity.
Describe subnetting and IP planning best practices for VPC/VNet designs to avoid collisions with on-prem networks.
Explain routing concepts used in cloud networks (route tables, peering, transit gateways).
Discuss strategies for migrating on-prem stateful workloads to cloud with minimal downtime.
B. Scenario-Based Questions

Your on-prem data center must connect to multiple cloud regions with low latency and failover. Propose an architecture using transit gateways / virtual hubs and explain failover behavior.
Show configuration snippets (pseudo) to set up a site-to-site VPN between an on-prem router and an AWS VPC, including shared key and routing considerations.
During migration, a subset of services show packet loss to cloud endpoints. Outline troubleshooting steps and checks across layers (physical, routing, security groups, ACLs).
You must plan IP addressing to accommodate future mergers (multiple CIDR ranges). Propose a scheme and explain how to prevent CIDR conflicts during peering.
Design a phased migration plan for a 3-tier stateful application, minimizing downtime and ensuring data consistency.
Skill 8 — Advanced IaC & Tooling (modular Terraform, Packer, HashiCorp Vault) A. Theory-Based Questions

Explain advantages of modular Terraform (DRY, reuse, versioning) and how to structure a module registry.
Describe how Packer works and typical use-cases for immutable images.
Explain HashiCorp Vault core concepts: secrets engines, auth methods, leases, and dynamic secrets.
Discuss testing strategies for IaC (unit tests, integration tests, policy-as-code like Sentinel or Open Policy Agent).
Explain state management strategies when using multiple workspaces or environments.
B. Scenario-Based Questions

Provide a high-level Terraform module layout for multi-environment deployments and explain how to handle environment-specific variables securely.
Design a Packer template (pseudo JSON/HCL) to build a golden AMI with preinstalled monitoring agents and security hardening steps.
Vault: Describe how to provide short-lived DB credentials to an application and include example API or CLI steps to request and revoke credentials.
Your CI pipeline must validate Terraform code and run a non-destructive plan. Outline the pipeline steps, tools, and policies to enforce before apply.
You need to migrate Terraform state to a new backend with locking. Provide commands and steps to perform migration safely, including validation.
Skill 9 — Self-Hosted Tooling & Operations (self-hosted CI, artifact repos, monitoring) A. Theory-Based Questions

List pros and cons of self-hosting CI/CD and artifact repositories versus using managed SaaS offerings.
Explain capacity planning considerations for self-hosted runners/agents.
Describe backup and restore strategies for artifact repositories and CI configuration.
Discuss monitoring key metrics for CI systems and artifact repos (queue length, job duration, disk I/O).
Explain multi-tenant considerations and access controls for self-hosted developer tooling.
B. Scenario-Based Questions

Your self-hosted GitLab runner fleet is overwhelmed during peak builds. Propose scaling and optimization measures and include quick configuration snippets where relevant.
An artifact repository's storage is nearing capacity and GC is taking too long. Outline an approach to safely free space and improve retention policies.
Design a monitoring dashboard (high-level list of widgets/metrics) to observe CI health and job performance, and list alerting thresholds.
A recent upgrade to the CI master caused several pipelines to break. Describe rollback and troubleshooting steps, including safety measures before upgrading.
Implement (pseudo-steps) a secure backup and restore plan for your self-hosted Jenkins instance including secrets and plugins.
Skill 10 — Troubleshooting, Resiliency & Monitoring (logging, observability, DR, HA patterns) A. Theory-Based Questions

Define observability and explain differences between logs, metrics, and traces.
Explain RTO and RPO and how they influence disaster recovery planning.
Describe common HA patterns at application and infrastructure level (load balancing, active-active, active-passive).
Discuss structured logging best practices and correlation IDs.
Explain how to design SLOs and SLIs and tie them to alerting and incident response.
B. Scenario-Based Questions

Production users report intermittent errors with a 500 spike. Given example logs showing: [2026-03-10T12:04:15Z] ERROR user=123 svc=payments err="timeout: upstream-service" Describe an investigation plan and immediate mitigations.
Your monitoring shows degraded performance in one AZ only. List steps to isolate whether the issue is networking, storage, or compute related.
Design a DR runbook for a critical database that supports failover to a secondary region with an RPO of 5 minutes.
A recent deployment increased tail latency. Show how you would use distributed tracing and metrics to locate the root cause and propose a rollback or fix plan.
You must craft alerts to avoid noise but catch real incidents. Provide three example alert rules (with thresholds) and explain suppression and escalation logic.
