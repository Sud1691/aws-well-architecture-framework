# 4-Week Study Plan: NVIDIA Technical Rounds Prep

**Assumption:** Round 2 scheduled in ~2 weeks, remaining rounds over following 2-3 weeks

---

## Week 1 (March 18-24): Foundation Building

### Monday - Terraform Mastery (90 min)

**Morning (45 min): Review Your Work**
- [ ] Open EIP `infra/terraform/` directory
- [ ] Review: `dynamodb.tf`, `eventbridge.tf`, `iam.tf`, `secrets.tf`
- [ ] For each file, answer: "Why did I design it this way?"
- [ ] List 3 decisions you'd change if rebuilding

**Evening (45 min): Study Advanced Patterns**
- [ ] Read: [Terraform State Best Practices](https://developer.hashicorp.com/terraform/language/state)
- [ ] Focus on: Remote state, locking, workspaces
- [ ] Write down: "How would I manage state for 100 teams?"

**Practice Question (Write Answer):**
*"Design a Terraform module structure for a multi-account, multi-region AWS setup with 50 development teams. How do you handle state? Drift detection? Module versioning?"*

---

### Tuesday - Kubernetes Security (90 min)

**Morning (45 min): RBAC Deep Dive**
- [ ] Review: [K8s RBAC documentation](https://kubernetes.io/docs/reference/access-authn-authz/rbac/)
- [ ] Study: Role vs ClusterRole, RoleBinding vs ClusterRoleBinding
- [ ] Draw diagram: "How would you give Team A access to namespace A but not namespace B?"

**Evening (45 min): Network Policies & Pod Security**
- [ ] Read: [Network Policies](https://kubernetes.io/docs/concepts/services-networking/network-policies/)
- [ ] Read: [Pod Security Standards](https://kubernetes.io/docs/concepts/security/pod-security-standards/)
- [ ] Write scenario: "Block all egress except to specific external API"

**Practice Question:**
*"Design RBAC for a multi-tenant K8s cluster with 10 teams. Requirements: Teams can't see each other's namespaces, platform team has cluster-admin, security team can audit all namespaces."*

---

### Wednesday - AWS Multi-Region Architecture (90 min)

**Morning (45 min): RDS Multi-AZ vs Multi-Region**
- [ ] Review your answer from hiring manager question
- [ ] Study: [RDS Multi-AZ](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Concepts.MultiAZ.html)
- [ ] Study: [RDS Cross-Region Read Replicas](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_ReadRepl.XRgn.html)
- [ ] Create comparison table: cost, RTO, RPO, use cases

**Evening (45 min): Failover Strategies**
- [ ] Read: [Route53 Health Checks](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/dns-failover.html)
- [ ] Study: Active-Active vs Active-Passive
- [ ] Sketch architecture: "Multi-region app with automatic failover"

**Practice Question:**
*"You need 99.99% uptime for a customer-facing API. Design multi-region architecture with RDS, S3, and ALB. Explain failover strategy, data replication, and cost implications."*

---

### Thursday - API-First Infrastructure (90 min)

**Morning (45 min): REST API Design**
- [ ] Design API schema for infrastructure provisioning:
```
  POST /api/databases
  POST /api/compute/ec2
  POST /api/kubernetes/namespaces
  GET /api/infrastructure/{id}/status
  DELETE /api/infrastructure/{id}
```
- [ ] For each endpoint: request schema, response schema, error codes
- [ ] Think through: authentication, rate limiting, validation

**Evening (45 min): Review Your Tools**
- [ ] EIP API structure (`/api/risk/score`, `/api/architecture/services`)
- [ ] How would you expose DNA Sequencing as API?
- [ ] Write OpenAPI spec for one endpoint

**Practice Question:**
*"Design a REST API for developers to provision AWS infrastructure. Requirements: RDS, EC2, S3, VPC. Include: authentication, validation, async provisioning, status tracking, cost estimation before provisioning."*

---

### Friday - Your Production Tools (90 min)

**Morning (45 min): EIP Deep Dive**
- [ ] Run EIP locally, test all endpoints
- [ ] Review `eip/core/provider_registry.py` - explain stub-first pattern
- [ ] Review `eip/pillars/risk_engine/scorer.py` - explain algorithm
- [ ] Prepare: "Walk me through EIP architecture" (4-minute answer)

**Evening (45 min): DNA Sequencing Deep Dive**
- [ ] Run DNA Sequencing end-to-end demo
- [ ] Review `infra_dna_sequencer.py` - explain cross-layer detection
- [ ] Prepare example: "Secret rotates, deployment doesn't update, pods crash"
- [ ] Practice: "How does DNA Sequencing catch config drift?" (3-minute answer)

**Practice Question:**
*"Compare EIP and DNA Sequencing. What problem does each solve? How do they complement each other? If you could only build one for NVIDIA, which and why?"*

---

### Weekend (March 23-24): Rest + Light Review

**Saturday:**
- [ ] No intense studying
- [ ] Skim your notes from the week
- [ ] Watch 1-2 YouTube videos on topics you found confusing
- [ ] Exercise, relax, recharge

**Sunday:**
- [ ] 30-minute review: Re-read your practice question answers
- [ ] 30-minute mock: Answer one question out loud, record yourself
- [ ] Rest of day: Off

---

## Week 2 (March 25-31): Depth + Polish

### Monday - Terraform Testing & Validation (90 min)

**Morning (45 min): Terraform Testing**
- [ ] Read: [Terratest](https://terratest.gruntwork.io/)
- [ ] Read: [Terraform Test](https://developer.hashicorp.com/terraform/language/tests)
- [ ] Write example: Unit test for a Terraform module

**Evening (45 min): Policy as Code**
- [ ] Read: [OPA (Open Policy Agent)](https://www.openpolicyagent.org/)
- [ ] Study: Sentinel for Terraform Cloud
- [ ] Write policy: "Prevent S3 buckets without encryption"

**Practice Question:**
*"How would you ensure all Terraform modules created by 50 teams comply with security standards? Include: validation, testing, enforcement, feedback loop."*

---

### Tuesday - K8s Upgrades & Operations (90 min)

**Morning (45 min): Cluster Upgrades**
- [ ] Study: [EKS Upgrade Process](https://docs.aws.amazon.com/eks/latest/userguide/update-cluster.html)
- [ ] Review: Node group upgrades, in-place vs blue-green
- [ ] Document: Zero-downtime upgrade strategy

**Evening (45 min): Disaster Recovery**
- [ ] Study: [Velero](https://velero.io/) for K8s backup/restore
- [ ] Review: etcd backup/restore
- [ ] Plan: "Complete cluster failure - how to recover?"

**Practice Question:**
*"You need to upgrade 20 EKS clusters from v1.28 to v1.30. Each cluster runs production workloads. Design the upgrade strategy, rollback plan, and validation approach."*

---

### Wednesday - AWS Cost Optimization (90 min)

**Morning (45 min): Cost Analysis**
- [ ] Study: [AWS Cost Explorer](https://docs.aws.amazon.com/cost-management/latest/userguide/ce-what-is.html)
- [ ] Review: Reserved Instances, Savings Plans, Spot Instances
- [ ] Calculate: When does Reserved Instance make sense?

**Evening (45 min): Right-Sizing**
- [ ] Study: [AWS Compute Optimizer](https://aws.amazon.com/compute-optimizer/)
- [ ] Review: CloudWatch metrics for utilization
- [ ] Design: Automated right-sizing recommendation system

**Practice Question:**
*"Your AWS bill is $100K/month. Management wants 20% reduction without impacting performance. How would you approach this? Tools, methodology, risks."*

---

### Thursday - Secrets Management Deep Dive (90 min)

**Morning (45 min): Vault vs AWS Secrets Manager**
- [ ] Read: [HashiCorp Vault](https://developer.hashicorp.com/vault/docs)
- [ ] Focus on: Dynamic secrets, secret rotation, auth methods
- [ ] Compare: Vault vs AWS Secrets Manager (features, cost, ops overhead)

**Evening (45 min): K8s Secrets Integration**
- [ ] Review: [External Secrets Operator](https://external-secrets.io/)
- [ ] Study: Sealed Secrets, CSI Secret Store Driver
- [ ] Your Argo CD setup: How do you handle secrets?

**Practice Question:**
*"Design secrets management for 10K engineers across 100 microservices. Requirements: Rotation every 30 days, audit trail, K8s integration, minimal ops overhead. Vault? AWS? Hybrid?"*

---

### Friday - System Design Practice (90 min)

**Full Session: Mock System Design**

Pick one, spend 90 min designing:

**Option 1: Multi-Tenant Developer Platform**
*"Design a platform where developers can self-service provision: databases, compute, storage, K8s namespaces. 1000 developers, 200 teams, multi-account AWS."*

**Option 2: Secrets Rotation System**
*"Design a system that auto-rotates all secrets (DB passwords, API keys, certs) every 30 days without breaking applications. 500 services, 50 databases, 100 external APIs."*

**Option 3: Infrastructure Drift Detection**
*"Design a system to detect drift between declared state (Terraform) and actual state (AWS). Scale: 10K resources, 50 accounts. Requirements: real-time detection, auto-remediation option."*

**Structure your answer:**
1. Requirements clarification (5 min)
2. High-level architecture (20 min)
3. Data model (15 min)
4. API design (15 min)
5. Scale considerations (15 min)
6. Trade-offs and alternatives (20 min)

---

### Weekend (March 30-31): Mock Interview

**Saturday (2 hours): Full Mock Interview**

**Set up:**
- Camera on (simulate video call)
- Timer visible
- Record yourself

**Interview simulation (90 min):**

**0-5 min: Intro**
- "Tell me about yourself"
- "Why NVIDIA?"
- "Why this role?"

**5-25 min: Technical Deep-Dive (Pick 4 questions)**
1. "Walk me through EIP architecture"
2. "How would you design Terraform modules for 50 teams?"
3. "Design RBAC for multi-tenant K8s cluster"
4. "How do you handle K8s cluster upgrades with zero downtime?"
5. "Multi-region architecture for 99.99% uptime"
6. "How does DNA Sequencing detect config drift?"

**25-35 min: System Design**
- Pick one from Friday's options
- Whiteboard it (use paper/tablet)

**35-45 min: Your Questions**
- Ask 3-4 smart questions

**Watch recording, note:**
- [ ] Rambling or concise?
- [ ] Answering the question or tangents?
- [ ] Confident or uncertain?
- [ ] Too many "um" or "like"?

---

**Sunday:**
- [ ] Review weak areas from mock
- [ ] Re-do 2-3 answers that were bad
- [ ] Rest of day: OFF

---

## Week 3 (April 1-7): Round 2 Prep Week

**Assumption: Round 2 scheduled this week**

### Monday - Focus Area Deep Dive

**Once you know Round 2 focus (IaC / K8s / AWS):**

**If IaC:**
- [ ] Review all Terraform notes from Week 1-2
- [ ] Prepare 5 stories from your experience
- [ ] Practice: Module design, state management, testing

**If Kubernetes:**
- [ ] Review all K8s notes
- [ ] Prepare: RBAC, upgrades, security, multi-tenancy
- [ ] Practice: Your Argo CD setup explanation

**If AWS Architecture:**
- [ ] Review: Multi-region, cost optimization, Well-Architected
- [ ] Prepare: Real architectures you've built
- [ ] Practice: Design questions

**If Developer Platform:**
- [ ] Review: API design, adoption strategies, product thinking
- [ ] Prepare: EIP + DNA Sequencing stories
- [ ] Practice: "How would you build X as a product?"

---

### Tuesday - War Stories Prep

**Write out 5 detailed stories (30 min each):**

**Story 1: Technical Challenge**
- Situation: What was the problem?
- Task: What did you need to solve?
- Action: What did you actually do? (Technical details)
- Result: What was the outcome? (Metrics)

**Story 2: Production Incident**
- The incident, your debugging process, the fix, the prevention

**Story 3: Scaling Challenge**
- System that didn't scale, how you made it scale, lessons learned

**Story 4: Security Issue**
- Vulnerability found, how you fixed it, how you prevented recurrence

**Story 5: Golden Path Adoption**
- How you got developers to use your platform, metrics

**Practice telling each in 3-4 minutes out loud**

---

### Wednesday - Likely Questions Prep

**Write crisp answers (2-3 min each):**

**Technical Questions:**
1. "Walk me through your Terraform module design"
2. "How do you test infrastructure code?"
3. "Explain your K8s security approach"
4. "How do you handle secrets in K8s?"
5. "Design a multi-region failover architecture"
6. "How would you reduce AWS costs by 20%?"
7. "What's your approach to K8s upgrades?"
8. "How do you detect and fix infrastructure drift?"

**Behavioral Questions:**
1. "Tell me about a time you disagreed with your team"
2. "Tell me about a failure and what you learned"
3. "How do you prioritize competing demands?"
4. "How do you stay current with new technologies?"

**Product Questions:**
1. "How would you convince developers to use your platform?"
2. "How do you measure platform success?"
3. "What's your approach to platform documentation?"
4. "How do you gather developer feedback?"

---

### Thursday - Your Tools Polish

**EIP:**
- [ ] Run locally, ensure no errors
- [ ] Test all 5 pillars
- [ ] Prepare 4-min architecture walkthrough
- [ ] Have production metrics ready
- [ ] Screenshots of API responses

**DNA Sequencing:**
- [ ] End-to-end demo works
- [ ] Prepare secret rotation example
- [ ] 3-min explanation of cross-layer detection
- [ ] Have example output ready

**Practice:**
- [ ] "Walk me through EIP" (4 min)
- [ ] "Walk me through DNA Sequencing" (3 min)
- [ ] "How do these complement each other?" (2 min)

---

### Friday - Questions for Them

**Prepare 10 questions, pick best 4 during interview:**

**About the Role:**
1. "What's the first project I'd work on?"
2. "What does success look like in first 6 months?"
3. "What's the biggest platform challenge right now?"

**About the Team:**
4. "How is Product Security team structured?"
5. "What's the mix of security engineers vs platform engineers?"
6. "What's the on-call rotation?"

**Technical:**
7. "What's the current tech stack - Go, Python, flexibility?"
8. "How many K8s clusters are you managing?"
9. "What's the secrets management solution?"
10. "How do you handle multi-cloud (AWS, GCP, Azure)?"

**Strategic:**
11. "How does NVIDIA think about AI security differently?"
12. "What's the build vs buy philosophy for tooling?"

---

### Weekend Before Round 2

**Saturday (Light Review):**
- [ ] Skim all notes (don't study new material)
- [ ] Practice your opening (30 sec pitch)
- [ ] Review your 5 war stories
- [ ] Early bed (8+ hours sleep)

**Sunday (Rest Day):**
- [ ] No studying at all
- [ ] Exercise, relax
- [ ] Normal sleep schedule
- [ ] Positive self-talk

---

## Week 4 (April 8-14): Remaining Rounds

### Each Round Prep Pattern

**3 Days Before:**
- [ ] Deep dive on that round's focus area
- [ ] Review relevant notes
- [ ] Practice 5-6 likely questions

**1 Day Before:**
- [ ] Light review only (skim notes)
- [ ] Practice opening + closing
- [ ] Normal workday, early bed

**Day Of:**
- [ ] No cramming (makes you nervous)
- [ ] 30-min review in morning
- [ ] Normal activities until interview
- [ ] If evening interview: 30-min nap at 6 PM

**After Each Round:**
- [ ] Write down every question asked
- [ ] Note what you did well/poorly
- [ ] Adjust prep for next round

---

## Daily Habits (Throughout All 4 Weeks)

**Every Morning (15 min):**
- [ ] Review yesterday's notes
- [ ] Read one technical article
- [ ] Practice one answer out loud

**Every Evening (45 min):**
- [ ] Study session (follow weekly plan above)
- [ ] Write down 3 things learned
- [ ] Preview tomorrow's topic

**Every Weekend:**
- [ ] Saturday: Mock interview or light review
- [ ] Sunday: Complete rest

**Energy Management:**
- [ ] 7-8 hours sleep every night
- [ ] Exercise 3-4x/week
- [ ] Healthy meals
- [ ] Avoid burnout (marathon, not sprint)

---

## Key Principles

### 1. Consistency > Intensity
- 90 min/day for 4 weeks = 42 hours total
- Better than 12-hour cramming sessions
- Spaced repetition works better

### 2. Active Learning > Passive Reading
- Write answers, don't just read
- Practice out loud, don't just think
- Build/demo things, don't just study

### 3. Focus on Gaps, Not Strengths
- You already know K8s basics
- Spend time on: Vault, multi-region, cost optimization
- Polish weak areas, not strong areas

### 4. Connect Everything to Your Work
- Every concept: "How does this relate to EIP?"
- Every question: "How did I solve this at ThoughtWorks?"
- Your experience is your advantage

---

## Red Flags to Avoid

**Don't:**
- ❌ Cram 8 hours the night before
- ❌ Learn new technologies from scratch (Go, Rust, etc)
- ❌ Build new projects (you have EIP + DNA Sequencing)
- ❌ Memorize answers word-for-word
- ❌ Neglect sleep/exercise/health
- ❌ Study on weekends (burnout risk)

**Do:**
- ✅ Sustainable daily practice
- ✅ Deepen existing knowledge
- ✅ Polish what you've already built
- ✅ Practice explaining clearly
- ✅ Maintain energy and health
- ✅ Rest on weekends

---

## The Week-by-Week Summary

**Week 1:** Foundation (Terraform, K8s, AWS, APIs, Your Tools)  
**Week 2:** Depth (Testing, Ops, Cost, Secrets, System Design)  
**Week 3:** Round 2 Prep (Focus area deep-dive, stories, questions)  
**Week 4:** Execute (Remaining rounds, adapt based on feedback)

**Total time investment:** 90 min/day × 20 days = 30 hours over 4 weeks

**Plus:** 2 hours mock interview, 2 hours Round 2 focused prep = **~35 hours total**

---

## Success Metrics

**You'll know you're ready when:**
- [ ] Can explain EIP + DNA Sequencing in 30 seconds each
- [ ] Can answer "Design X" questions with structured approach
- [ ] Have 5 war stories ready (3-4 min each)
- [ ] Can discuss Terraform/K8s/AWS with depth, not surface
- [ ] Know your gaps and how you'd address them
- [ ] Feel confident but not arrogant
- [ ] Sleep well the night before interviews

---

**Sud, this is your roadmap. 90 min/day, sustainable pace, deep preparation.**

**Stick to this plan and you'll walk into Round 2 completely ready.**

**Start tomorrow (Monday). Week 1, Day 1: Terraform Mastery.** 

**Let's go.** 🚀
