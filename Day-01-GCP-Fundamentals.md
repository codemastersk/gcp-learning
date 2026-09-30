# Day 01: Google Cloud Platform (GCP) Fundamentals

**Learning Path:** Azure to GCP | Cloud & DevOps Engineering
**Day:** 01
**Topics:** GCP Architecture, Organization, Folders, Projects, Regions, Zones, IAM, Billing and gcloud CLI

## 1. Introduction to Google Cloud Platform

Google Cloud Platform (GCP) is a cloud computing platform provided by Google. It offers services for computing, networking, storage, databases, Kubernetes, security, monitoring and automation.

Instead of maintaining physical servers and data centers, organizations can use GCP to provision and manage infrastructure on demand.

### Why do organizations use GCP?

* **Scalability:** Scale infrastructure according to application requirements.
* **High Availability:** Deploy workloads across multiple zones.
* **Security:** Manage identities, permissions and security policies.
* **Automation:** Provision infrastructure using Terraform and gcloud CLI.
* **Monitoring:** Monitor applications and infrastructure using Cloud Monitoring and Cloud Logging.
* **Cost Management:** Track resource usage and optimize cloud spending.

### Common GCP Services

| Category           | GCP Service                    | Purpose                             |
| ------------------ | ------------------------------ | ----------------------------------- |
| Compute            | Compute Engine                 | Virtual machines                    |
| Networking         | VPC                            | Cloud networking                    |
| Storage            | Cloud Storage                  | Object storage                      |
| Containers         | Google Kubernetes Engine (GKE) | Managed Kubernetes                  |
| CI/CD              | Cloud Build                    | Build and deployment automation     |
| Container Registry | Artifact Registry              | Store container images and packages |
| Identity           | Cloud IAM                      | Identity and access management      |
| Monitoring         | Cloud Monitoring               | Metrics, dashboards and alerts      |
| Logging            | Cloud Logging                  | Collect and analyze logs            |
| Security           | Secret Manager                 | Store application secrets           |
| Database           | Cloud SQL                      | Managed relational databases        |

## 2. GCP Architecture and Resource Hierarchy

GCP provides a resource hierarchy to organize cloud infrastructure, manage access and apply governance policies.

**Hierarchy:**

Organization → Folders → Projects → Resources

### Organization

An Organization is the top-level resource that represents a company in GCP.

It provides centralized management of resources, IAM permissions, policies and governance.

**Key responsibilities:**

* Centralized resource management
* Organization-level IAM
* Security and governance
* Organization Policies

### Folders

Folders help organize projects into logical groups based on teams, departments or environments.

For example:

* DevOps
* Development
* Testing
* Production

Folders can contain other folders and projects. IAM permissions and supported policies can be inherited by child resources.

### Projects

A Project is a fundamental resource container in GCP. Most cloud resources are created within a project.

**Key features:**

* Resource organization
* API enablement
* IAM access management
* Quota management
* Billing association

Each project has a unique Project ID and a Project Number.

Example:

* Project Name: Development Environment
* Project ID: company-dev-001
* Project Number: A Google-generated numeric identifier

A Project ID cannot be changed after creation.

### Resources

Resources are the actual cloud services created within projects.

Examples:

* Compute Engine VM
* VPC Network
* Subnets
* Cloud Storage Buckets
* GKE Clusters
* Cloud SQL Instances

**Example architecture:**

```text
Organization
│
├── DevOps Folder
│   ├── Development Project
│   │   ├── VPC
│   │   ├── Subnet
│   │   ├── Compute Engine VM
│   │   └── Cloud Storage
│   │
│   └── Production Project
│       ├── VPC
│       ├── GKE Cluster
│       └── Cloud SQL
│
└── Development Team Folder
    └── Application Project
        ├── Compute Engine
        └── Cloud Storage
```

## 3. GCP Regions and Zones

Google Cloud infrastructure is distributed across geographical locations called Regions and Zones.

### Region

A Region is a specific geographical area where GCP resources are available.

Examples:

* `asia-south1` — Mumbai
* `asia-south2` — Delhi
* `us-central1` — Iowa
* `europe-west1` — Belgium

Regions help organizations select infrastructure locations based on latency, availability, compliance and cost.

### Zone

A Zone is an isolated deployment area within a Region. Zonal resources, such as Compute Engine VMs, are deployed in a specific zone.

Example:

```text
Region: asia-south1 (Mumbai)
│
├── asia-south1-a
├── asia-south1-b
└── asia-south1-c
```

### Single-Zone vs Multi-Zone Deployment

**Single-Zone Deployment**

An application runs in one zone. It can be simpler to manage, but a failure in that zone can affect the application.

**Multi-Zone Deployment**

An application is distributed across multiple zones within a region. With suitable load balancing, health checks and data replication, this can improve availability and resilience.

### Region vs Zone

| Region                                 | Zone                           |
| -------------------------------------- | ------------------------------ |
| Geographical location                  | Isolated area within a region  |
| Contains multiple zones                | Belongs to one region          |
| Used for regional deployment decisions | Used to deploy zonal resources |
| Example: asia-south1                   | Example: asia-south1-a         |

**Important:** GCP VPC networks are global resources, while their subnets are regional. This is an important architectural difference from Azure VNets, which are regional.

## 4. Azure vs GCP

As a DevOps Engineer with Azure experience, understanding the similarities and differences between both platforms helps in cloud migration and multi-cloud infrastructure management.

| Microsoft Azure                | Google Cloud                    |
| ------------------------------ | ------------------------------- |
| Microsoft Entra ID / Tenant    | Cloud Identity and Organization |
| Management Groups              | Folders                         |
| Subscription                   | Project                         |
| Resource Group                 | No exact equivalent             |
| Virtual Network (VNet)         | VPC Network                     |
| Subnet                         | Subnet                          |
| Azure Virtual Machine          | Compute Engine                  |
| Azure Blob Storage             | Cloud Storage                   |
| Azure Kubernetes Service (AKS) | Google Kubernetes Engine (GKE)  |
| Azure Container Registry       | Artifact Registry               |
| Azure Monitor                  | Cloud Monitoring                |
| Log Analytics                  | Cloud Logging                   |
| Azure Key Vault                | Secret Manager / Cloud KMS      |
| Azure RBAC                     | Cloud IAM                       |
| Azure Policy                   | Organization Policy Service     |
| Azure Pipelines                | Cloud Build / Azure Pipelines   |
| NAT Gateway                    | Cloud NAT                       |

These are functional comparisons, not exact one-to-one equivalents. The architecture, capabilities and configuration options can differ.

## 5. Identity and Access Management (IAM)

Cloud IAM controls who can access resources and what actions they can perform.

IAM is based on three main components:

### Principal

A principal is an identity that can receive permissions.

Examples:

* Google Account
* Google Group
* Service Account
* Federated Identity

### Role

A role is a collection of permissions that determines which actions a principal can perform.

Common role types:

* **Basic Roles:** Owner, Editor and Viewer. These are broad roles and should be used cautiously.
* **Predefined Roles:** Google-managed roles designed for specific services and tasks.
* **Custom Roles:** Organization-defined collections of permissions for specific requirements.

### Resource

A resource is the GCP object on which access is granted, such as an Organization, Folder, Project, VM or Storage Bucket.

### Example: Granting Compute Viewer Access

```bash
gcloud projects add-iam-policy-binding company-dev-001 \
  --member="user:developer@company.com" \
  --role="roles/compute.viewer"
```

This grants the user the Compute Viewer role on the specified project.

### Service Accounts

A Service Account is a machine identity used by applications, workloads and automation tools to authenticate to Google Cloud.

For example, a Terraform pipeline can use an authorized service identity to create and manage infrastructure.

For production CI/CD pipelines, Workload Identity Federation is a recommended keyless authentication approach when supported by the external identity provider. It avoids the need to maintain long-lived service account keys.

### Principle of Least Privilege

Always grant only the permissions required to perform a particular task. Avoid unnecessary Owner or Editor access, especially in production environments.

## 6. GCP Billing

GCP Billing is used to track and manage cloud resource costs.

A billing account can be linked to multiple projects, subject to account configuration and permissions.

### Important Billing Features

* Project-wise cost tracking
* Billing reports
* Budget creation
* Budget alerts
* Cost analysis
* Resource optimization

**Important:** A budget alert does not automatically stop resources when spending reaches the budget amount. Automatic actions require separately configured automation.

## 7. Google Cloud Console

Google Cloud Console is a web-based graphical interface for managing GCP resources.

Using the Console, engineers can:

* Create and manage projects
* Configure IAM permissions
* Deploy virtual machines
* Create VPC networks
* Configure monitoring and logging
* Review billing and usage

Console: https://console.cloud.google.com/

## 8. gcloud CLI

The Google Cloud CLI is a command-line tool used to manage Google Cloud resources and automate common operations.

### Useful Commands

**Check authenticated accounts**

```bash
gcloud auth list
```

**View the current project**

```bash
gcloud config get-value project
```

**Set the active project**

```bash
gcloud config set project company-dev-001
```

**List available regions**

```bash
gcloud compute regions list
```

**List available zones**

```bash
gcloud compute zones list
```

**List Compute Engine instances**

```bash
gcloud compute instances list
```

## 9. GCP in a DevOps Workflow

A typical GCP DevOps workflow can look like this:

```text
Developer
    |
    v
Git Repository
    |
    v
CI/CD Pipeline
    |
    v
Authentication to GCP
    |
    v
Terraform
    |
    v
GCP Infrastructure
    |
    +---- VPC
    |
    +---- Compute Engine
    |
    +---- GKE
    |
    +---- Cloud Storage
    |
    v
Cloud Monitoring and Logging
```

In a production environment, IAM, security policies, infrastructure automation, monitoring and cost management work together to maintain reliable cloud infrastructure.

## 10. Key Learnings

* Understood the fundamentals of Google Cloud Platform.
* Learned the GCP resource hierarchy: Organization, Folders, Projects and Resources.
* Explored the difference between Regions and Zones.
* Compared Azure services with their functional GCP counterparts.
* Learned the fundamentals of IAM, Roles, Principals and Service Accounts.
* Understood GCP Billing and cost management.
* Explored Google Cloud Console and basic gcloud CLI commands.
* Learned how GCP fits into a typical DevOps workflow.

## 11. Interview Preparation

**Q1. What is GCP?**

GCP is Google's cloud computing platform that provides infrastructure and managed services for computing, networking, storage, databases, security and application deployment.

**Q2. What is the difference between a Region and a Zone?**

A Region is a geographical area containing multiple zones. A Zone is an isolated deployment area within a region where zonal resources can be deployed.

**Q3. What is a GCP Project?**

A Project is a logical boundary for organizing resources, enabling APIs, managing IAM, quotas and billing.

**Q4. What is IAM in GCP?**

IAM is an access management service that controls which principals can perform which actions on specific resources through roles and permissions.

**Q5. What is a Service Account?**

A Service Account is a machine identity used by applications and automation tools to access GCP resources securely.

**Q6. What is the difference between Azure VNet and GCP VPC?**

Azure VNet is regional, whereas GCP VPC is global. GCP subnets are regional, and they can be created in different regions within the same VPC.

## 12. Resources

* [Google Cloud Documentation](https://cloud.google.com/docs)
* [GCP Resource Hierarchy](https://cloud.google.com/resource-manager/docs/cloud-platform-resource-hierarchy)
* [GCP IAM Documentation](https://cloud.google.com/iam/docs)
* [GCP Regions and Zones](https://cloud.google.com/compute/docs/regions-zones)
* [Google Cloud CLI](https://cloud.google.com/sdk/docs/install)

---

**Next:** Day 02 — GCP Networking: VPC, Subnets, CIDR, Firewall Rules and Routes.

