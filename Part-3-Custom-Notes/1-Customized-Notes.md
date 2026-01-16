### Part 3: Customized Notes:

```
════════════════════════════════════════════════════════════════
  📌 IAM Access Analyzer, AWS Config, GuardDuty, AWS Inspector
════════════════════════════════════════════════════════════════
```


```
┌─────────────────────────────────────────────────────────────────┐
│                     YOUR HOUSE (AWS Resources)                  │
└─────────────────────────────────────────────────────────────────┘

🔑 IAM Access Analyzer:
   "Who has keys to your house? Did you give keys to strangers?"
   (Checks WHO can access)

⚙️ AWS Config:
   "Is your smoke detector installed? Is the door the right type?"
   (Checks CONFIGURATION against rules)

👀 GuardDuty:
   "Is someone sneaking around your house right now?"
   (Detects suspicious ACTIVITY)

🔍 Amazon Inspector:
   "Your pipes are made of lead. Your wiring is faulty. 
    Your foundation has cracks."
   (Scans for VULNERABILITIES in the materials themselves)
```

| Service | One-Liner |
|---------|-----------|
| **Inspector** | "Your software has known security holes" |
| **Access Analyzer** | "You're sharing resources with outsiders" |
| **Config** | "Your resources aren't configured to policy" |
| **GuardDuty** | "Someone's doing something shady" |

```
════════════════════════════════════════════════════════════════
                     📌 AURORA ENDPOINTS
════════════════════════════════════════════════════════════════
```

## 21. Aurora Reader and Writer Endpoints

**What it is:** Aurora provides different endpoints to route traffic to the right instances automatically.

### Four Endpoint Types:

| Endpoint | Purpose | Use Case |
|----------|---------|----------|
| **Cluster (Writer)** | Points to primary instance | Writes (INSERT, UPDATE, DELETE) |
| **Reader** | Load balances across replicas | Read scaling |
| **Instance** | Points to one specific instance | Troubleshooting |
| **Custom** | Points to subset you define | Analytics on specific replicas |

**Key Insight:** Use cluster endpoint, NOT instance endpoint. Instance endpoints don't redirect after failover.

### Failover Behavior:

| Endpoint Type | After Failover |
|---------------|----------------|
| **Cluster (Writer)** | ✅ Automatically points to new primary |
| **Reader** | ✅ Automatically excludes failed instance |
| **Instance** | ❌ Still points to old (dead) instance — app breaks |

### URL Pattern:
```
Writer:  cluster-xxx.rds.amazonaws.com
Reader:  cluster-ro-xxx.rds.amazonaws.com  (note the "-ro-")
```

### Exam Triggers:

- "Distribute read traffic" → Reader endpoint
- "App fails after Aurora failover" → Probably using instance endpoint
- "Separate analytics queries" → Custom endpoint


```
════════════════════════════════════════════════════════════════
                     📌 SURGEQUEUELENGTH
════════════════════════════════════════════════════════════════
```

## SurgeQueueLength

**Definition:** A count of the total number of requests that are pending submission to a registered instance.

### The Analogy: Restaurant Waiting Line
```
Customers (requests) ──► Waiting Line ──► Kitchen (EC2 instances)
                              │
                    SurgeQueueLength = 
                    How many people in line
```

### What It Means:

| SurgeQueueLength | Meaning |
|------------------|---------|
| **Low (near 0)** | Requests being handled quickly ✅ |
| **High (growing)** | Backend can't keep up ⚠️ |
| **Max (1,024)** | Queue full — new requests rejected ❌ |

### Key Facts:

- **Applies to:** Classic Load Balancer only (ALB/NLB use different metrics)
- **Max queue size:** 1,024
- **When queue is full:** Load balancer returns HTTP 503 (Service Unavailable)

### What To Do If High:

| Action | Why |
|--------|-----|
| Add more instances | More capacity to handle requests |
| Scale up instance size | Faster processing per instance |
| Check backend health | Unhealthy instances = fewer workers |

### Exam Triggers:

- "Requests pending at load balancer" → SurgeQueueLength
- "Classic Load Balancer queue metric" → SurgeQueueLength
- "HTTP 503 errors from CLB" → Queue might be full (check SurgeQueueLength)

```
════════════════════════════════════════════════════════════════
         📌 AWS SERVICE CATALOG TAGOPTION LIBRARY
════════════════════════════════════════════════════════════════
```

## AWS Service Catalog TagOption Library

**Definition:** Enforces the tagging of all instances that will be launched in the VPC.

### The Analogy: Name Tag Policy at a Conference
```
Conference Rule: "Everyone MUST wear a name tag with department and role"

Without TagOption Library:
  Person walks in → No name tag → Nobody knows who they are 😕

With TagOption Library:
  Person walks in → MUST pick from approved tags → Everyone labeled ✅
```

### What It Does:

| Feature | Description |
|---------|-------------|
| **Enforces tags** | Resources MUST have required tags at launch |
| **Standardizes values** | Users pick from predefined tag values |
| **Centralized management** | Admins define tags once, apply everywhere |

### How It Works:
```
Admin creates TagOptions:
  - Environment: [dev, staging, prod]
  - CostCenter: [engineering, marketing, sales]
  - Owner: [team-a, team-b, team-c]

User launches resource:
  → MUST select from these options
  → Can't skip or make up their own
```

### Why Tags Matter:

| Use Case | How Tags Help |
|----------|---------------|
| **Cost allocation** | "How much is each team spending?" |
| **Compliance** | "Which resources belong to which project?" |
| **Automation** | "Delete all resources tagged 'temp'" |
| **Access control** | "Only allow access to resources tagged 'prod'" |

### Service Catalog Context:

| Component | Purpose |
|-----------|---------|
| **Service Catalog** | IT-approved products users can launch |
| **TagOption Library** | Enforces consistent tagging on those products |
| **Portfolio** | Collection of products with TagOptions attached |

### Exam Triggers:

- "Enforce tagging on provisioned resources" → TagOption Library
- "Standardize tag values across organization" → TagOption Library
- "Ensure cost allocation tags are applied" → TagOption Library
- "Service Catalog governance" → TagOption Library

```
════════════════════════════════════════════════════════════════
            📌 AWS SYSTEMS MANAGER PATCH MANAGER
════════════════════════════════════════════════════════════════
```

## AWS Systems Manager Patch Manager

**Definition:** Automates the process of patching managed instances with security-related updates.

### The Analogy: Automated Building Maintenance
```
Without Patch Manager:
  IT admin → SSH into server 1 → Run updates → SSH into server 2 → Repeat 100x 😩

With Patch Manager:
  Define schedule → Patch Manager updates all 100 servers automatically ✅
```

### How It Works:
```
┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
│  Patch Baseline │ ──► │ Maintenance     │ ──► │ Instances get   │
│  (What to patch)│     │ Window (When)   │     │ patched         │
└─────────────────┘     └─────────────────┘     └─────────────────┘
```

### Key Components:

| Component | What It Does |
|-----------|--------------|
| **Patch Baseline** | Defines WHICH patches to apply (critical, security, all) |
| **Patch Group** | Groups instances that get the same baseline |
| **Maintenance Window** | Defines WHEN patching happens |
| **Compliance Report** | Shows which instances are patched/unpatched |

### Patch Baselines:

| Baseline Type | Description |
|---------------|-------------|
| **AWS-Managed** | Predefined baselines for common OS (Amazon Linux, Windows, etc.) |
| **Custom** | You define which patches to include/exclude |

### Example Custom Baseline:
```
Include:
  - Severity: Critical, Important
  - Classification: Security

Exclude:
  - Specific patch that breaks our app (KB12345)

Auto-approve after: 7 days
```

### Maintenance Windows:

| Setting | Example |
|---------|---------|
| **Schedule** | Every Sunday at 2:00 AM |
| **Duration** | 4 hours |
| **Cutoff** | Stop starting new tasks 1 hour before end |

### Compliance Dashboard:

| Status | Meaning |
|--------|---------|
| **Compliant** | All required patches installed ✅ |
| **Non-Compliant** | Missing required patches ⚠️ |
| **Unknown** | Instance not reporting |

### What It Patches:

| OS | Supported |
|----|-----------|
| Amazon Linux | ✅ |
| Ubuntu | ✅ |
| Windows Server | ✅ |
| RHEL | ✅ |
| CentOS | ✅ |
| macOS | ✅ |

### Exam Triggers:

- "Automate OS patching" → Patch Manager
- "Security updates across fleet" → Patch Manager
- "Patch compliance reporting" → Patch Manager
- "Schedule patching during maintenance window" → Patch Manager
- "Which instances are missing patches?" → Patch Manager compliance

### Patch Manager vs Other Tools:

| Tool | Purpose |
|------|---------|
| **Patch Manager** | OS and application patching |
| **AWS Config** | Configuration compliance |
| **Inspector** | Vulnerability scanning (finds issues) |
| **Patch Manager** | Fixes the issues Inspector finds |

```
════════════════════════════════════════════════════════════════
                     📌 AMAZON ELASTICACHE
════════════════════════════════════════════════════════════════
```

## Amazon ElastiCache

**Definition:** A fully managed, Redis and Memcached compatible service delivering real-time, cost-optimized performance for modern applications.

### The Analogy: Sticky Notes on Your Monitor
```
Without Cache:
  "What's the capital of France?" → Walk to library → Look it up → "Paris"
  "What's the capital of France?" → Walk to library → Look it up → "Paris"
  (Same slow trip every time)

With Cache:
  "What's the capital of France?" → Walk to library → Look it up → "Paris"
  Write "France = Paris" on sticky note
  "What's the capital of France?" → Check sticky note → "Paris" ⚡ Instant!
```

### What It Does:
```
┌─────────────┐      ┌─────────────┐      ┌─────────────┐
│   User      │ ──►  │ ElastiCache │ ──►  │  Database   │
│             │      │  (Cache)    │      │  (RDS)      │
└─────────────┘      └─────────────┘      └─────────────┘
                            │
                     Check cache first
                     If found: Return instantly ⚡
                     If not: Query database, store in cache
```

### Two Flavors:

| Engine | Best For | Key Features |
|--------|----------|--------------|
| **Redis** | Complex data, persistence | Data structures, replication, persistence, pub/sub |
| **Memcached** | Simple caching, multithreaded | Pure caching, no persistence, multi-threaded |

### Redis vs Memcached:

| Feature | Redis | Memcached |
|---------|-------|-----------|
| **Data persistence** | ✅ Yes | ❌ No |
| **Replication** | ✅ Yes | ❌ No |
| **Multi-AZ failover** | ✅ Yes | ❌ No |
| **Complex data types** | ✅ Lists, sets, hashes | ❌ Simple key-value only |
| **Multi-threaded** | ❌ Single-threaded | ✅ Multi-threaded |
| **Use case** | Leaderboards, sessions, queues | Simple caching at scale |

### Memory Trick:
```
Redis    = "Rich" features (persistence, replication, data types)
Memcached = "Minimal" (simple, fast, multi-threaded)
```

### Common Use Cases:

| Use Case | Why Cache Helps |
|----------|-----------------|
| **Session storage** | Fast user session lookups |
| **Database query caching** | Reduce load on RDS |
| **Leaderboards** | Real-time sorted rankings (Redis) |
| **Real-time analytics** | Fast read/write for counters |
| **API response caching** | Speed up repeated API calls |

### ElastiCache for Redis — Cluster Mode:

| Mode | Description |
|------|-------------|
| **Cluster Mode Disabled** | 1 shard, up to 5 replicas, simpler |
| **Cluster Mode Enabled** | Multiple shards, data partitioned, more scalable |

### Exam Triggers:

- "Reduce database load" → ElastiCache
- "In-memory caching" → ElastiCache
- "Session storage" → ElastiCache (Redis)
- "Leaderboards, real-time rankings" → ElastiCache (Redis)
- "Simple caching, multi-threaded" → ElastiCache (Memcached)
- "Need persistence and replication" → Redis (not Memcached)
- "Microsecond latency" → ElastiCache

### ElastiCache vs DAX:

| Service | Caches For |
|---------|------------|
| **ElastiCache** | General purpose (RDS, APIs, sessions) |
| **DAX** | DynamoDB only (sits in front of DynamoDB) |

```
════════════════════════════════════════════════════════════════
                  📌 AMAZON OPENSEARCH SERVICE
════════════════════════════════════════════════════════════════
```

## 38. Amazon OpenSearch Service

**Definition:** A managed service that makes it easy for you to perform interactive log analytics, real-time application monitoring, and website search using an open-source, distributed search and analytics suite derived from Elasticsearch.

### The Analogy: Super-Powered Search Engine for Your Data
```
Without OpenSearch:
  "Find all error logs from last week containing 'timeout'" 
  → Manually grep through millions of log files 😩

With OpenSearch:
  "Find all error logs from last week containing 'timeout'"
  → Results in milliseconds + visualizations ⚡
```

### What It Does:
```
┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
│   Log Sources   │ ──► │   OpenSearch    │ ──► │   Dashboards    │
│                 │     │                 │     │                 │
│ • CloudWatch    │     │ • Index data    │     │ • Visualize     │
│ • CloudTrail    │     │ • Search fast   │     │ • Alerts        │
│ • VPC Flow Logs │     │ • Analyze       │     │ • Dashboards    │
│ • Application   │     │                 │     │                 │
└─────────────────┘     └─────────────────┘     └─────────────────┘
```

### Three Main Use Cases:

| Use Case | Example |
|----------|---------|
| **Log Analytics** | Search and analyze application/infrastructure logs |
| **Real-time Monitoring** | Dashboards showing live metrics and alerts |
| **Website Search** | Power the search bar on your website/app |

### What's the "Elasticsearch" Connection?
```
Elasticsearch (original open-source project)
        │
        ▼
AWS forked it when licensing changed
        │
        ▼
Amazon OpenSearch Service (AWS's managed version)
```

They're essentially the same technology — exam may use either name.

### Common Data Pipeline:
```
CloudWatch Logs ──► Subscription Filter ──► Kinesis Firehose ──► OpenSearch
        │
        └──► Real-time log analysis and dashboards
```

### Key Components:

| Component | Purpose |
|-----------|---------|
| **Domain** | Your OpenSearch cluster (like an RDS instance) |
| **Index** | Collection of documents (like a database table) |
| **Documents** | Individual records in JSON format |
| **OpenSearch Dashboards** | Visualization tool (formerly Kibana) |

### OpenSearch vs Other Services:

| Service | Best For |
|---------|----------|
| **OpenSearch** | Full-text search, log analytics, real-time dashboards |
| **Athena** | SQL queries on S3 data (ad-hoc analysis) |
| **CloudWatch Logs Insights** | Quick log queries (simpler, less powerful) |
| **QuickSight** | Business intelligence dashboards |

### When to Use OpenSearch:

| Scenario | Use OpenSearch? |
|----------|-----------------|
| "Search through millions of log entries" | ✅ Yes |
| "Real-time monitoring dashboard" | ✅ Yes |
| "Website search functionality" | ✅ Yes |
| "Simple CloudWatch metric alarm" | ❌ No — use CloudWatch |
| "One-time SQL query on S3" | ❌ No — use Athena |

### Key Features:

| Feature | Description |
|---------|-------------|
| **Full-text search** | Search across all fields instantly |
| **Near real-time** | Data searchable within seconds of ingestion |
| **Scalable** | Add nodes to handle more data |
| **OpenSearch Dashboards** | Built-in visualization (Kibana fork) |
| **Alerting** | Trigger alerts based on query results |

### Exam Triggers:

- "Log analytics" → OpenSearch
- "Search and analyze logs" → OpenSearch
- "Real-time application monitoring" → OpenSearch
- "Full-text search" → OpenSearch
- "Elasticsearch" → OpenSearch (same thing on AWS)
- "Kibana dashboards" → OpenSearch Dashboards
- "Centralized logging solution" → OpenSearch

### Common Exam Pattern:
```
CloudWatch Logs + Kinesis Firehose + OpenSearch = Centralized Log Analytics

VPC Flow Logs ──► CloudWatch Logs ──► Kinesis Firehose ──► OpenSearch ──► Dashboards
                                                                              │
                                                              "Visualize network traffic"
```

```
════════════════════════════════════════════════════════════════
                     📌 AWS OPSWORKS
════════════════════════════════════════════════════════════════
```

## 41. AWS OpsWorks

**Definition:** A configuration management service that provides managed instances of Chef and Puppet. (Think: AWS runs Chef/Puppet for you so you can automate server configuration without managing the Chef/Puppet infrastructure yourself.)

### The Analogy: Recipe Book for Servers
```
Without OpsWorks/Chef/Puppet:
  IT Admin → SSH into server 1 → Install Apache, configure settings
          → SSH into server 2 → Install Apache, configure settings
          → SSH into server 3... (repeat 100 times) 😩

With OpsWorks:
  Write ONE recipe: "Install Apache, set these configs"
  Apply to all 100 servers automatically ✅
```

### What Are Chef and Puppet?

| Tool | What It Does | Terminology |
|------|--------------|-------------|
| **Chef** | Configuration management | "Recipes" and "Cookbooks" |
| **Puppet** | Configuration management | "Manifests" and "Modules" |

Both do the same thing: **define server state as code and enforce it automatically.**

### The Problem OpsWorks Solves:
```
Without OpsWorks:
  ┌─────────────────────────────────────────┐
  │  You manage Chef/Puppet servers         │
  │  You handle updates, scaling, backups   │
  │  You maintain the infrastructure        │
  └─────────────────────────────────────────┘
                    😩 Extra work

With OpsWorks:
  ┌─────────────────────────────────────────┐
  │  AWS manages Chef/Puppet for you        │
  │  AWS handles updates, scaling, backups  │
  │  You just write recipes                 │
  └─────────────────────────────────────────┘
                    ✅ Focus on your app
```

### Three OpsWorks Flavors:

| Flavor | Description | Use Case |
|--------|-------------|----------|
| **OpsWorks for Chef Automate** | Fully managed Chef server | Already using Chef |
| **OpsWorks for Puppet Enterprise** | Fully managed Puppet server | Already using Puppet |
| **OpsWorks Stacks** | AWS's own simplified model | New to configuration management |

### OpsWorks Stacks Concepts:
```
┌─────────────────────────────────────────────────────────────┐
│                         STACK                               │
│                  (Your entire application)                  │
│                                                             │
│   ┌─────────────┐  ┌─────────────┐  ┌─────────────┐        │
│   │   LAYER     │  │   LAYER     │  │   LAYER     │        │
│   │  (Web)      │  │  (App)      │  │  (Database) │        │
│   │             │  │             │  │             │        │
│   │ ┌────┐┌────┐│  │ ┌────┐┌────┐│  │   ┌────┐    │        │
│   │ │EC2 ││EC2 ││  │ │EC2 ││EC2 ││  │   │RDS │    │        │
│   │ └────┘└────┘│  │ └────┘└────┘│  │   └────┘    │        │
│   └─────────────┘  └─────────────┘  └─────────────┘        │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

| Component | What It Is |
|-----------|------------|
| **Stack** | Container for your entire app (all layers) |
| **Layer** | Group of instances with same purpose (web, app, db) |
| **Instance** | EC2 instance within a layer |
| **App** | Code deployed to instances |
| **Recipe** | Chef code that configures instances |

### Lifecycle Events:

OpsWorks runs recipes at specific times:

| Event | When It Runs |
|-------|--------------|
| **Setup** | Instance finishes booting |
| **Configure** | Instance enters/leaves online state |
| **Deploy** | You deploy an app |
| **Undeploy** | You remove an app |
| **Shutdown** | Instance is stopped |

### OpsWorks vs Other Services:

| Service | Purpose | When to Use |
|---------|---------|-------------|
| **OpsWorks** | Configuration management (Chef/Puppet) | Complex server configs, existing Chef/Puppet |
| **CloudFormation** | Infrastructure as code (create resources) | Provisioning AWS resources |
| **Systems Manager** | Operations (patching, commands, inventory) | General management tasks |
| **Elastic Beanstalk** | Deploy apps (PaaS) | Simple deployments, no Chef needed |

### Key Decision:
```
Already using Chef/Puppet? 
  └──► OpsWorks (managed Chef/Puppet)

Starting fresh, simple needs?
  └──► Systems Manager or Elastic Beanstalk

Just need to provision infrastructure?
  └──► CloudFormation
```

### Exam Triggers:

- "Chef" → OpsWorks
- "Puppet" → OpsWorks
- "Configuration management" → OpsWorks
- "Recipes and cookbooks" → OpsWorks (Chef)
- "Manifests" → OpsWorks (Puppet)
- "Automate server configuration at scale" → OpsWorks
- "Migrate existing Chef workloads to AWS" → OpsWorks for Chef Automate

```
════════════════════════════════════════════════════════════════
                     📌 AWS GLOBAL ACCELERATOR
════════════════════════════════════════════════════════════════
```

## AWS Global Accelerator

**Definition:** A networking service that provides static IP addresses that act as a fixed entry point to your applications and eliminate the complexity of managing specific IP addresses for different AWS Regions and Availability Zones.

### The Analogy: Toll-Free 1-800 Number
```
Without Global Accelerator:
  Customer in Tokyo → Dials local number → Routed over public internet → Your app in Virginia
  (Slow, unpredictable routing)

With Global Accelerator:
  Customer in Tokyo → Dials 1-800 number → Nearest AWS edge → AWS private backbone → Your app
  (Fast, consistent, optimized routing)
```

### The Problem It Solves:
```
Without Global Accelerator:
  ┌────────────┐                                    ┌────────────┐
  │   User     │ ───── Public Internet ─────────►  │   Your     │
  │  (Tokyo)   │       (many hops, variable)       │   App      │
  └────────────┘                                    └────────────┘
                        😩 Slow, inconsistent

With Global Accelerator:
  ┌────────────┐     ┌─────────────┐     ┌─────────────────┐     ┌────────────┐
  │   User     │ ──► │  Nearest    │ ──► │  AWS Private    │ ──► │   Your     │
  │  (Tokyo)   │     │  Edge (TYO) │     │  Backbone       │     │   App      │
  └────────────┘     └─────────────┘     └─────────────────┘     └────────────┘
                                ✅ Fast, consistent
```

### Key Features:

| Feature | What It Means |
|---------|---------------|
| **Static IP addresses** | 2 static IPs that never change (anycast) |
| **Global edge network** | Traffic enters AWS at nearest edge location |
| **AWS backbone** | Traffic travels on AWS private network, not public internet |
| **Health checks** | Automatic failover to healthy endpoints |

### How It Works:
```
1. You get 2 static anycast IPs (e.g., 75.2.60.5, 99.83.190.102)
2. User connects to one of these IPs
3. Traffic enters AWS at nearest edge location
4. AWS routes over private backbone to your app
5. If endpoint fails, traffic automatically reroutes
```

### Static IPs — Why They Matter:

| Without Global Accelerator | With Global Accelerator |
|---------------------------|------------------------|
| IP changes if you redeploy | Same 2 IPs forever |
| Must update DNS, firewall rules | No changes needed |
| DNS propagation delays | Instant failover |
| Whitelist multiple IPs | Whitelist just 2 IPs |

### Global Accelerator vs CloudFront:

| Feature | Global Accelerator | CloudFront |
|---------|-------------------|------------|
| **Type** | Network layer (Layer 4) | Content delivery (Layer 7) |
| **Best for** | TCP/UDP apps, gaming, VoIP | HTTP/HTTPS, static content |
| **Static IPs** | ✅ Yes | ❌ No |
| **Caching** | ❌ No | ✅ Yes |
| **Use case** | Non-HTTP apps, IP whitelisting | Websites, APIs, video streaming |

### Memory Trick:
```
Global Accelerator = "Fast lane onto the AWS highway"
  - Enters at nearest on-ramp (edge)
  - Travels on private highway (backbone)
  - Exits at your app

CloudFront = "Local warehouse with copies of your stuff"
  - Caches content at edge
  - Serves content directly from edge
```

### When to Use Which:

| Scenario | Use |
|----------|-----|
| "Speed up website with caching" | CloudFront |
| "Static IPs for whitelisting" | Global Accelerator |
| "Gaming, VoIP, TCP/UDP apps" | Global Accelerator |
| "Video streaming, images" | CloudFront |
| "Instant failover between regions" | Global Accelerator |
| "HTTP API acceleration" | Either (GA for static IPs, CF for caching) |

### Endpoint Types:

Global Accelerator can route to:

| Endpoint | Description |
|----------|-------------|
| **ALB** | Application Load Balancer |
| **NLB** | Network Load Balancer |
| **EC2** | EC2 instances directly |
| **Elastic IP** | Static IPs on instances |

### Multi-Region Failover:
```
                    Global Accelerator
                    (2 static IPs)
                          │
            ┌─────────────┴─────────────┐
            ▼                           ▼
     ┌─────────────┐             ┌─────────────┐
     │  us-east-1  │             │  eu-west-1  │
     │    (ALB)    │             │    (ALB)    │
     │   Healthy   │             │   Healthy   │
     └─────────────┘             └─────────────┘
     
     If us-east-1 fails → Traffic automatically goes to eu-west-1
     No DNS changes, no IP changes, instant failover ✅
```

### Exam Triggers:

- "Static IP addresses for application" → Global Accelerator
- "Improve global application performance" → Global Accelerator
- "IP whitelisting for global app" → Global Accelerator
- "Instant failover across regions" → Global Accelerator
- "TCP/UDP acceleration" → Global Accelerator
- "Gaming, VoIP, real-time apps" → Global Accelerator
- "Caching content at edge" → CloudFront (not Global Accelerator)

### Quick Comparison Table:

| Need | Answer |
|------|--------|
| Static IPs | Global Accelerator |
| Caching | CloudFront |
| TCP/UDP apps | Global Accelerator |
| HTTP/HTTPS content | CloudFront |
| Both static IPs AND HTTP | Global Accelerator → ALB |

