# AWS CloudOps Engineer Vocabulary Part 4

<details>
<summary>76. AWS WAF</summary>

Definition: A web application firewall that lets you monitor and manage web requests that are forwarded to protected AWS resources.

</details>

<br>

<details>
<summary>77. AWS Backup</summary>

Definition: A fully managed backup service that makes it easy to centralize and automate the backup of data across AWS services in the cloud as well as on premises.

</details>

<br>

<details>
<summary>78. Amazon Elastic Block Store (Amazon EBS)</summary>

Definition: Provides block level storage volumes for use with EC2 instances.

</details>

<br>

<details>
<summary>79. Amazon Elastic File System (Amazon EFS)</summary>

Definition: It is built to scale on demand to petabytes without disrupting applications, growing and shrinking automatically as you add and remove files.

</details>

<br>

<details>
<summary>80. Amazon FSx</summary>

Definition: Makes it easy and cost effective to launch, run, and scale feature-rich, high performance file systems in the cloud.

</details>

<br>

<details>
<summary>81. Amazon S3</summary>

Definition: It is an object storage service that offers industry-leading scalability, data availability, security, and performance.

</details>

<br>

<details>
<summary>82. Amazon S3 Glacier</summary>

Definition: This is a storage service that is designed to store data that is not frequently accessed or used, also known as "cold data."

</details>

<br>

<details>
<summary>83. AWS Storage Gateway</summary>

Definition: A service that connects an on-premises software appliance with cloud-based storage to provide seamless and secure integration between your on-premises IT environment and the AWS storage infrastructure in the AWS Cloud.

</details>

<br>

<details>
<summary>84. EBS Volume</summary>

Definition: A durable, block-level storage device that you can attach to your instances.

</details>

<br>

<details>
<summary>85. Read Replica</summary>

Definition: A read-only copy of a DB instance. Used to offload read traffic (like reporting queries) from the primary database.

</details>

<br>

<details>
<summary>86. AWS Step Functions</summary>

Definition: A visual workflow manager that coordinates multiple AWS services in a defined sequence, handling retries, parallel execution, and error handling automatically.

</details>

<br>

<details>
<summary>87. Scale Up (Vertical Scaling)</summary>

Definition: Get a BIGGER machine. Increase capacity by using a larger instance type with more CPU, RAM, or storage.

</details>

<br>

<details>
<summary>88. Scale Down (Vertical Scaling)</summary>

Definition: Get a SMALLER machine. Reduce capacity by using a smaller instance type to save costs.

</details>

<br>

<details>
<summary>89. Scale Out (Horizontal Scaling)</summary>

Definition: Add MORE machines. Increase capacity by adding more instances to distribute the load.

</details>

<br>

<details>
<summary>90. Scale In (Horizontal Scaling)</summary>

Definition: Remove machines. Reduce capacity by terminating instances when demand decreases.

</details>

<br>

<details>
<summary>91. Spot Instances</summary>

Definition: Enables you to request unused EC2 instances at steep discounts (up to 90%), but can be interrupted with 2-minute warning. Best for flexible, fault-tolerant workloads.

</details>

<br>

<details>
<summary>92. On-Demand Instances</summary>

Definition: You pay for compute capacity by the second with no long-term commitments. Most flexible but most expensive option.

</details>

<br>

<details>
<summary>93. Reserved Instances</summary>

Definition: Provides significant savings (up to 72%) on your Amazon EC2 costs compared to On-Demand pricing in exchange for a 1 or 3 year commitment.

</details>

<br>

<details>
<summary>94. AWS Direct Connect</summary>

Definition: Links your internal network to an AWS Direct Connect location over a standard Ethernet fiber-optic cable for dedicated, private connectivity to AWS.

</details>

<br>

<details>
<summary>95. Convertible Reserved Instances</summary>

Definition: Can be exchanged during the term for another Convertible Reserved Instance with new attributes, including instance family, instance type, platform, scope, or tenancy.

</details>

<br>

<details>
<summary>96. NAT Gateway</summary>

Definition: You can use this so that instances in a private subnet can connect to services outside your VPC, but external services cannot initiate a connection with those instances.

</details>

<br>

```
════════════════════════════════════════════════════════════════
                     📌 SECURITY SERVICES COMPARISON
════════════════════════════════════════════════════════════════
```

| Service | One-Liner |
|---------|-----------|
| **Inspector** | "Your software has known security holes" |
| **Access Analyzer** | "You're sharing resources with outsiders" |
| **Config** | "Your resources aren't configured to policy" |
| **GuardDuty** | "Someone's doing something shady" |

```
════════════════════════════════════════════════════════════════
                     📌 SCALING QUICK REFERENCE
════════════════════════════════════════════════════════════════
```

| Term | Direction | Method |
|------|-----------|--------|
| **Scale Up** | ⬆️ Vertical | Bigger instance |
| **Scale Down** | ⬇️ Vertical | Smaller instance |
| **Scale Out** | ➡️ Horizontal | More instances |
| **Scale In** | ⬅️ Horizontal | Fewer instances |

```
════════════════════════════════════════════════════════════════
                     📌 EC2 PURCHASING OPTIONS
════════════════════════════════════════════════════════════════
```

| Option | Commitment | Discount | Best For |
|--------|------------|----------|----------|
| **On-Demand** | None | None | Unpredictable, can't interrupt |
| **Reserved** | 1-3 years | Up to 72% | Steady, predictable usage |
| **Spot** | None | Up to 90% | Flexible, can be interrupted |
| **Dedicated Hosts** | Per host | Up to 70% | BYOL, compliance |