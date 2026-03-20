# AWS Well-Architected Framework - Detailed Study Checklist

## **How to Use This Checklist**
- [ ] Block 2-hour focused study sessions per pillar
- [ ] For each topic: Read → Visualize architecture → Ask "How would I implement this for NVIDIA's platform?"
- [ ] Mark ✅ only after you can explain it to someone else
- [ ] Create a 1-page cheat sheet per pillar with decision trees

---

## **PILLAR 1: Operational Excellence** (Days 1-2)

### **Core Principle**: Run and monitor systems to deliver business value, continually improve processes

### **Design Principles Checklist**
- [ ] Understand "Operations as Code" - IaC implications
- [ ] "Annotate documentation" - metadata tagging strategies
- [ ] "Make frequent, small, reversible changes" - blue/green, canary patterns
- [ ] "Refine operations procedures frequently" - runbook automation
- [ ] "Anticipate failure" - game days, chaos engineering
- [ ] "Learn from all operational failures" - blameless postmortems

---

### **Topic 1: Organization**

#### **1.1 Organizational Priorities**
- [ ] **Business value mapping**: How infrastructure decisions align to product velocity
- [ ] **Stakeholder identification**: Engineering, security, finance, compliance
- [ ] **Tradeoff evaluation**: Speed vs stability, cost vs performance

**Interview Question Practice:**
- *"Your team wants to deploy 10x/day but security requires approval gates. How do you balance?"*
- Your answer framework: _________________________________

#### **1.2 Operating Model**
- [ ] **Team structure models**: Centralized vs federated vs hybrid
- [ ] **Platform team responsibilities**: Golden paths, self-service APIs
- [ ] **Communication mechanisms**: Runbooks, dashboards, alerts

**Map to NVIDIA context:**
- [ ] How would EIP's "infrastructure-as-API" fit centralized vs federated model?
- [ ] What capabilities should platform team expose vs delegate?

---

### **Topic 2: Prepare**

#### **2.1 Design Telemetry**
- [ ] **Metrics hierarchy**: Business → Application → Infrastructure
- [ ] **CloudWatch custom metrics**: Publishing patterns, metric math
- [ ] **CloudWatch Logs Insights**: Query syntax, dashboard widgets
- [ ] **X-Ray tracing**: Service map, annotations, sampling rules
- [ ] **EventBridge for operational events**: Rule patterns, targets

**Hands-on checklist:**
- [ ] Create CloudWatch dashboard with 5 golden signals (latency, traffic, errors, saturation, cost)
- [ ] Write Logs Insights query to find P99 API latency
- [ ] Set up X-Ray for a sample Lambda→DynamoDB flow

#### **2.2 Design for Operations**
- [ ] **Metadata standards**: Tagging strategy (cost center, environment, owner, compliance)
- [ ] **AWS Config**: Resource inventory, compliance rules, remediation
- [ ] **Systems Manager**: Parameter Store vs Secrets Manager decision tree
- [ ] **Service Catalog**: Portfolio design, constraints, TagOptions

**Decision framework to memorize:**
```
Parameter Store vs Secrets Manager:
- Use Parameter Store: Config values, non-sensitive, free tier, simple rotation
- Use Secrets Manager: DB credentials, API keys, auto-rotation, cross-region replication
```

- [ ] Create this decision tree for 3 more service pairs (RDS vs DynamoDB, ALB vs API Gateway, ECS vs Lambda)

#### **2.3 Infrastructure as Code**
- [ ] **CloudFormation**: StackSets (multi-account), nested stacks, drift detection
- [ ] **CDK constructs**: L1 vs L2 vs L3 abstraction levels
- [ ] **Terraform on AWS**: State management (S3 + DynamoDB locking), workspaces
- [ ] **CI/CD for IaC**: CodePipeline, GitHub Actions, testing strategies

**Practice scenario:**
- [ ] Design multi-account VPC deployment using CloudFormation StackSets
- [ ] When would you choose CDK over raw CloudFormation? (Answer: ______)

#### **2.4 Deployment Strategies**
- [ ] **Blue/Green**: Route 53 weighted routing, ALB target groups
- [ ] **Canary**: API Gateway canary deployments, Lambda versions/aliases
- [ ] **Feature flags**: AppConfig, Parameter Store with versioning
- [ ] **Rollback mechanisms**: CloudFormation change sets, CodeDeploy auto-rollback

**Map to services:**
- [ ] ECS blue/green: How does it work with ALB target groups?
- [ ] Lambda canary: Alias traffic shifting percentages
- [ ] Database schema changes: How to deploy without downtime?

---

### **Topic 3: Operate**

#### **3.1 Runbook Automation**
- [ ] **Systems Manager Automation**: Documents, pre-built runbooks
- [ ] **EventBridge rules**: Triggering automated responses
- [ ] **Step Functions**: Orchestrating multi-step operations
- [ ] **Lambda for operational tasks**: Auto-remediation patterns

**Practice designing:**
- [ ] Automated response to "EC2 instance failed health check": SNS → Lambda → ASG replacement
- [ ] Automated EBS snapshot lifecycle: EventBridge schedule → Lambda → create snapshot → tag → cleanup old

#### **3.2 Understanding Workload Health**
- [ ] **CloudWatch Alarms**: Metric math, composite alarms, anomaly detection
- [ ] **Health checks**: Route 53, ALB, ECS, Lambda
- [ ] **Service Health Dashboard**: AWS Personal Health Dashboard, EventBridge integration
- [ ] **AWS Compute Optimizer**: Right-sizing recommendations

**Alarm strategy:**
- [ ] Design alarm hierarchy: Critical (page), Warning (ticket), Info (dashboard)
- [ ] Practice: "API error rate >1% for 5 mins" → What CloudWatch alarm config?

---

### **Topic 4: Evolve**

#### **4.1 Learn from Experience**
- [ ] **CloudWatch Insights**: Application Insights, Container Insights
- [ ] **DevOps-Research (DORA) metrics**: Deployment frequency, lead time, MTTR, change failure rate
- [ ] **Postmortem frameworks**: 5 Whys, timeline reconstruction
- [ ] **Game days**: Chaos engineering with AWS Fault Injection Simulator (FIS)

**Create template:**
- [ ] Design incident response runbook structure
- [ ] Define what metrics indicate "successful deployment" for your use case

---

### **Operational Excellence - Key AWS Services Summary**

| Category | Services | Your Understanding ✅ |
|----------|----------|---------------------|
| **IaC** | CloudFormation, CDK, Terraform | ☐ |
| **CI/CD** | CodePipeline, CodeBuild, CodeDeploy | ☐ |
| **Observability** | CloudWatch, X-Ray, EventBridge | ☐ |
| **Automation** | Systems Manager, Lambda, Step Functions | ☐ |
| **Configuration** | AppConfig, Parameter Store, Secrets Manager | ☐ |
| **Standards** | Config, Service Catalog, Control Tower | ☐ |

---

## **PILLAR 2: Security** (Days 2-3)

### **Core Principle**: Protect information, systems, and assets while delivering business value

### **Design Principles Checklist**
- [ ] "Implement a strong identity foundation" - least privilege
- [ ] "Enable traceability" - logging at all layers
- [ ] "Apply security at all layers" - defense in depth
- [ ] "Automate security best practices" - guardrails as code
- [ ] "Protect data in transit and at rest" - encryption everywhere
- [ ] "Keep people away from data" - minimize human access
- [ ] "Prepare for security events" - incident response automation

---

### **Topic 1: Security Foundations**

#### **1.1 AWS Account Structure**
- [ ] **AWS Organizations**: OU design (workload, environment, function-based)
- [ ] **Service Control Policies (SCPs)**: Deny vs allow lists, inheritance
- [ ] **Control Tower**: Account Factory, guardrails (detective vs preventive)
- [ ] **Multi-account strategy patterns**: 
  - [ ] Sandbox | Dev | Staging | Prod per workload
  - [ ] Shared services account (networking, logging, security tools)
  - [ ] Log archive account (centralized CloudTrail, Config, VPC Flow Logs)

**Practice designing:**
- [ ] 3-tier OU structure for 50 application teams
- [ ] SCP to prevent regions outside us-east-1, us-west-2, eu-west-1
- [ ] SCP to enforce IMDSv2 on all EC2 instances

**SCP example to understand:**
```json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Effect": "Deny",
    "Action": "ec2:RunInstances",
    "Resource": "arn:aws:ec2:*:*:instance/*",
    "Condition": {
      "StringNotEquals": {
        "ec2:MetadataHttpTokens": "required"
      }
    }
  }]
}
```
- [ ] Explain how this enforces IMDSv2 requirement

#### **1.2 Root Account Protection**
- [ ] **MFA enforcement**: Hardware token vs virtual
- [ ] **Root credential usage**: Only for specific tasks (changing support plan, closing account)
- [ ] **CloudTrail monitoring**: Alert on root account usage

---

### **Topic 2: Identity and Access Management**

#### **2.1 IAM Foundations**
- [ ] **Principal types**: Root, IAM users, roles, federated users
- [ ] **Policy types**: Identity-based, resource-based, permission boundaries, SCPs, session policies
- [ ] **Policy evaluation logic**: Explicit deny > explicit allow > implicit deny

**Master this flow:**
```
Request → Check SCPs → Check permission boundaries → Check identity policies → Check resource policies
```

- [ ] Practice: Given overlapping allow/deny, what's the effective permission?

#### **2.2 Least Privilege Implementation**
- [ ] **IAM Access Analyzer**: Finding overly permissive policies
- [ ] **Permission boundaries**: Delegating admin rights safely
- [ ] **IAM conditions**: IP restrictions, MFA requirements, time-based, tag-based
- [ ] **Session tags**: Attribute-based access control (ABAC)

**Condition examples to know:**
- [ ] `aws:SourceIp` - Restrict to corporate IP range
- [ ] `aws:MultiFactorAuthPresent` - Require MFA for sensitive actions
- [ ] `aws:PrincipalTag` - Allow only if user has tag Department=Engineering
- [ ] `aws:RequestedRegion` - Limit to specific regions

#### **2.3 Role Design Patterns**
- [ ] **Service roles**: EC2, Lambda, ECS task roles
- [ ] **Cross-account roles**: AssumeRole trust policies
- [ ] **Role chaining**: Max 1 hour sessions, use cases
- [ ] **Service-linked roles**: Auto-created, managed by AWS

**Practice scenario:**
- [ ] Design cross-account access: Dev account → assume role in Prod account to read S3 logs (but not modify)
- [ ] Create trust policy + permissions policy for this

#### **2.4 Federation & SSO**
- [ ] **AWS IAM Identity Center** (formerly SSO): SAML 2.0, permission sets
- [ ] **SAML federation**: IdP integration (Okta, Azure AD)
- [ ] **OIDC federation**: GitHub Actions, web identity federation
- [ ] **Temporary credentials**: STS AssumeRole, GetSessionToken

**Map to enterprise scenario:**
- [ ] Employees authenticate with Okta → map to AWS roles based on AD groups
- [ ] Design permission set strategy: By role (Developer, DataScientist) or by environment (ProdReadOnly)?

---

### **Topic 3: Detection**

#### **3.1 Logging & Monitoring**
- [ ] **CloudTrail**: Organization trail, data events, Insights
- [ ] **VPC Flow Logs**: Accept/reject traffic analysis
- [ ] **AWS Config**: Resource compliance, configuration history
- [ ] **GuardDuty**: Threat detection (compromised credentials, cryptocurrency mining, malicious IPs)
- [ ] **Security Hub**: Aggregated findings, CIS AWS Foundations Benchmark

**Log strategy to design:**
- [ ] Where to store logs? (S3 + lifecycle policy to Glacier)
- [ ] How long to retain? (Compliance requirements: 7 years for some industries)
- [ ] How to search? (CloudWatch Logs Insights, Athena on S3)

**GuardDuty finding types to know:**
- [ ] UnauthorizedAccess:EC2/SSHBruteForce
- [ ] Backdoor:EC2/C&CActivity
- [ ] CryptoCurrency:EC2/BitcoinTool
- [ ] Practice: What automated response would you design for each?

#### **3.2 Detective Controls**
- [ ] **Macie**: Discover sensitive data (PII, credentials in S3)
- [ ] **Inspector**: Vulnerability scanning (EC2, ECR, Lambda)
- [ ] **Detective**: Security investigation graph
- [ ] **Access Analyzer**: Unintended external access to resources

**Practice scenario:**
- [ ] S3 bucket accidentally public → How do you detect? (Config rule, Access Analyzer)
- [ ] Prevent it? (S3 Block Public Access, SCP)

---

### **Topic 4: Infrastructure Protection**

#### **4.1 Network Segmentation**
- [ ] **VPC design**: CIDR planning, subnet strategy (public/private/data tiers)
- [ ] **Security Groups**: Stateful, source-based rules
- [ ] **NACLs**: Stateless, subnet-level, rule ordering
- [ ] **PrivateLink**: Service endpoints, interface endpoints
- [ ] **Transit Gateway**: Hub-and-spoke, network segmentation

**Practice designing:**
- [ ] 3-tier app (web/app/data) VPC with public, private, isolated subnets
- [ ] Security group rules: Web tier accepts 443 from internet, app tier accepts 8080 only from web tier
- [ ] When to use NACLs vs security groups? (Answer: ______)

#### **4.2 Edge Protection**
- [ ] **CloudFront**: Signed URLs, signed cookies, geo-restrictions
- [ ] **AWS WAF**: Rule groups (SQL injection, XSS), rate limiting, managed rules
- [ ] **Shield Standard vs Advanced**: DDoS protection, cost protection
- [ ] **Route 53**: DNSSEC, health checks, private hosted zones

**WAF rule examples:**
- [ ] Block requests with SQL keywords in query string
- [ ] Rate limit: Max 2000 requests per 5 mins per IP
- [ ] Geo-block: Deny requests from non-allowed countries

#### **4.3 Compute Protection**
- [ ] **Systems Manager Session Manager**: No SSH keys, logged sessions
- [ ] **EC2 Instance Connect**: Temporary SSH key upload
- [ ] **Secrets rotation**: RDS, Redshift automatic rotation via Secrets Manager
- [ ] **IMDSv2**: Hop limit, session-oriented requests

**Practice:**
- [ ] Explain why IMDSv2 prevents SSRF attacks: ______
- [ ] Design secret rotation for custom application API key (Lambda rotator)

---

### **Topic 5: Data Protection**

#### **5.1 Data Classification**
- [ ] **Tagging strategy**: Sensitivity (Public, Internal, Confidential, Restricted)
- [ ] **Macie for discovery**: Custom data identifiers, automated classification jobs
- [ ] **S3 Object Ownership**: ACL disabled, bucket owner enforced

#### **5.2 Encryption at Rest**
- [ ] **S3 encryption**: SSE-S3, SSE-KMS, SSE-C, client-side
- [ ] **EBS encryption**: Default encryption, encrypted snapshots
- [ ] **RDS encryption**: TDE for Oracle/SQL Server, KMS for others
- [ ] **DynamoDB encryption**: KMS customer managed keys
- [ ] **KMS key policies**: Key administrators vs key users

**KMS decision tree:**
- [ ] AWS managed key vs customer managed key: When to use each?
- [ ] Single-region vs multi-region key: Use cases
- [ ] Key rotation: Automatic (yearly) vs manual

**Practice scenario:**
- [ ] Design KMS key policy: Dev team can encrypt/decrypt, security team can only view metadata

#### **5.3 Encryption in Transit**
- [ ] **TLS everywhere**: ALB HTTPS listeners, CloudFront SSL/TLS
- [ ] **Certificate management**: ACM, auto-renewal, multi-domain (SAN)
- [ ] **VPN**: Site-to-Site VPN, Client VPN
- [ ] **Direct Connect**: MACsec encryption

---

### **Topic 6: Incident Response**

#### **6.1 Preparation**
- [ ] **IR plan components**: Roles, communication plan, escalation paths
- [ ] **Forensics account**: Isolated account for investigation
- [ ] **Automated isolation**: Lambda to quarantine compromised instance (new SG, snapshot, tag)

#### **6.2 Detection & Analysis**
- [ ] **EventBridge rules**: GuardDuty findings → SNS → Lambda
- [ ] **Step Functions**: Orchestrate investigation workflow
- [ ] **CloudWatch Logs for correlation**: Combine CloudTrail + VPC Flow + application logs

**Practice automation:**
- [ ] GuardDuty finds compromised EC2 instance → Automated response:
  1. Snapshot EBS volumes
  2. Apply quarantine security group (deny all)
  3. Create forensic copy in isolated account
  4. Notify security team via SNS
- [ ] Design this flow with EventBridge + Lambda + Step Functions

---

### **Security - Key AWS Services Summary**

| Category | Services | Your Understanding ✅ |
|----------|----------|---------------------|
| **Identity** | IAM, IAM Identity Center, Organizations, STS | ☐ |
| **Detection** | CloudTrail, Config, GuardDuty, Security Hub, Macie, Detective | ☐ |
| **Network** | VPC, Security Groups, NACLs, PrivateLink, Transit Gateway | ☐ |
| **Edge** | WAF, Shield, CloudFront, Route 53 | ☐ |
| **Data Protection** | KMS, ACM, Secrets Manager, Macie | ☐ |
| **Incident Response** | EventBridge, Lambda, Step Functions, Systems Manager | ☐ |

---

## **PILLAR 3: Reliability** (Day 4)

### **Core Principle**: Recover from failures, meet demand, mitigate disruptions

### **Design Principles Checklist**
- [ ] "Automatically recover from failure"
- [ ] "Test recovery procedures" - game days
- [ ] "Scale horizontally" - distribute load
- [ ] "Stop guessing capacity" - auto-scaling
- [ ] "Manage change through automation" - IaC for consistency

---

### **Topic 1: Foundations**

#### **1.1 Service Quotas**
- [ ] **Service Quotas console**: Monitoring, request increases
- [ ] **Common quotas**: VPC per region (5), EC2 instances per type, API rate limits
- [ ] **Trusted Advisor**: Service limit checks

**Practice:**
- [ ] You need to deploy 100 VPCs in us-east-1. What's the limitation and process to increase?

#### **1.2 Network Topology**
- [ ] **VPC fundamentals**: CIDR sizing (/16 to /28), subnet sizing
- [ ] **Multi-VPC patterns**: Transit Gateway, VPC peering (limitations)
- [ ] **Hybrid connectivity**: Direct Connect (LAG, resiliency), VPN (redundant tunnels)
- [ ] **DNS**: Route 53 Resolver, private hosted zones, hybrid DNS

**Practice designing:**
- [ ] Multi-region VPC architecture with Transit Gateway in each region
- [ ] Highly available Direct Connect: 2 connections, 2 locations, 2 routers

---

### **Topic 2: Workload Architecture**

#### **2.1 Distributed System Design**
- [ ] **Microservices patterns**: Service discovery (ECS Service Discovery, Cloud Map)
- [ ] **API design**: RESTful principles, versioning, pagination
- [ ] **Loose coupling**: SQS, SNS, EventBridge for async communication
- [ ] **Service mesh**: App Mesh basics (virtual nodes, routers, routes)

**Failure isolation patterns:**
- [ ] **Bulkheads**: Separate connection pools, thread pools
- [ ] **Circuit breakers**: Fail fast, prevent cascade failures
- [ ] How does SQS DLQ implement this? ______

#### **2.2 High Availability Patterns**
- [ ] **Multi-AZ**: RDS Multi-AZ (sync replication, auto failover), EFS, ALB
- [ ] **Auto Scaling Groups**: Launch templates, target tracking, step scaling, scheduled scaling
- [ ] **Elastic Load Balancing**: ALB vs NLB vs GWLB decision matrix
  - [ ] ALB: HTTP/HTTPS, path-based routing, host-based routing, lambda targets
  - [ ] NLB: TCP/UDP, static IPs, extreme performance (millions requests/sec)
  - [ ] GWLB: Third-party virtual appliances (firewalls, IDS/IPS)

**Practice scenario:**
- [ ] Design HA architecture for web app: ALB in 3 AZs → EC2 ASG (min 3, desired 6) in 3 AZs → RDS Multi-AZ
- [ ] What happens if 1 AZ fails? (Answer: ______)

---

### **Topic 3: Change Management**

#### **3.1 Monitoring**
- [ ] **CloudWatch metrics resolution**: Standard (5 min), detailed (1 min), custom (high-resolution 1 sec)
- [ ] **CloudWatch Alarms**: Static threshold, anomaly detection, composite alarms
- [ ] **CloudWatch Dashboards**: Cross-account, cross-region

**Metrics to always monitor:**
- [ ] **Application**: Request count, latency (avg, P50, P90, P99), error rate
- [ ] **Infrastructure**: CPU, memory, disk I/O, network
- [ ] **Business**: Orders/min, revenue, active users

#### **3.2 Capacity Planning**
- [ ] **Demand forecasting**: Historical analysis, seasonal patterns
- [ ] **Auto Scaling**: Target tracking policies (CPU 70%, ALB requests/target)
- [ ] **Scaling cooldown**: Why needed, default values
- [ ] **Predictive scaling**: ML-based, for cyclic patterns

**Practice:**
- [ ] Design auto-scaling policy for daily traffic pattern: Ramp up 8am-10am, steady 10am-6pm, ramp down 6pm-8pm

---

### **Topic 4: Failure Management**

#### **4.1 Backup Strategy**
- [ ] **AWS Backup**: Centralized, cross-region copy, lifecycle policies
- [ ] **RDS snapshots**: Automated (retention 1-35 days), manual (unlimited), cross-region copy
- [ ] **EBS snapshots**: Incremental, Data Lifecycle Manager
- [ ] **S3 versioning + replication**: CRR (cross-region), SRR (same-region)

**Backup retention strategy:**
- [ ] Design tiered retention: Daily (7 days), Weekly (4 weeks), Monthly (12 months), Yearly (7 years)

#### **4.2 Disaster Recovery Strategies**

**Memorize this matrix:**

| Strategy | RTO | RPO | AWS Pattern | Cost |
|----------|-----|-----|-------------|------|
| **Backup & Restore** | Hours-Days | Hours | S3 snapshots, restore on-demand | $ |
| **Pilot Light** | 10s of minutes | Minutes | Core infra running (DB replicas), scale on failover | $$ |
| **Warm Standby** | Minutes | Seconds | Scaled-down replica running, scale up on failover | $$$ |
| **Multi-Site Active/Active** | Real-time | Real-time | Full capacity in multiple regions | $$$$ |

- [ ] For each strategy, list 3 AWS services you'd use
- [ ] Map your current project: What's acceptable RTO/RPO? Which strategy fits?

**Pilot Light example:**
- [ ] Primary: us-east-1 (full stack), Secondary: us-west-2 (only RDS read replica, AMIs, CloudFormation templates ready)
- [ ] Failure trigger: Route 53 health check fails → EventBridge → Lambda → Deploy CloudFormation stack in us-west-2 → Update Route 53

#### **4.3 Resilience Testing**
- [ ] **Chaos engineering**: Fault Injection Simulator (FIS)
- [ ] **FIS experiment templates**: Terminate EC2, throttle API calls, network partition
- [ ] **Game days**: Scheduled failure drills, runbook validation

---

### **Topic 5: Multi-Region Architectures**

#### **5.1 Decision Framework**

**Use this checklist for multi-region assessment:**

- [ ] **Latency requirements**: 
  - Global user base with <100ms latency needs? → Multi-region
  - All users in one geography? → Single region with multi-AZ
  
- [ ] **Availability SLA**:
  - 99.99% (52 min/year downtime)? → Multi-AZ sufficient
  - 99.999% (5 min/year downtime)? → Need multi-region
  
- [ ] **Compliance**:
  - GDPR data residency? → EU users data stays in eu-west-1
  - HIPAA, PCI-DSS? → May require region-specific controls
  
- [ ] **Data consistency**:
  - Can tolerate eventual consistency (seconds delay)? → DynamoDB Global Tables
  - Need strong consistency? → Stick to single region or use Aurora Global with app-level routing
  
- [ ] **Cost tolerance**:
  - Budget for 2x infrastructure?
  - Data transfer costs (cross-region: ~$0.02/GB)

#### **5.2 Multi-Region Data Strategies**

**Option 1: DynamoDB Global Tables**
- [ ] **How it works**: Multi-master, last-write-wins conflict resolution
- [ ] **Replication lag**: Typically <1 second
- [ ] **Use case**: Shopping carts, user sessions, gaming leaderboards
- [ ] **Cost**: Pay for replicated writes in each region

**Practice:**
- [ ] User in Tokyo writes to ap-northeast-1 table
- [ ] User in London reads from eu-west-2 table
- [ ] What's the expected lag? What if concurrent writes? (Answer: ______)

**Option 2: Aurora Global Database**
- [ ] **How it works**: 1 primary region (write), up to 5 secondary regions (read)
- [ ] **Replication lag**: <1 second
- [ ] **Failover**: Promote secondary to primary in <1 minute (RPO <1 sec, RTO <1 min)
- [ ] **Use case**: E-commerce (writes in primary, reads globally), SaaS platforms

**Practice:**
- [ ] Primary in us-east-1 fails → How to promote eu-west-1? (Answer: ______)
- [ ] After failover, old primary comes back online → What happens?

**Option 3: S3 Cross-Region Replication**
- [ ] **Requirements**: Versioning enabled on both buckets
- [ ] **Replication Time Control (RTC)**: 99.99% within 15 minutes SLA
- [ ] **Filters**: Prefix, tags
- [ ] **Ownership**: Replica owner vs source owner

**Option 4: Application-Level Sharding**
- [ ] **Pattern**: US users → us-east-1, EU users → eu-west-1, APAC → ap-southeast-1
- [ ] **Route 53**: Geolocation routing policy
- [ ] **Data isolation**: Strong consistency per region, no cross-region sync needed
- [ ] **Use case**: SaaS platforms with regional data sovereignty

#### **5.3 Multi-Region Traffic Routing**

**Route 53 Routing Policies to master:**

- [ ] **Failover**: Primary/secondary, health check based
  - **Use case**: Active-passive DR
  - **Example**: Primary us-east-1, failover to us-west-2 if unhealthy
  
- [ ] **Geolocation**: Route based on user's location
  - **Use case**: Compliance (EU users must hit EU region)
  - **Example**: EU → eu-west-1, US → us-east-1, default → us-east-1
  
- [ ] **Geoproximity**: Route based on proximity with bias
  - **Use case**: Shift traffic gradually between regions (testing)
  - **Bias**: -99 to +99 (expand or shrink geographic coverage)
  
- [ ] **Latency-based**: Route to lowest latency endpoint
  - **Use case**: Optimize user experience globally
  - **Example**: User in Mumbai automatically routed to ap-south-1
  
- [ ] **Weighted**: Percentage-based traffic split
  - **Use case**: Blue/green deployments, A/B testing
  - **Example**: 90% to v1, 10% to v2

**Practice designing:**
- [ ] Global app with 3 regions (US, EU, APAC)
- [ ] EU users MUST stay in EU (compliance)
- [ ] US/APAC users routed by latency
- [ ] Which routing policies do you combine? (Answer: ______)

#### **5.4 Multi-Region Service Patterns**

**Compute:**
- [ ] **ECS/EKS**: Separate clusters per region, shared ECR (replicate images)
- [ ] **Lambda**: Deploy same function to multiple regions, regional API Gateway

**Secrets:**
- [ ] **Secrets Manager**: Multi-region secret replication feature
- [ ] **Parameter Store**: Manually replicate to secondary regions

**Monitoring:**
- [ ] **CloudWatch cross-region dashboards**: Single pane of glass
- [ ] **EventBridge cross-region event bus**: Aggregate events centrally

---

### **Reliability - Key AWS Services Summary**

| Category | Services | Your Understanding ✅ |
|----------|----------|---------------------|
| **Foundations** | Service Quotas, Trusted Advisor | ☐ |
| **HA Compute** | ASG, ELB (ALB, NLB), EC2 Multi-AZ | ☐ |
| **HA Data** | RDS Multi-AZ, Aurora, DynamoDB, EFS | ☐ |
| **Multi-Region** | Route 53, DynamoDB Global Tables, Aurora Global, S3 CRR | ☐ |
| **Monitoring** | CloudWatch, X-Ray, EventBridge | ☐ |
| **Backup/DR** | AWS Backup, S3 versioning, Snapshot lifecycle | ☐ |

---

## **PILLAR 4: Performance Efficiency** (Day 5)

### **Core Principle**: Use resources efficiently, adapt to demand, experiment rapidly

### **Topic 1: Selection**

#### **1.1 Compute Selection**

**EC2 Instance Types Decision Matrix:**

- [ ] **General Purpose (T, M)**: Balanced CPU/memory
  - T: Burstable (web servers, dev environments)
  - M: Steady-state workloads
  
- [ ] **Compute Optimized (C)**: High CPU (HPC, batch, gaming servers)
  
- [ ] **Memory Optimized (R, X, z1d)**: High memory (databases, caching, analytics)
  - R: General high-memory
  - X: Largest memory (up to 4TB)
  - z1d: High compute + high memory + NVMe
  
- [ ] **Storage Optimized (I, D, H)**: High I/O
  - I: NVMe SSD (NoSQL databases)
  - D: HDD (MapReduce, distributed file systems)
  - H: Balance disk/compute (Hadoop, HDFS)
  
- [ ] **Accelerated (P, G, Inf, Trn)**: GPU/ML
  - P: General GPU (ML training, HPC)
  - G: Graphics workloads
  - Inf: Inference
  - Trn: ML training

**Practice scenario:**
- [ ] Web app with variable traffic: ______ instance type
- [ ] In-memory Redis cache: ______ instance type
- [ ] Machine learning inference API: ______ instance type

**Container Orchestration:**
- [ ] **ECS vs EKS**: When to use each?
  - ECS: AWS-native, simpler, integrated with AWS services
  - EKS: Kubernetes, portability, complex orchestration
  
- [ ] **Fargate vs EC2 launch type**:
  - Fargate: Serverless, no instance management, pay per task
  - EC2: More control, cheaper at scale, GPU support

**Serverless:**
- [ ] **Lambda**: Event-driven, <15 min execution, cold start considerations
- [ ] **Lambda memory**: 128MB-10GB (CPU scales proportionally)
- [ ] **Lambda concurrency**: Reserved vs unreserved, burst limits

**Decision tree to create:**
```
Event-driven + <15 min → Lambda
Long-running + containerized + simple → ECS Fargate
Long-running + Kubernetes → EKS
GPU workload → EC2 P instances or ECS EC2 with GPU
```

#### **1.2 Storage Selection**

**Block Storage:**

- [ ] **EBS Volume Types**:
  
  | Type | IOPS | Throughput | Use Case | Cost |
  |------|------|------------|----------|------|
  | gp3 | 3K-16K | 125-1000 MB/s | General purpose, boot volumes | $$ |
  | io2 | 100-64K | 1000 MB/s | Databases, critical apps | $$$$ |
  | st1 | 500 | 500 MB/s | Big data, log processing | $ |
  | sc1 | 250 | 250 MB/s | Cold data, infrequent access | $ |
  
- [ ] Practice: MySQL database with 10K IOPS requirement → Which EBS type?

**File Storage:**

- [ ] **EFS**: NFS, multi-AZ, auto-scaling, Linux only
  - Performance modes: General Purpose vs Max I/O
  - Throughput modes: Bursting vs Provisioned vs Elastic
  
- [ ] **FSx for Windows**: SMB, AD integration
- [ ] **FSx for Lustre**: HPC, ML training, S3 integration

**Object Storage:**

- [ ] **S3 Storage Classes**:
  
  | Class | Availability | Min Duration | Use Case | Cost |
  |-------|--------------|--------------|----------|------|
  | Standard | 99.99% | None | Hot data | $$$ |
  | Intelligent-Tiering | 99.9% | None | Unknown access | Auto |
  | Standard-IA | 99.9% | 30 days | Infrequent access | $$ |
  | One Zone-IA | 99.5% | 30 days | Reproducible, infrequent | $ |
  | Glacier Instant | 99.9% | 90 days | Archive, instant retrieval | $ |
  | Glacier Flexible | 99.99% | 90 days | Archive, mins-hours retrieval | $ |
  | Glacier Deep Archive | 99.99% | 180 days | Long-term archive, 12hr retrieval | $ |

- [ ] **Lifecycle policies**: Transition rules, expiration rules

**Practice scenario:**
- [ ] User uploads: Standard → 30 days → Standard-IA → 90 days → Glacier
- [ ] Design lifecycle policy for this

#### **1.3 Database Selection**

**Relational:**
- [ ] **RDS vs Aurora**: When Aurora justifies cost (5x writes, 15 read replicas, backtrack)
- [ ] **Aurora Serverless v2**: Auto-scaling ACUs (Aurora Capacity Units), vs provisioned

**NoSQL:**
- [ ] **DynamoDB**: Key-value, single-digit ms latency
  - Capacity modes: On-demand vs provisioned
  - DynamoDB Streams, DAX (caching), Global Tables
  
- [ ] **DocumentDB**: MongoDB-compatible, managed
- [ ] **Keyspaces**: Cassandra-compatible

**In-Memory:**
- [ ] **ElastiCache Redis vs Memcached**:
  - Redis: Persistence, replication, Pub/Sub, complex data types, snapshots
  - Memcached: Simple, multi-threaded, no persistence

**Analytics:**
- [ ] **Redshift**: Petabyte-scale DW, columnar, OLAP
- [ ] **Athena**: Serverless SQL on S3, pay-per-query

**Graph:**
- [ ] **Neptune**: Property graph (Gremlin), RDF (SPARQL)

**Time Series:**
- [ ] **Timestream**: Serverless, trillion events/day

**Decision framework:**
- [ ] Transactional + relational + <64TB → RDS
- [ ] Transactional + relational + >64TB or high read scaling → Aurora
- [ ] Key-value + millisecond latency + massive scale → DynamoDB
- [ ] Complex queries on historical data → Redshift
- [ ] Ad-hoc queries on S3 → Athena

---

### **Topic 2: Review**

#### **2.1 Performance Monitoring**
- [ ] **CloudWatch metrics**: Custom metrics for application-level insights
- [ ] **CloudWatch Logs Insights**: Query language, stats, aggregations
- [ ] **X-Ray**: Service map, trace analysis, annotations, metadata
- [ ] **Compute Optimizer**: EC2, EBS, Lambda recommendations

**Practice:**
- [ ] Write Logs Insights query: "Show me P99 latency for /api/checkout endpoint by hour for last 7 days"

#### **2.2 Load Testing**
- [ ] **Tools**: Distributed Load Testing solution (AWS-provided), Locust, k6
- [ ] **Patterns**: Ramp-up, spike, soak test
- [ ] **Metrics to capture**: Throughput, response time (P50, P95, P99), error rate

---

### **Topic 3: Monitoring**

#### **3.1 Caching Strategy**

**Caching Layers:**

1. **CloudFront (Edge)**
   - [ ] TTL configuration, invalidation
   - [ ] Cache behaviors (path-based)
   - [ ] Signed URLs/cookies for private content
   
2. **API Gateway**
   - [ ] Method caching, cache key parameters
   - [ ] Cache invalidation via header
   
3. **Application (ElastiCache)**
   - [ ] Cache-aside pattern, write-through
   - [ ] Redis cluster mode vs non-cluster
   
4. **Database (DAX for DynamoDB, Aurora read replicas)**
   - [ ] DAX: Microsecond latency, write-through
   - [ ] RDS read replicas: Offload reads

**Practice designing:**
- [ ] Content site: CloudFront → API Gateway cache → Lambda → DynamoDB + DAX
- [ ] Which content cached where? TTLs for each layer?

---

### **Performance - Key AWS Services Summary**

| Category | Services | Your Understanding ✅ |
|----------|----------|---------------------|
| **Compute** | EC2, Lambda, ECS, EKS, Fargate | ☐ |
| **Storage** | EBS, EFS, FSx, S3 storage classes | ☐ |
| **Database** | RDS, Aurora, DynamoDB, ElastiCache, Redshift | ☐ |
| **Network** | CloudFront, API Gateway, Global Accelerator | ☐ |
| **Monitoring** | CloudWatch, X-Ray, Compute Optimizer | ☐ |

---

## **PILLAR 5: Cost Optimization** (Day 6)

### **Core Principle**: Deliver business value at lowest price point

### **Topic 1: Expenditure Awareness**

#### **1.1 Cost Allocation**
- [ ] **Tagging strategy**: Cost center, project, environment, owner (enforce with SCPs)
- [ ] **Cost allocation tags**: Activate in billing console
- [ ] **Cost categories**: Organize spending (e.g., R&D, Sales, Operations)

#### **1.2 Cost Monitoring**
- [ ] **AWS Cost Explorer**: Forecasting, anomaly detection, RI recommendations
- [ ] **AWS Budgets**: Cost, usage, RI utilization alerts
- [ ] **Cost and Usage Report (CUR)**: Granular data to S3, Athena queries

**Practice:**
- [ ] Set up budget: Alert if EC2 spend exceeds $5K/month
- [ ] Create CUR query: "Show me total data transfer cost by service for last quarter"

---

### **Topic 2: Cost-Effective Resources**

#### **2.1 Pricing Models**

**EC2 Pricing:**
- [ ] **On-Demand**: No commitment, highest price
- [ ] **Savings Plans**: 1 or 3 year, ~66% discount, flexible (EC2, Lambda, Fargate)
- [ ] **Reserved Instances**: 1 or 3 year, ~72% discount, committed instance type/region
- [ ] **Spot**: Up to 90% discount, can be terminated with 2-min notice

**Savings Plans vs Reserved Instances:**
- [ ] Savings Plans: Flexibility (change instance type/region), applies to Lambda/Fargate
- [ ] RIs: Slightly cheaper, can sell on marketplace, more rigid

**Practice scenario:**
- [ ] You run 50 m5.large instances 24/7 in us-east-1
- [ ] Calculate: On-Demand cost, 1yr Compute Savings Plan discount, 3yr RI discount

**Spot Best Practices:**
- [ ] **Spot Fleet**: Mix instance types to reduce interruption
- [ ] **Interruption handling**: 2-min warning, CloudWatch Events, checkpoint work
- [ ] **Use cases**: Batch, CI/CD, stateless web, big data
- [ ] **Not suitable**: Databases, stateful apps without checkpointing

#### **2.2 Right-Sizing**
- [ ] **Compute Optimizer**: EC2, ASG, Lambda, EBS recommendations
- [ ] **Trusted Advisor**: Underutilized resources
- [ ] **CloudWatch metrics**: CPU <10% for 14 days → downsize candidate

**Practice:**
- [ ] Instance running at 5% CPU average → Which tool to check recommendations?
- [ ] Lambda with 3GB memory but only using 500MB → How to optimize?

---

### **Topic 3: Matching Supply with Demand**

#### **3.1 Auto Scaling**
- [ ] **EC2 Auto Scaling**: Target tracking, step scaling, scheduled
- [ ] **DynamoDB auto scaling**: Target utilization for read/write capacity
- [ ] **Aurora Serverless v2**: Auto-scale ACUs based on load
- [ ] **Lambda**: Automatic scaling (but watch concurrency limits)

#### **3.2 Serverless-First**
- [ ] **Lambda**: Pay per request (no idle cost)
- [ ] **API Gateway**: Pay per API call
- [ ] **S3**: Pay for storage + requests (no compute when idle)
- [ ] **Athena**: Pay per query (no DW running 24/7)

**Cost comparison:**
- [ ] Calculate: ALB ($18/month) + EC2 t3.small ($15/month) vs API Gateway + Lambda
- [ ] Break-even point: ______ requests/month

---

### **Topic 4: Optimizing Over Time**

#### **4.1 Regular Review**
- [ ] **Weekly**: Anomaly detection in Cost Explorer
- [ ] **Monthly**: Budget variance, rightsizing opportunities
- [ ] **Quarterly**: RI/Savings Plan utilization, commitment adjustments
- [ ] **Annually**: Architecture review, serverless migration candidates

#### **4.2 S3 Optimization**
- [ ] **Lifecycle policies**: Automate transitions
- [ ] **S3 Intelligent-Tiering**: Automatic cost optimization
- [ ] **S3 Storage Lens**: Organization-wide visibility, storage metrics
- [ ] **Delete incomplete multipart uploads**: Lifecycle rule after 7 days

---

### **Cost Optimization - Key AWS Services Summary**

| Category | Services | Your Understanding ✅ |
|----------|----------|---------------------|
| **Monitoring** | Cost Explorer, Budgets, CUR | ☐ |
| **Rightsizing** | Compute Optimizer, Trusted Advisor | ☐ |
| **Pricing** | Savings Plans, RIs, Spot | ☐ |
| **Optimization** | S3 Lifecycle, Intelligent-Tiering, Auto Scaling | ☐ |

---

## **PILLAR 6: Sustainability** (Day 6)

### **Core Principle**: Minimize environmental impact

### **Design Principles**
- [ ] "Understand your impact" - measure carbon footprint
- [ ] "Establish sustainability goals" - efficiency metrics
- [ ] "Maximize utilization" - right-size, consolidate
- [ ] "Adopt more efficient hardware/software" - Graviton, newer gen
- [ ] "Use managed services" - shared infrastructure efficiency
- [ ] "Reduce downstream impact" - data transfer, caching

### **Key Practices**

#### **Region Selection**
- [ ] **AWS Customer Carbon Footprint Tool**: Track emissions
- [ ] **Regions by carbon intensity**: eu-north-1 (Stockholm - renewable), us-west-2 (Oregon - hydro)
- [ ] **Latency vs carbon**: Trade-off analysis

#### **Efficient Architectures**
- [ ] **Graviton processors**: ARM-based, 60% less energy than x86
- [ ] **Serverless**: No idle resources
- [ ] **Auto scaling**: Match supply to demand exactly
- [ ] **Data lifecycle**: Delete unnecessary data

#### **Reduce Data Movement**
- [ ] **Edge caching**: CloudFront reduces origin load
- [ ] **Data compression**: gzip, Brotli
- [ ] **Efficient protocols**: HTTP/3, gRPC

**Practice:**
- [ ] Application running on c5.xlarge (x86) in us-east-1
- [ ] Estimate savings: Migrate to c7g.xlarge (Graviton) in eu-north-1
- [ ] Consider: Cost, carbon, latency tradeoff

---

## **Migration Strategies (7 Rs)** (Day 7)

### **The 7 Rs Framework**

#### **1. Retire**
- [ ] **Definition**: Decommission application (no longer needed)
- [ ] **Discovery**: Portfolio analysis, usage metrics
- [ ] **Savings**: Eliminate licensing, infrastructure, maintenance
- [ ] **Example**: Legacy reporting tool replaced by modern BI

#### **2. Retain**
- [ ] **Definition**: Keep on-premises (not ready for cloud)
- [ ] **Reasons**: Compliance, high refactoring cost, recently upgraded
- [ ] **Example**: Mainframe system with low cloud ROI

#### **3. Rehost (Lift-and-Shift)**
- [ ] **Definition**: Move to cloud without changes
- [ ] **Tools**: AWS Application Migration Service (MGN), VM Import
- [ ] **Benefits**: Quick migration, immediate cloud benefits (elasticity, HA)
- [ ] **Drawbacks**: Not cloud-optimized, technical debt carried over
- [ ] **Example**: Physical servers → EC2

#### **4. Relocate**
- [ ] **Definition**: Hypervisor-level move (VMware to AWS)
- [ ] **Tool**: VMware Cloud on AWS
- [ ] **Use case**: Large VMware footprint, need rapid migration

#### **5. Replatform (Lift-Tinker-Shift)**
- [ ] **Definition**: Minimal cloud optimization during migration
- [ ] **Examples**:
  - Self-managed MySQL on EC2 → RDS for MySQL
  - Self-managed Redis → ElastiCache
  - Web server binaries → Elastic Beanstalk
- [ ] **Benefits**: Managed services reduce ops burden
- [ ] **Effort**: Low to medium

**Practice scenario:**
- [ ] E-commerce app: Web tier (IIS), App tier (.NET), DB (SQL Server on VM)
- [ ] Replatform approach: ______

#### **6. Repurchase**
- [ ] **Definition**: Move to SaaS (drop and shop)
- [ ] **Examples**:
  - Exchange → Microsoft 365
  - Self-hosted CRM → Salesforce
  - Custom HR app → Workday
- [ ] **Benefits**: Zero infrastructure management
- [ ] **Considerations**: Data migration, integration, vendor lock-in

#### **7. Refactor (Re-architect)**
- [ ] **Definition**: Redesign for cloud-native
- [ ] **Patterns**:
  - Monolith → microservices
  - VMs → containers → serverless
  - Relational → NoSQL (where appropriate)
- [ ] **Benefits**: Maximum cloud value (scalability, cost optimization, agility)
- [ ] **Effort**: High
- [ ] **Example**: Monolithic Java app → ECS containers + DynamoDB + API Gateway

---

### **Migration Assessment Framework**

**For each application, evaluate:**

- [ ] **Technical complexity**: Codebase, dependencies, data volume
- [ ] **Business criticality**: Downtime tolerance, user impact
- [ ] **Compliance requirements**: Data residency, audit trails
- [ ] **Cost**: Migration effort + run rate
- [ ] **Timeline**: Business deadline constraints

**Decision matrix to create:**

| Factor | Rehost | Replatform | Refactor |
|--------|--------|------------|----------|
| Speed | ✅ Fast | Medium | ❌ Slow |
| Cost (short-term) | $ | $$ | $$$$ |
| Cloud optimization | ❌ Low | Medium | ✅ High |
| Risk | Low | Medium | High |
| Best for | Simple apps, time-sensitive | Database-heavy, standard stacks | Strategic apps, innovation |

---

### **AWS Migration Tools**

- [ ] **Migration Hub**: Central tracking for migrations
- [ ] **Application Discovery Service**: Inventory on-prem servers, dependency mapping
- [ ] **Application Migration Service (MGN)**: Rehost tool (continuous replication)
- [ ] **Database Migration Service (DMS)**: Homogeneous + heterogeneous DB migrations
  - [ ] **Schema Conversion Tool (SCT)**: Convert schema (Oracle → Aurora)
  - [ ] **Change Data Capture (CDC)**: Minimal downtime migration
- [ ] **DataSync**: Large-scale data transfer (on-prem → S3/EFS)
- [ ] **Snow Family**: Physical data transfer (Snowcone, Snowball, Snowmobile)
- [ ] **Transfer Family**: SFTP/FTPS/FTP → S3

**Practice:**
- [ ] Migrate 500TB database from Oracle on-prem to Aurora PostgreSQL
- [ ] Steps: ______
- [ ] Tools: ______

---

## **Final Exam Prep Scenarios**

### **Scenario 1: Multi-Region E-Commerce Platform**

**Requirements:**
- Global user base (US, EU, APAC)
- 99.99% availability SLA
- GDPR compliance (EU data in EU)
- <100ms API latency target
- Black Friday traffic: 10x normal

**Design this architecture covering all 6 pillars. Include:**
- [ ] Compute layer (with auto-scaling)
- [ ] Data layer (with multi-region strategy)
- [ ] Network/traffic routing
- [ ] Security (IAM, network, data protection)
- [ ] Monitoring and incident response
- [ ] Cost optimization (RI/Savings Plan strategy)
- [ ] DR strategy (RTO/RPO)

---

### **Scenario 2: Legacy Migration**

**Current state:**
- Monolithic .NET app on Windows Server
- SQL Server 2016 database (2TB, 10K IOPS)
- 50 concurrent users, 8am-6pm usage pattern
- 99.9% uptime currently

**Design migration approach:**
- [ ] Which of the 7 Rs? Why?
- [ ] Step-by-step migration plan
- [ ] AWS services for each tier
- [ ] Rollback strategy
- [ ] Testing approach

---

### **Scenario 3: Security Incident**

**Situation:** GuardDuty alerts on compromised IAM credentials

**Design automated response:**
- [ ] Detection (which services?)
- [ ] Automated containment (quarantine, disable credentials)
- [ ] Forensics collection
- [ ] Notification workflow
- [ ] Prevention (how to avoid recurrence?)

---

## **Your Weekly Study Schedule**

| Day | Focus | Hours | Deliverable |
|-----|-------|-------|-------------|
| **Day 1** | Operational Excellence + Security (Foundations) | 3-4 | 1-page cheat sheet per pillar |
| **Day 2** | Security (IAM, Detection, Data Protection) | 3-4 | IAM policy decision tree |
| **Day 3** | Reliability (HA, DR, Multi-Region) | 3-4 | Multi-region architecture diagram |
| **Day 4** | Performance + Cost Optimization | 3-4 | Service selection decision matrices |
| **Day 5** | Sustainability + Migration Strategies | 2-3 | 7Rs decision framework |
| **Day 6** | Hands-on: Build multi-region demo | 3-4 | Working demo you can discuss |
| **Day 7** | Practice scenarios + whiteboarding | 2-3 | Walk through 3 scenarios |

---

## **Success Metrics**

You're ready when you can:
- [ ] Whiteboard a multi-region architecture in 15 minutes
- [ ] Explain the tradeoffs between any two similar services (RDS vs DynamoDB, ALB vs NLB)
- [ ] Design an IAM policy for a complex scenario
- [ ] Choose the right migration strategy for an application in 5 minutes
- [ ] Calculate RTO/RPO for a DR strategy
- [ ] Explain how you'd implement all 6 pillars in a single architecture

---

## **Quick Reference: Multi-Region Decision Parameters**

### **When to Choose Multi-Region:**

1. **Latency Requirements**
   - [ ] Global user base requiring <100ms response times
   - [ ] Users distributed across continents
   - [ ] CDN alone can't solve latency issues

2. **Availability SLA**
   - [ ] Need 99.99%+ availability (>52 min downtime/year unacceptable)
   - [ ] Single region outage would violate SLA
   - [ ] Revenue loss from downtime exceeds multi-region cost

3. **Compliance & Data Residency**
   - [ ] GDPR: EU user data must stay in EU
   - [ ] China: Data localization laws
   - [ ] Industry-specific regulations (HIPAA, PCI-DSS regional requirements)

4. **Consistency Requirements**
   - [ ] Can tolerate eventual consistency (seconds)? → DynamoDB Global Tables
   - [ ] Need strong consistency? → Single-region or Aurora Global with read-only secondaries

5. **Cost Tolerance**
   - [ ] Budget for ~2x infrastructure costs
   - [ ] Data transfer costs: $0.02/GB cross-region
   - [ ] Can justify with revenue protection or SLA requirements

### **Multi-Region Implementation Patterns:**

**Pattern A: Active-Passive (Warm Standby)**
- [ ] Primary region serves 100% traffic
- [ ] Secondary region: scaled-down infrastructure, replicated data
- [ ] Route 53 health check triggers failover
- [ ] **RTO**: 5-15 minutes | **Cost**: $$ | **Use**: Cost-conscious DR

**Pattern B: Active-Active (Read Local, Write Global)**
- [ ] All regions serve read traffic (Route 53 latency/geo routing)
- [ ] Writes go to primary region, replicate to secondaries
- [ ] Aurora Global Database or DynamoDB Global Tables
- [ ] **RTO**: <1 minute | **Cost**: $$$ | **Use**: Global apps, high availability

**Pattern C: Active-Active (Multi-Master)**
- [ ] All regions accept reads AND writes
- [ ] DynamoDB Global Tables (last-write-wins conflict resolution)
- [ ] **RTO**: Real-time | **Cost**: $$$$ | **Use**: Global collaboration apps

**Pattern D: Geo-Sharding**
- [ ] Each region owns a data shard (US users → us-east-1, EU → eu-west-1)
- [ ] Route 53 geolocation routing enforces data residency
- [ ] No cross-region data sync needed
- [ ] **Consistency**: Strong per region | **Cost**: $$ | **Use**: GDPR compliance

---

## **Service Selection Decision Trees**

### **Compute Selection:**
```
├─ Event-driven workload?
│  ├─ Yes → Execution time?
│  │  ├─ <15 min → Lambda
│  │  └─ >15 min → Step Functions + Lambda or Fargate
│  └─ No → Long-running service?
│     ├─ Yes → Container-based?
│     │  ├─ Yes → Need Kubernetes?
│     │  │  ├─ Yes → EKS
│     │  │  └─ No → ECS (Fargate if simple, EC2 if GPU/control needed)
│     │  └─ No → EC2 (with Auto Scaling)
│     └─ No → Batch processing?
│        └─ Yes → AWS Batch or Spot Instances
```

### **Database Selection:**
```
├─ Data model?
│  ├─ Relational (SQL)
│  │  ├─ Size <64TB → RDS
│  │  └─ Size >64TB OR need 15 read replicas → Aurora
│  ├─ Key-Value (NoSQL)
│  │  ├─ Need multi-region writes → DynamoDB Global Tables
│  │  └─ Single region → DynamoDB
│  ├─ Document
│  │  └─ MongoDB compatible → DocumentDB
│  ├─ In-Memory Cache
│  │  ├─ Need persistence/replication → ElastiCache Redis
│  │  └─ Simple cache only → ElastiCache Memcached
│  ├─ Analytics/Data Warehouse
│  │  ├─ Structured data, regular queries → Redshift
│  │  └─ Ad-hoc queries on S3 → Athena
│  └─ Graph
│     └─ Neptune
```

### **Storage Selection:**
```
├─ Access pattern?
│  ├─ File system (NFS/SMB)
│  │  ├─ Linux → EFS
│  │  ├─ Windows → FSx for Windows File Server
│  │  └─ HPC/ML → FSx for Lustre
│  ├─ Block storage (attached to EC2)
│  │  ├─ Need >64K IOPS → io2
│  │  ├─ General purpose → gp3
│  │  └─ Throughput-optimized → st1
│  └─ Object storage
│     ├─ Hot data (frequent access) → S3 Standard
│     ├─ Unknown access pattern → S3 Intelligent-Tiering
│     ├─ Infrequent access → S3 Standard-IA
│     └─ Archive → Glacier (Instant/Flexible/Deep Archive)
```

---

## **Common Interview Question Patterns**

### **Pattern 1: "Design a multi-region architecture for..."**
**Your response framework:**
1. Clarify requirements (latency SLA, availability SLA, compliance)
2. Choose traffic routing strategy (Route 53 policy)
3. Choose data replication strategy (Aurora Global vs DynamoDB Global vs sharding)
4. Explain trade-offs (cost vs consistency vs complexity)
5. Address monitoring and failover

### **Pattern 2: "How would you migrate..."**
**Your response framework:**
1. Assess current state (tech stack, data volume, dependencies)
2. Choose from 7 Rs (explain why)
3. Design migration phases (discovery → pilot → waves)
4. Plan for rollback and testing
5. Identify risks and mitigations

### **Pattern 3: "Secure this architecture..."**
**Your response framework:**
1. Identity & Access (IAM roles, least privilege, MFA)
2. Network (VPC design, security groups, NACLs)
3. Data protection (encryption at rest/transit, KMS)
4. Detection (CloudTrail, GuardDuty, Config)
5. Incident response (automated remediation)

### **Pattern 4: "Optimize costs for..."**
**Your response framework:**
1. Right-size resources (Compute Optimizer)
2. Choose pricing models (Savings Plans, Spot)
3. Implement auto-scaling
4. Storage lifecycle policies
5. Monitor and review (Cost Explorer, Budgets)

---

## **Final Preparation Checklist**

**Week Before Interview:**
- [ ] Review all 6 pillar cheat sheets
- [ ] Whiteboard 3 different architectures from memory
- [ ] Explain multi-region patterns without notes
- [ ] Practice service selection decision trees out loud
- [ ] Review your own project through WAF lens

**Day Before Interview:**
- [ ] Sleep well (seriously)
- [ ] Review multi-region decision parameters
- [ ] Quick review of IAM policy evaluation logic
- [ ] Review RTO/RPO calculations
- [ ] Prepare questions about NVIDIA's infrastructure

**During Interview:**
- [ ] Listen carefully to requirements before jumping to solutions
- [ ] Ask clarifying questions (latency requirements? compliance? budget?)
- [ ] Think out loud - show your reasoning process
- [ ] Acknowledge trade-offs - there's rarely one "right" answer
- [ ] Map solutions back to WAF pillars when appropriate

---

**Good luck, Sud! You've got this. The preparation work you're putting in will show.**
