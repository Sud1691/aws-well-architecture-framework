# Pattern-2-API-First-Infrastructure-Platform

## Purpose

This document covers **API-first infrastructure platforms** from the perspective of a **Senior Platform Engineer**.

The goal is to understand how a platform team can expose infrastructure capabilities as **productized, self-service APIs** rather than relying on:

- manual tickets
- ad hoc Terraform execution
- informal Slack requests
- one-off platform intervention
- uncontrolled direct cloud access

For a Senior Platform Engineer, this pattern matters because it sits at the center of modern internal platform design.

It is where several concerns come together:

- self-service enablement
- governance
- workflow orchestration
- abstraction design
- platform safety
- auditability
- policy enforcement
- standardization without excessive central bottlenecks

A strong interview answer should show that you understand both sides of the problem:

- how to make infrastructure easy to consume
- how to keep that ease from becoming unmanaged sprawl

---

## What “API-First Infrastructure” Actually Means

API-first infrastructure means platform capabilities are exposed through **well-defined contracts** that application or product teams can consume programmatically.

Instead of saying:

- “Open a ticket for a database”
- “Ask the platform team to create a queue”
- “Copy this Terraform and modify it yourself”
- “Ping someone to approve an IAM role”

the platform provides a clear interface such as:

- `POST /databases`
- `POST /queues`
- `POST /service-identities`
- `POST /environments`
- `POST /network-access-requests`

This does **not** necessarily mean every workflow must be a public HTTP API in the narrow sense. The idea is broader:

- infrastructure is requested through defined contracts
- inputs are validated
- workflows are orchestrated
- policies are enforced
- outputs and lifecycle state are visible
- the process is auditable and repeatable

That is the essence of infrastructure-as-an-API.

---

## Why This Pattern Matters for a Senior Platform Engineer

At small scale, infrastructure can be managed through direct access and informal coordination.

At larger scale, that breaks down.

Without API-first thinking, organizations often end up with:

- platform teams acting as ticket fulfillment engines
- inconsistent resource creation patterns
- duplicated Terraform usage
- weak governance
- poor discoverability of approved patterns
- fragile human approval chains
- low delivery speed
- unclear ownership

An API-first platform solves for this by turning platform capabilities into **reusable products**.

The deeper value is not just automation.
It is:

- consistency
- controlled delegation
- standardization
- discoverability
- embedded governance
- operational traceability

That is why this pattern is especially relevant in senior platform interviews.

---

## The First Principle: Platform APIs Should Expose Intent, Not Raw Cloud Complexity

This is one of the most important design principles.

Bad platform APIs expose the underlying cloud provider almost directly.

Examples of bad abstraction:

- “Choose your subnet IDs”
- “Provide 17 IAM policy options”
- “Specify every database parameter group manually”
- “Select all security group rules yourself”
- “Pass in raw Terraform variables copied from a module”

That is not really a platform product.
That is just thin wrapping around infrastructure internals.

A stronger API expresses **intent**.

Examples:

- “Create a PostgreSQL database for an internal service”
- “Provision an ephemeral test environment”
- “Request read-only access from service A to service B”
- “Create an event bus for asynchronous integration”
- “Enable internet ingress for this public API”

Then the platform maps that intent to approved infrastructure patterns.

A Senior Platform Engineer should care deeply about this distinction because it determines whether the platform reduces complexity or simply relocates it.

---

## Core Questions to Ask When Designing an API-First Platform

Before choosing services, you need clarity on platform design questions.

### 1. Who are the consumers?

Examples:

- application teams
- ML platform users
- data engineering teams
- SRE teams
- internal developer portal workflows
- CI/CD automation systems

This matters because different consumers need different levels of abstraction and guardrails.

### 2. What are the highest-value self-service capabilities?

Do not start with “how do we API-enable everything?”

Start with:

- which requests are most common
- which requests create the most friction
- which patterns should be standardized
- which manual workflows cause the most delay or risk
- which capabilities have clear golden-path designs

Examples of high-value capabilities:

- database provisioning
- service identity creation
- environment creation
- event infrastructure
- DNS registration
- secret bootstrap
- ingress setup
- policy-compliant storage creation

### 3. What must remain centrally controlled?

Not everything should be self-service.

Examples that may need stricter platform/security control:

- organization-level IAM boundaries
- account vending strategy
- cross-account network architecture
- foundational shared services
- critical security exceptions
- sensitive data access models

A good platform answer includes both **self-service boundaries** and **central control boundaries**.

### 4. What is the lifecycle model?

Provisioning is only one part of the story.

You also need to think about:

- updates
- deletion
- drift detection
- ownership changes
- approval workflows
- deprecation
- status visibility
- retries and compensation logic

A weak platform design focuses only on create.
A mature one handles the full lifecycle.

### 5. What metadata matters?

This is often overlooked and extremely important.

Examples:

- service owner
- environment
- compliance tier
- cost center
- data sensitivity
- operational tier
- RTO/RPO class
- region requirements
- expiration date for ephemeral resources

Good metadata enables:

- automation
- governance
- cost reporting
- incident routing
- policy enforcement
- discoverability

This is one of the clearest places where platform engineering becomes product engineering.

---

## A Reference Example: POST /api/databases

A useful mental model is:

> A user calls `POST /api/databases` with `{ type: "postgres", size: "medium" }`

The real question is:
what happens behind the scenes?

A mature API-first platform does not simply create an RDS instance immediately.
It usually performs a sequence like:

1. authenticate and authorize the caller
2. validate the request schema
3. enrich the request with team/service metadata
4. check policy constraints
5. determine the approved provisioning pattern
6. orchestrate infrastructure creation
7. configure secrets, networking, backups, and monitoring
8. record audit and ownership information
9. return request status and identifiers
10. emit lifecycle events for other systems

That flow is far more realistic than “API Gateway invokes Lambda and creates a DB.”

Interviewers often want to see whether you understand this full control flow.

---

## The Main AWS Building Blocks for API-First Infrastructure Platforms

The point is not that these are the only services you can use.
The point is understanding the role each layer plays.

---

## 1. API Gateway

### Why it matters

API Gateway is often the entry point for infrastructure requests.

It provides:

- request ingestion
- authentication integration
- throttling
- request transformation
- API contract exposure
- usage plans and access control options

### What it is good for

- exposing self-service APIs
- acting as the front door for internal platform capabilities
- integrating with Lambda, Step Functions, or internal backends
- giving teams a documented programmable interface

### What a strong answer should include

A mature answer should mention that API Gateway is only the front door.
It should not be treated as the whole platform.

The hard parts are behind it:

- orchestration
- policy
- idempotency
- state management
- lifecycle handling

---

## 2. Lambda

### Why it matters

Lambda often works well for:

- request handlers
- lightweight validation
- metadata enrichment
- dispatch logic
- event processing
- glue code between services

### Where it fits

Examples:

- validating database requests
- mapping request intent to platform blueprints
- looking up team metadata
- publishing request events
- updating provisioning status
- invoking downstream orchestration

### What a strong answer should include

Lambda is useful, but not every platform workflow should become a pile of Lambdas.

A good answer should show discipline:

- use Lambda for bounded logic
- avoid overcomplicated orchestration inside Lambda code
- push multi-step workflows into explicit state machines where appropriate

That sounds much more senior.

---

## 3. Step Functions

### Why it matters

Step Functions are extremely useful for **multi-step provisioning workflows**.

They help orchestrate things like:

- validation
- approval
- provisioning
- retries
- rollback or compensation steps
- asynchronous waits
- human-in-the-loop branching
- state visibility

### Why they are valuable in platform engineering

Infrastructure workflows are often:

- long-running
- failure-prone
- dependent on multiple systems
- partially asynchronous

Examples:

- create database
- wait for availability
- inject secret
- register monitoring
- update CMDB or service catalog entry
- notify the requester
- emit success/failure event

This is exactly the kind of workflow Step Functions handle better than ad hoc code chains.

### What a strong answer should include

A strong answer should mention that Step Functions improve:

- observability of workflow state
- retry behavior
- operational debugging
- explicit control flow

That is especially useful in infrastructure automation, where failures must be understandable and recoverable.

---

## 4. EventBridge

### Why it matters

EventBridge is important when you want the platform to be **event-driven** instead of tightly coupled.

Examples:

- provisioning request accepted
- environment created
- resource ready
- approval granted
- secret rotated
- policy violation detected
- provisioning failed

### Why this matters in a platform

An API-first platform should often emit events that downstream systems can consume.

Examples of consumers:

- audit systems
- notification systems
- inventory systems
- cost allocation pipelines
- security scanners
- documentation portals
- incident or change-management tooling

### What a strong answer should include

A good answer should frame EventBridge as part of a broader platform pattern:

- APIs handle intent submission
- workflows orchestrate provisioning
- events broadcast lifecycle changes

That is a much richer model than synchronous request/response automation only.

---

## 5. Service Catalog

### Why it matters

Service Catalog can be used to expose **approved infrastructure patterns** in a controlled way.

It is useful when you want:

- pre-approved templates
- curated product offerings
- controlled provisioning
- versioned infrastructure products
- governance around who can request what

### Where it fits

Examples:

- standard database products
- standard VPC-connected application patterns
- pre-approved analytics workspaces
- golden-path infrastructure stacks

### What a strong answer should include

A good answer should avoid overselling Service Catalog as a complete internal platform.

It is one useful building block, especially for curated provisioning, but most mature platforms also need:

- richer workflow logic
- policy integration
- metadata systems
- lifecycle visibility
- event integration
- custom APIs or portals

That nuance matters.

---

## 6. Systems Manager Parameter Store

### Why it matters

Parameter Store can act as a foundational configuration layer in some platform workflows.

Examples:

- environment configuration
- service bootstrap values
- approved config references
- platform-level dynamic settings
- request-time configuration lookup

### Important nuance

Parameter Store is useful for configuration, but it is not a universal substitute for:

- secret lifecycle management
- product metadata systems
- workflow state tracking
- policy engines

A mature answer keeps it in the right role.

---

## A Broader Platform Control Flow

A clean way to explain this in an interview is with a layered request flow.

### Example flow

1. user or automation calls platform API
2. API Gateway authenticates the caller
3. Lambda or backend service validates and enriches the request
4. policy checks determine whether the request is allowed
5. Step Functions orchestrates provisioning
6. provisioning may invoke IaC pipelines, Service Catalog products, or direct AWS service integrations
7. EventBridge emits lifecycle events
8. status is written to a control-plane store
9. secrets, monitoring, tagging, and ownership are configured
10. caller receives status and can query progress later

This is a strong answer because it separates:

- ingress
- validation
- policy
- orchestration
- execution
- state
- observability

That separation is a sign of platform maturity.

---

## API-First Does Not Mean “No IaC”

This is a critical point.

A common misunderstanding is:

- APIs are the platform
- IaC is separate

In reality, the API-first model often sits **on top of IaC**.

The platform API becomes the controlled consumer interface.
Behind it, the actual provisioning may still be driven by:

- Terraform
- CloudFormation
- CDK-generated templates
- Service Catalog products
- custom controllers
- Kubernetes operators

That is often the best model.

### Why this matters

IaC remains essential for:

- reproducibility
- reviewability
- drift control
- module reuse
- consistency across environments

The API layer adds:

- productized consumption
- validation
- policy enforcement
- workflow orchestration
- lifecycle visibility

A strong Senior Platform Engineer answer should explicitly connect these two layers.

---

## Important Design Principles for API-First Infrastructure Platforms

These are the principles that make the difference between a usable platform and a thin automation wrapper.

---

## 1. Design around golden paths

Start with common, repeated, high-value patterns.

Examples:

- standard PostgreSQL database
- standard public API service
- standard private service with service-to-service auth
- standard queue or event topic
- standard ephemeral preview environment

Golden paths reduce ambiguity and let the platform team embed safe defaults.

---

## 2. Enforce policy early

Do not wait until after provisioning to discover violations.

Examples of policy checks:

- allowed regions
- data classification restrictions
- approved instance classes
- naming rules
- mandatory metadata
- environment-specific constraints
- backup requirements

A strong platform does not create first and audit later unless there is a very good reason.

---

## 3. Make requests idempotent

Infrastructure APIs must handle retries safely.

Without idempotency, you risk:

- duplicate resources
- inconsistent state
- broken workflows
- hard-to-debug user experience

This is especially important in long-running or partially asynchronous provisioning flows.

---

## 4. Separate control plane from execution plane

The platform control plane handles:

- requests
- metadata
- policy
- status
- lifecycle logic

The execution plane handles:

- actual provisioning
- cloud operations
- deployments
- resource updates

This separation improves clarity, scale, and security.

---

## 5. Expose lifecycle state, not just success/failure

Infrastructure provisioning is rarely instantaneous.

Users need to see states such as:

- pending validation
- awaiting approval
- provisioning
- failed
- ready
- deleting
- drift detected
- deprecated

A mature platform feels like a product because users can understand state and progress.

---

## 6. Build metadata in from day one

Metadata is not decoration.

It is how the platform knows:

- who owns something
- why it exists
- what policies apply
- who gets paged
- how to allocate cost
- when to expire it
- what resilience tier it belongs to

Without good metadata, self-service quickly becomes operational debt.

---

## 7. Prefer declarative intent over procedural commands

Better request:

- “I need a medium PostgreSQL database for a production service.”

Weaker request:

- “Run these 11 steps to configure networking, storage, backups, and access.”

Intent-based APIs make it easier to:

- evolve the underlying implementation
- improve defaults over time
- hide cloud-specific complexity
- standardize outcomes

---

## 8. Design for safe failure and compensation

Provisioning workflows fail in the real world.

Examples:

- DB created but secret injection failed
- policy passed but capacity unavailable
- monitoring registered but DNS step failed
- approval granted after request became stale

A mature platform thinks about:

- rollback
- cleanup
- retry policies
- partial completion handling
- operator intervention paths

That is a strong signal of seniority.

---

## What Should Be Self-Service vs Centrally Managed?

This is a high-value interview topic.

A good platform answer is not “make everything self-service.”

### Strong candidates usually say:

Self-service should cover:
- common, low-ambiguity, repeatable infrastructure patterns
- capabilities with strong guardrails
- requests where standardization adds speed and safety

Examples:
- managed databases with approved tiers
- queues and topics
- service accounts or workload identities
- standard storage buckets
- ephemeral test environments
- DNS or ingress requests under policy

Central control should remain stronger for:
- org-level IAM guardrails
- foundational network topology
- account vending
- cross-account trust design
- sensitive exceptions
- high-risk policy overrides

This answer shows judgment, not ideology.

---

## Failure Modes in API-First Infrastructure Platforms

Senior answers get much stronger when they include failure modes.

### 1. Thin abstraction over cloud internals

Symptoms:
- consumers still need deep AWS knowledge
- too many exposed knobs
- platform feels harder than direct cloud usage

### 2. Ticket system disguised as an API

Symptoms:
- API submits request but humans still manually do the work
- no real lifecycle visibility
- poor standardization
- central team remains bottleneck

### 3. Weak metadata model

Symptoms:
- ownership unclear
- cost attribution broken
- policy decisions inconsistent
- poor incident routing

### 4. Orchestration without idempotency

Symptoms:
- duplicate resources
- retry chaos
- stuck workflows
- partial state corruption

### 5. Policy bolted on after the fact

Symptoms:
- frequent rework
- compliance failures
- broken user experience
- platform loses trust

### 6. No update/delete lifecycle

Symptoms:
- platform creates resources well but cannot evolve or retire them safely
- drift and orphaned infrastructure grow over time

### 7. Platform API becomes too generic

Symptoms:
- every request becomes custom
- golden paths disappear
- support burden rises
- internal platform stops being a product and becomes a consultancy team

These are exactly the kinds of insights that make an answer sound grounded.

---

## A Senior Platform Engineer’s Design Approach

If asked how you would build infrastructure-as-an-API, a strong structure is:

### Step 1: Identify the highest-friction repeated requests

Start with things like:
- databases
- environments
- service identities
- event infrastructure
- ingress
- storage

### Step 2: Define golden-path products

For each one:
- define allowed inputs
- define defaults
- define metadata requirements
- define policy boundaries
- define lifecycle states

### Step 3: Build a control-plane workflow

Use components such as:
- API Gateway for ingress
- Lambda or service layer for validation and request handling
- Step Functions for orchestration
- EventBridge for lifecycle events
- IaC backends for actual provisioning

### Step 4: Add observability and auditability

Track:
- request status
- provisioning duration
- failure points
- policy denials
- ownership
- change history

### Step 5: Limit scope deliberately

Do not try to API-ify everything at once.
Start with the highest-value requests and mature the platform incrementally.

That is a very believable senior answer.

---

## Example Interview Scenario

### Scenario

A company wants to reduce the platform team’s ticket load. Application teams commonly request:
- PostgreSQL databases
- S3 buckets
- service IAM roles
- internal DNS entries

Leadership asks: “How would you build a self-service infrastructure platform on AWS?”

### Strong way to reason through it

I would start by identifying the most common requests and turning those into productized golden paths rather than exposing raw cloud primitives.

For example, instead of giving teams direct Terraform modules with many optional parameters, I would define platform APIs around intent, such as creating a standard PostgreSQL database, a policy-compliant storage bucket, or a service identity with approved access boundaries.

I would use API Gateway as the front door for internal APIs, but keep the real complexity in the control plane behind it. Request handlers would validate input, attach ownership and environment metadata, and run policy checks before provisioning begins.

For multi-step workflows such as database provisioning, backup registration, secret creation, and monitoring setup, I would use Step Functions so that workflow state, retries, and partial failures are visible and manageable.

The actual provisioning layer could still rely on Terraform, CloudFormation, or Service Catalog, because I would not want to lose the consistency and reviewability of IaC. The API layer is for productized consumption and control, not for replacing infrastructure-as-code.

I would emit lifecycle events through EventBridge so that audit systems, portals, cost systems, and notification services can react without tightly coupling everything to one synchronous workflow.

I would also be careful not to make the API too generic. The goal is not to expose all AWS features indirectly. The goal is to provide a small set of high-value paved roads with strong defaults, metadata, lifecycle visibility, and policy enforcement.

That answer shows:
- product thinking
- workflow design
- IaC integration
- governance
- practical scoping

---

## Model Q&A

### Q1. What does API-first infrastructure really mean?

It means infrastructure capabilities are exposed through defined, consumable contracts with validation, policy enforcement, orchestration, and lifecycle visibility, rather than through informal requests or raw cloud access.

### Q2. Why not just give teams Terraform modules?

Because raw module consumption often pushes too much cloud complexity to consumers, creates inconsistency, and does not solve workflow, policy, metadata, audit, or lifecycle visibility problems by itself.

### Q3. Does API-first replace IaC?

No. In most strong designs, APIs sit on top of IaC. The API is the productized control surface, while Terraform, CloudFormation, or similar tools remain the provisioning engine.

### Q4. Why are Step Functions useful here?

Because infrastructure workflows are often long-running, multi-step, partially asynchronous, and failure-prone. Step Functions make orchestration, retries, and workflow state explicit.

### Q5. What is the biggest risk in API-first platform design?

Building abstractions that are either too thin to add value or too generic to remain supportable. The best platforms expose a small number of high-value, well-governed golden paths.

### Q6. What metadata should the platform require?

At minimum:
- owner
- environment
- cost center or business unit
- compliance or data sensitivity tier
- service name
- operational tier
- expiration where relevant

### Q7. What should remain centrally controlled?

Typically:
- org-level IAM guardrails
- core network design
- account structure
- sensitive policy exceptions
- foundational shared services

---

## What Interviewers Are Really Testing

In API-first platform questions, interviewers are often testing whether you can:

- think of the platform as a product
- balance self-service with governance
- separate control plane from execution details
- connect APIs to workflows, metadata, and policy
- understand that provisioning is only one part of lifecycle management
- avoid both overexposed cloud complexity and overengineered abstraction
- build repeatable internal platform capabilities instead of human bottlenecks

That is why the strongest answers include:

- consumer model
- abstraction model
- workflow orchestration
- policy enforcement
- metadata design
- lifecycle state
- IaC integration
- scoped golden paths

---

## Final Takeaway

An API-first infrastructure platform is not just “AWS automation behind an endpoint.”

A mature platform should:

- expose intent-based self-service capabilities
- embed policy and metadata from the start
- orchestrate provisioning safely
- preserve IaC as the provisioning backbone
- emit lifecycle events
- provide visibility into state and ownership
- focus on high-value paved roads rather than generic cloud passthrough

For a Senior Platform Engineer, the most impressive answer is usually not the one with the most services.

It is the one that shows how to turn infrastructure from **manual coordination** into a **reliable, governed, productized internal capability**.
