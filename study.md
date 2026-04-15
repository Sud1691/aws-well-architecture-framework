# Platform Engineering Interview Study Plan

## 2-Week Program for High-Level Architecture Mastery

**Goal:** Build mental models for platform architecture discussions and system design interviews at companies like NVIDIA.

**Time Commitment:** 90 minutes/day  
**Format:** 30 min study + 30 min diagram + 30 min verbal explanation  
**Rules:** No code generation during study. Paper/whiteboard only. No AI assistance during exercises.

-----

## Week 1: Foundational Mental Models

### Day 1: State Machines (Lifecycle Management)

**Why it matters:** Every deployment, health check, and circuit breaker is a state machine.

**Core Concept:**

```
State = what you remember between events
Event = something that happened (API call, timeout, metric threshold)
Transition = state changes based on event
Action = side effect (alert, deploy, rollback)
```

**Study Materials:**

- Circuit breaker pattern (CLOSED → OPEN → HALF_OPEN)
- Deployment states (PENDING → DEPLOYING → HEALTHY → FAILED)
- Pod lifecycle (Pending → Running → Succeeded/Failed)

**Exercise 1.1: Design deployment gate state machine**
Requirements:

- Parse terraform plan output
- States: ANALYZING → WAITING_APPROVAL → APPLYING → COMPLETE
- Events: plan_parsed, approval_granted, apply_success, apply_failure
- When do you transition? What triggers rollback?

**Exercise 1.2: Pod health checker**
Requirements:

- States: STARTING → READY → DEGRADED → FAILING
- Events: health_check_success, health_check_failure
- Track consecutive failures (3 failures → FAILING state)

**Deliverable:** State diagram on paper with all transitions labeled

-----

### Day 2: Event-Driven Architecture (Decoupling)

**Why it matters:** Platform services must not block each other. Async is critical at scale.

**Core Concept:**

```
Synchronous: A calls B, A waits for B to finish (blocking)
Asynchronous: A publishes event to queue/bus, returns immediately. B processes later.

Use async when:
- B's latency shouldn't block A's response (Slack alerts)
- B might be down, A should still work (monitoring sidecar)
- Multiple consumers need same event (EventBridge → Lambda + SNS + Kinesis)
```

**Study Materials:**

- EventBridge event routing patterns
- SQS vs SNS vs EventBridge (when to use which)
- Lambda async invocation vs synchronous

**Exercise 2.1: Secret rotation propagation**
Requirements:

- Vault rotates DB password (event source)
- Need to: update K8s secrets (3 namespaces), restart pods, verify health, alert on failure
- Design event flow: What publishes? What consumes? What runs sync vs async?

**Exercise 2.2: Terraform apply audit trail**
Requirements:

- When terraform apply runs, log to: S3, CloudWatch, Slack, DataDog
- Should terraform wait for all logging to complete?
- Design event-driven flow with failure handling

**Deliverable:** Event flow diagram showing publishers, queue/bus, consumers, failure paths

-----

### Day 3: Failure Modes & Retries (Resilience)

**Why it matters:** Every external dependency (Vault, K8s API, Slack) can fail. You must handle it.

**Core Concept:**

```
Transient failure: Network blip, rate limit → Retry succeeds
Permanent failure: Service down, 404 → Retry won't help

Retry strategies:
- Exponential backoff: 1s, 2s, 4s, 8s (best for rate limits)
- Jitter: add randomness to prevent thundering herd
- Circuit breaker: stop retrying after N failures, give service time to recover
```

**Study Materials:**

- AWS SDK retry behavior (default backoff)
- Kubernetes API server rate limiting
- Vault token expiration handling

**Exercise 3.1: Init container secret fetching**
Requirements:

- Pod init container fetches secret from Vault before main container starts
- Vault might be: slow (500ms), rate-limiting (429), down (503)
- Design retry logic: How many attempts? What backoff? When do you give up?
- What’s the pod state during retries? (Init:CrashLoopBackOff)

**Exercise 3.2: ArgoCD sync with timeout**
Requirements:

- Deploy via GitOps, wait for sync completion
- ArgoCD API might be: slow, returning 503, stuck syncing
- Design timeout strategy: When do you fail the deployment? How do you alert?

**Deliverable:** Retry flow diagram with backoff calculation, timeout thresholds, failure exit paths

-----

### Day 4: Resource Pools & Quotas (Multi-tenancy)

**Why it matters:** Shared platforms need resource isolation to prevent noisy neighbors.

**Core Concept:**

```
Pool pattern:
- Pre-allocate limited resources (IP addresses, CPU, memory)
- Enforce quotas per tenant (prevent resource exhaustion)
- Track usage for chargeback/showback

Common resource pools:
- Database connection pools (max 100 connections)
- Kubernetes ResourceQuota (10 CPU per namespace)
- VPC CIDR blocks (each team gets /24 subnet)
- NAT Gateway IP pools (limited elastic IPs)
```

**Study Materials:**

- Kubernetes ResourceQuota and LimitRange
- RDS connection pool sizing
- VPC CIDR allocation strategies

**Exercise 4.1: Multi-tenant namespace quotas**
Requirements:

- Platform supports 50 teams, each gets a namespace
- Each namespace limited to: 10 CPU, 20GB RAM, 5 load balancers
- How do you enforce? What happens when team hits limit?
- Design quota request workflow (team needs more CPU)

**Exercise 4.2: Database connection pool exhaustion**
Requirements:

- RDS instance supports 100 connections
- 20 microservices share this database
- Each service creates 10 connections at startup
- What happens? (200 connections needed, only 100 available)
- Design connection pooling strategy

**Deliverable:** Quota enforcement architecture with request/approval workflow

-----

### Day 5: API Gateway Patterns (Control Plane)

**Why it matters:** Platform teams expose infrastructure via APIs, not raw Terraform/kubectl.

**Core Concept:**

```
Developer → API Gateway → Backend Service → Infrastructure (K8s, Terraform, Vault)

API Gateway provides:
- Authentication (JWT, mTLS)
- Rate limiting (prevent abuse)
- Request validation (schema enforcement)
- Audit logging (compliance)
- Circuit breaking (protect backend)
```

**Study Materials:**

- AWS API Gateway vs ALB (when to use which)
- API Gateway authorizers (Lambda, Cognito)
- Rate limiting strategies (token bucket, fixed window)

**Exercise 5.1: Namespace provisioning API**
Requirements:

- Developers hit POST /namespaces → platform creates K8s namespace + RBAC + quotas
- Should this be sync or async? (What if namespace creation takes 30s?)
- How do you authenticate? (JWT from Okta?)
- How do you rate limit? (10 requests/hour per team?)

**Exercise 5.2: API Gateway vs ALB decision**
Scenarios:

- Internal platform API (only VPC access needed)
- Public-facing API (internet access, need DDoS protection)
- WebSocket endpoint (real-time deployment logs)
  For each scenario: API Gateway or ALB? Why?

**Deliverable:** API architecture diagram with auth flow, rate limiting, backend routing

-----

### Day 6: Secrets Management Architecture

**Why it matters:** Apps need secrets. Platform provides them securely without manual kubectl.

**Core Concept:**

```
Secrets flow: Vault (source of truth) → K8s Secret → Pod

Delivery methods:
- Init container: Fetch at startup, write to file/env
- Sidecar: Continuously sync (handles rotation)
- External Secrets Operator: Controller watches Vault, updates K8s Secret

Rotation without downtime:
- Dual-password support (old + new valid for 5 min)
- Rolling restart (10% pods at a time)
- Health check before marking rotated
```

**Study Materials:**

- Vault Kubernetes auth method
- External Secrets Operator vs Vault Agent
- K8s Secret vs ConfigMap (when to use which)

**Exercise 6.1: Secret rotation propagation**
Requirements:

- DB password rotates every 30 days
- 200 microservices use this password
- Can’t restart all 200 at once (capacity limit: 20 pods/min)
- Design zero-downtime rotation

**Exercise 6.2: Secrets delivery comparison**
Compare:

- Init container + Vault Agent
- External Secrets Operator
- CSI Secret Store Driver
  For each: pros/cons, failure modes, operational complexity

**Deliverable:** Secret rotation flow diagram with gradual rollout strategy

-----

### Day 7: Week 1 Review & Synthesis

**Capstone Exercise (90 min):**
Design a **deployment gate system** that blocks risky Terraform applies.

**Requirements:**

1. Parse `terraform plan` output (count resources created/changed/destroyed)
1. Calculate risk score (destroyed = 10 points, changed = 2, created = 1)
1. If score >20, require manual approval via Slack
1. Track last 10 deployments in memory
1. If >3 failures in last hour, auto-escalate to senior engineer
1. Retry Slack webhook POST with exponential backoff

**Deliverable:**

- Architecture diagram (components, data flow)
- State machine (ANALYZING → WAITING_APPROVAL → APPLYING → COMPLETE/FAILED)
- Event flow (where is async decoupling needed?)
- Failure handling (Slack down, approval timeout, apply fails)
- Resource tracking (what state do you store? where?)

**Do not write code.** Design only. Use Week 1 patterns.

-----

## Week 2: Platform Architecture Patterns

### Day 8: Multi-Region Active-Active

**Why it matters:** Enterprise platforms need geo-redundancy and low latency globally.

**Core Concept:**

```
User → Route53 (latency-based) → Regional ALBs → Regional EKS clusters

Challenges:
- Data consistency (how to replicate state?)
- Failover detection (what triggers traffic shift?)
- Blast radius (bad deploy shouldn't hit all regions)
- Cost (running 3x infrastructure)
```

**Study Materials:**

- Route53 routing policies (latency, geoproximity, failover)
- RDS cross-region read replicas
- DynamoDB Global Tables
- EKS multi-region patterns

**Exercise 8.1: API with regional databases**
Requirements:

- API in us-east-1, eu-west-1, ap-southeast-1
- RDS primary in us-east-1, read replicas in other regions
- Writes go to primary, reads from local replica
- If primary fails, promote eu-west-1

Questions to answer:

- How does Route53 detect region health?
- What happens during primary failover? (Write downtime?)
- How do you prevent split-brain? (Two primaries accepting writes)
- Progressive rollout: How do you deploy safely across regions?

**Exercise 8.2: Multi-region Kubernetes workload**
Requirements:

- Same application running in 3 EKS clusters (one per region)
- Shared state in DynamoDB Global Table
- Deploy to us-east-1 first, then eu-west-1, then ap-southeast-1
- If error rate >1% in any region, halt rollout

Questions to answer:

- How do you orchestrate cross-region deploys? (Step Functions? Custom controller?)
- How do you measure error rate per region?
- Rollback strategy if region 2 fails after region 1 deployed?

**Deliverable:** Multi-region architecture diagram with failover flow

-----

### Day 9: Internal Developer Platform (Golden Paths)

**Why it matters:** Self-service reduces platform team toil and speeds up developers.

**Core Concept:**

```
Golden Path = Pre-configured, secure, compliant infrastructure pattern

Examples:
- "Standard web service" → ALB + ECS + RDS + monitoring + secrets
- "Batch job" → Lambda + EventBridge + DLQ
- "Data pipeline" → S3 + Glue + Athena

Developer experience:
- CLI: platform create service my-api --type=web
- API: POST /services {name: "my-api", type: "web"}
- UI: Web portal with form
```

**Study Materials:**

- Platform engineering principles (Team Topologies)
- Backstage.io (Spotify’s IDP framework)
- Terraform modules as building blocks

**Exercise 9.1: Self-service EKS namespace**
Requirements:

- Developer runs: platform create namespace my-app
- Platform provisions:
  - Namespace with ResourceQuota (10 CPU, 20GB RAM)
  - NetworkPolicy (only allow traffic from ingress)
  - PodSecurityStandard (restricted)
  - Vault policy for secrets
  - Prometheus scrape config

Questions to answer:

- How do you enforce guardrails? (OPA Gatekeeper?)
- How do you prevent quota exhaustion?
- Namespace deletion workflow (check for running pods first?)
- Multi-cluster routing (dev vs staging vs prod clusters?)

**Exercise 9.2: Golden path adoption metrics**
Requirements:

- Track which teams use golden paths vs custom infra
- Measure: time-to-first-deploy, incident rate, cost efficiency
- Goal: 80% of teams on golden paths within 6 months

Questions to answer:

- How do you collect adoption metrics?
- How do you incentivize golden path usage?
- What if a team has valid reasons to deviate?

**Deliverable:** Golden path architecture with guardrails and self-service flow

-----

### Day 10: CI/CD Pipeline Design (Enterprise Scale)

**Why it matters:** Fast, reliable pipelines are the foundation of developer productivity.

**Core Concept:**

```
Pipeline stages (fail fast principle):
1. Lint + Unit Tests (fast, no infrastructure needed)
2. Build artifacts (only if stage 1 passes)
3. Integration tests (expensive, requires test environment)
4. Deploy to staging (async, monitored)
5. Deploy to prod (gated by approval + health checks)

Key decisions:
- Where does pipeline run? (GitHub Actions, GitLab CI, Jenkins)
- How do you store artifacts? (ECR, Artifactory)
- How do you handle secrets? (Vault, AWS Secrets Manager)
- How do you gate prod deploys? (Manual approval, automated canary)
```

**Study Materials:**

- GitHub Actions vs GitLab CI vs Jenkins
- Artifact versioning strategies (semver, git SHA)
- Progressive delivery (blue/green, canary, feature flags)

**Exercise 10.1: Terraform pipeline with cost gates**
Requirements:

- Developer opens PR with Terraform changes
- Pipeline runs:
1. terraform plan
1. Cost estimation (Infracost API)
1. If cost increase >$500/month, require approval
1. On merge, terraform apply

Questions to answer:

- Where does pipeline run? (GitHub Actions? Self-hosted runner?)
- How do you store Terraform state? (S3 + DynamoDB locking)
- Approval workflow (Slack webhook? GitHub review?)
- Blast radius containment (how to prevent bad apply from affecting multiple environments?)

**Exercise 10.2: Microservices build pipeline**
Requirements:

- 200 microservices, each deploys 5-10x/day
- Build time target: <5 minutes
- Need: Docker build, push to ECR, deploy to K8s via ArgoCD

Questions to answer:

- Monorepo or polyrepo? (How does this affect build triggers?)
- Build caching strategy (Docker layer cache? Remote cache?)
- Parallel builds (can you build 10 services simultaneously?)
- Deploy orchestration (all services at once? Progressive per-service?)

**Deliverable:** CI/CD pipeline diagram with stages, gates, and artifact flow

-----

### Day 11: Observability Architecture (3 Pillars)

**Why it matters:** You can’t operate what you can’t observe. Multi-tenant platforms need per-team visibility.

**Core Concept:**

```
Three pillars:

Metrics: "What's happening?" (counters, gauges, histograms)
  Examples: error_rate, p95_latency, pod_count
  Tools: Prometheus, CloudWatch Metrics

Logs: "Why did it happen?" (debug info, stack traces)
  Examples: Error logs, audit logs, access logs
  Tools: Fluentd, CloudWatch Logs, Elasticsearch

Traces: "Where's the bottleneck?" (distributed request flow)
  Examples: Which service in a chain is slow?
  Tools: AWS X-Ray, Jaeger, OpenTelemetry
```

**Study Materials:**

- Prometheus label best practices
- CloudWatch Logs Insights queries
- Distributed tracing correlation (trace IDs)

**Exercise 11.1: Multi-tenant metrics**
Requirements:

- Platform hosts 50 teams, each with 10 microservices
- Need: Per-team dashboards, cross-team aggregates, cost attribution

Questions to answer:

- How do you partition metrics by team? (Prometheus labels: team=“team-a”)
- Log aggregation: Centralized or per-team? (CloudWatch log groups per team?)
- Tracing: How do you correlate requests across services? (X-Trace-ID header)
- Cost attribution: How do you track per-team spending? (AWS tags)

**Exercise 11.2: Alert fatigue prevention**
Requirements:

- Platform generates 1000+ alerts/day across all teams
- Many are false positives or low priority
- Need: Alert routing, de-duplication, escalation

Questions to answer:

- How do you route alerts to correct team? (Metadata in alert payload)
- De-duplication strategy (same alert firing 10x in 5 min → send once)
- Escalation policy (P1 to PagerDuty immediately, P3 to Slack with delay)

**Deliverable:** Observability architecture with metrics/logs/traces collection and routing

-----

### Day 12: Security & Compliance (Platform Guardrails)

**Why it matters:** Platform team enforces security policies so app teams don’t have to become experts.

**Core Concept:**

```
Defense in depth layers:

1. Network: VPC, security groups, NACLs
2. Identity: IAM roles, RBAC, OIDC
3. Secrets: Vault, AWS Secrets Manager, encrypted at rest
4. Compute: Pod security policies, container scanning
5. Data: Encryption at rest and in transit
6. Audit: CloudTrail, K8s audit logs, access logs
```

**Study Materials:**

- AWS Well-Architected Security Pillar
- Kubernetes Pod Security Standards
- OPA Gatekeeper policies

**Exercise 12.1: Pod Security enforcement**
Requirements:

- Block pods that: run as root, use hostPath, are privileged
- Allow: non-root user, read-only root filesystem, limited capabilities

Questions to answer:

- How do you enforce? (OPA Gatekeeper, Pod Security Standards)
- What happens when developer tries to deploy non-compliant pod? (Rejected with error message)
- Exception workflow (team needs privileged pod for valid reason?)

**Exercise 12.2: Secrets access audit**
Requirements:

- Track which services access which secrets
- Alert if: service accesses secret it shouldn’t, unusual access pattern
- Compliance requirement: 90-day retention of all access logs

Questions to answer:

- How does Vault log access? (Audit logs to CloudWatch)
- Anomaly detection (service suddenly accessing 50 secrets → alert)
- Compliance reporting (generate monthly access report per team)

**Deliverable:** Security architecture with policy enforcement points and audit flow

-----

### Day 13: Cost Optimization & FinOps

**Why it matters:** Cloud spend can spiral out of control. Platform team owns cost guardrails.

**Core Concept:**

```
FinOps principles:

1. Visibility: Tag everything, track per-team/per-service
2. Optimization: Right-size instances, use Spot, reserved capacity
3. Governance: Quotas, budgets, alerts on overspend
4. Chargeback: Attribute costs back to teams

Common cost drivers:
- Over-provisioned EC2/RDS instances
- Idle resources (dev clusters running 24/7)
- Data transfer (cross-region, cross-AZ)
- Unoptimized storage (S3 lifecycle policies)
```

**Study Materials:**

- AWS Cost Explorer
- Kubernetes resource requests vs limits
- Spot instance best practices

**Exercise 13.1: Cost attribution system**
Requirements:

- 50 teams sharing AWS account
- Need: Monthly cost report per team
- Breakdown: EC2, RDS, S3, data transfer

Questions to answer:

- How do you tag resources? (team=team-a tag on all resources)
- Shared resources (how to split cost of shared NAT Gateway?)
- Kubernetes workloads (how to attribute EKS node costs to teams based on namespace usage?)

**Exercise 13.2: Cost anomaly detection**
Requirements:

- Detect when team’s weekly spend increases >50%
- Alert team immediately with breakdown
- Suggest optimization actions

Questions to answer:

- Data source (AWS Cost and Usage Reports?)
- Anomaly detection logic (compare current week to 4-week average?)
- Alert content (which service caused spike? link to AWS console)

**Deliverable:** Cost visibility architecture with tagging strategy and alerting flow

-----

### Day 14: System Design Capstone

**Full Interview Scenario (90 min):**
Design a **CI/CD platform for 200 microservices**

**Requirements:**

- 200 services across 20 teams
- Each service deploys 5-10x/day to dev, staging, prod
- Need: Fast feedback (<5 min build), progressive rollout, auto-rollback on errors
- Constraints: AWS only, Kubernetes for compute, GitOps for deploys

**Your Task:**

### 1. High-Level Architecture (30 min)

Draw component diagram showing:

- Entry point (GitHub webhook triggers build?)
- Build orchestration (GitHub Actions? Jenkins?)
- Artifact storage (ECR for Docker images)
- GitOps tool (ArgoCD, Flux?)
- Deployment pipeline (dev → staging → prod progression)
- Observability (metrics, logs, traces)

### 2. Design Decisions (30 min)

For each decision, explain trade-offs:

**API Gateway vs ALB for platform APIs?**

- API Gateway: Built-in auth, rate limiting, request validation
- ALB: Cheaper, WebSocket support, but need custom auth
- Decision: ?

**Sync vs async namespace creation?**

- Sync: User waits for response (30s timeout risk)
- Async: Return job ID, user polls status
- Decision: ?

**EventBridge vs direct API calls between platform services?**

- EventBridge: Decoupled, durable, multi-consumer
- Direct calls: Simple, fast, but tightly coupled
- Decision: ?

**Kubernetes multi-cluster vs single large cluster?**

- Multi: Better isolation, blast radius containment
- Single: Simpler operations, resource sharing
- Decision: ?

### 3. Failure Scenarios (20 min)

How do you handle:

- GitHub is down → Can teams still deploy?
- ArgoCD stuck syncing → How to detect and alert?
- Build takes >30 min → Timeout and notification?
- RDS primary fails during deployment → Impact on pipeline?

### 4. Security & Compliance (10 min)

- Where do secrets live? (Vault)
- Who can deploy to prod? (RBAC via Okta + SAML)
- How do you audit all platform actions? (CloudTrail + K8s audit logs)
- How do you prevent privilege escalation?

**Deliverable:**

- Architecture diagram (all components, data flow, failure paths)
- 1-page design doc (key decisions, trade-offs, open questions)
- State machine for deployment lifecycle
- Security boundaries diagram

-----

## Daily Study Format (90 min)

### Phase 1: Concept Study (30 min)

- Read the day’s pattern and scenarios
- Study linked AWS/K8s documentation
- Review real-world examples (open-source projects, blog posts)

### Phase 2: Architecture Design (30 min)

- Close all references
- Draw architecture diagram on paper for the exercise
- Label all components, data flows, failure paths
- Annotate with decision rationale

### Phase 3: Verbal Explanation (30 min)

- Record yourself explaining the architecture out loud
- Pretend you’re in an interview: “Let me walk you through this design…”
- Play back the recording
- Note: unclear explanations, missing details, weak justifications
- Re-record unclear sections

-----

## Success Criteria

By end of Week 2, you should be able to:

✅ **Explain component choices** — Why API Gateway over ALB? Why EventBridge over SQS?

✅ **Trace failure paths** — What breaks when Vault is down? How do you detect and recover?

✅ **Articulate trade-offs** — Sync vs async? Centralized vs distributed? Cost vs performance?

✅ **Design with constraints** — Cost limits, latency SLAs, compliance requirements, security boundaries

✅ **Think in layers** — Network → Compute → Data → Application → Observability

✅ **Scale considerations** — What works for 10 services breaks at 200. What changes?

-----

## Interview Readiness Checklist

Before your next interview round:

### Architecture Fluency

- [ ] Can draw component diagram for any platform pattern in <10 min
- [ ] Can explain why each component is needed (no “because that’s what we use”)
- [ ] Can propose 2-3 alternative architectures and compare trade-offs

### Failure Mode Analysis

- [ ] For any component, can list 3+ failure scenarios
- [ ] Can explain detection mechanism (health checks, metrics, alerts)
- [ ] Can propose remediation (retry, failover, rollback, escalation)

### Communication Clarity

- [ ] Can explain architecture without jargon (pretend interviewer is PM, not engineer)
- [ ] Can zoom in/out (high-level flow → implementation detail → back to high-level)
- [ ] Can pause and ask: “Does this make sense before I continue?”

### Production Thinking

- [ ] Every design includes: auth, rate limiting, audit logging
- [ ] Every external call includes: timeout, retry, circuit breaker
- [ ] Every stateful component includes: backup, recovery, migration path

-----

## Red Flags to Avoid

### ❌ Don’t generate code during interviews

Platform engineering interviews are architecture discussions, not coding tests.

### ❌ Don’t memorize architectures

Memorized diagrams are obvious. You’ll fail when they ask “what if X changes?”

### ❌ Don’t skip failure scenarios

Saying “we’ll use EventBridge” without explaining what happens when EventBridge is down = incomplete design.

### ❌ Don’t over-engineer

“We’ll use 5 microservices with Kafka and…” Stop. Start simple. Add complexity only when justified.

### ❌ Don’t say “I don’t know” and stop

Say: “I don’t know the exact API, but here’s how I’d figure it out…” and reason through it.

-----

## Post-Study: Interview Simulation

After completing Week 2:

1. **Record yourself** solving Day 14 capstone (90 min, no references)
1. **Watch the recording** — Would you hire this person?
1. **Identify gaps** — What did you struggle to explain?
1. **Repeat** the weak scenarios from Week 2

**Goal:** Explain any platform pattern fluently, without notes, in <10 minutes.

-----

## Resources (Reference Only, Not During Exercises)

### AWS Documentation

- Well-Architected Framework (all 6 pillars)
- EKS Best Practices Guide
- EventBridge patterns and use cases
- API Gateway vs ALB comparison

### Kubernetes

- Resource management (requests, limits, quotas)
- Pod Security Standards
- Network Policies
- Admission Controllers (OPA Gatekeeper)

### Platform Engineering

- Team Topologies book (platform team patterns)
- Backstage.io documentation (IDP examples)
- CNCF landscape (tool categories)

### Blog Posts / Case Studies

- Spotify platform engineering blog
- Netflix engineering blog (Spinnaker, Titus)
- Datadog engineering blog (multi-tenancy at scale)

-----

## Final Note: The Meta-Skill

This study plan builds **technical communication** — the ability to transfer a mental model of a system into someone else’s head using only words and diagrams.

**This is THE skill for senior platform roles:**

- Design reviews with principal engineers
- Incident post-mortems with leadership
- Architecture RFCs for cross-team alignment
- Mentoring junior engineers

Code is just the artifact. **Reasoning is what gets you hired.**

-----

Good luck. Now close this doc and start Day 1.