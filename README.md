# healthcare-provider-acme - Configuration Documentation

> **Generated:** 2026-02-22T10:15:40.581203Z
> **Profile:** Standard
> **Organization:** acme.com

---

## Executive Summary

This document describes the Cloud Foundation configuration for **acme.com**. This establishes your GCP Landing Zone.

| Attribute | Value |
|-----------|-------|
| Cloud Foundation Name | `healthcare-provider-acme` |
| Organization ID | `` |
| Primary Region | `us-east1` |
| Configuration Profile | Standard |
| Architecture Type | Shared VPC |

| Compliance Frameworks | SOC2, HIPAA, CIS |


| Organization Policies | 10 enforced |


| Log Retention | 2190 days |

| Billing Account | `` |


### Compliance Requirements

This cloud foundation is configured to support:

- **SOC2**

- **HIPAA**

- **CIS**



---

## 1. Organization Structure

### 1.1 Folder Hierarchy


```
acme.com ()
│

├── 📁 Production
│   └── Purpose: environment

├── 📁 Staging
│   └── Purpose: environment

├── 📁 Development
│   └── Purpose: environment

├── 📁 Shared Services
│   └── Purpose: shared_services

├── 📁 Security
│   └── Purpose: security

```

| Folder | Purpose | Description |
|--------|---------|-------------|

| Production | environment | Production workloads |

| Staging | environment | Pre-production testing |

| Development | environment | Development |

| Shared Services | shared_services | Common infrastructure |

| Security | security | Security tooling |



### 1.2 Bootstrap Projects


| Project Name | Folder | Purpose | APIs |
|--------------|--------|---------|------|

| `prj-seed-cicd` | Shared Services | cicd | cloudbuild.googleapis.com, artifactregistry.googleapis.com |

| `prj-seed-logging` | Security | logging | logging.googleapis.com |

| `prj-seed-networking` | Shared Services | networking | compute.googleapis.com, servicenetworking.googleapis.com |



### 1.3 Environments


Configured environments: **Development, Staging, Production**


---

## 2. Identity & Access Management

### 2.1 Administrative Groups


| Group Name | Purpose | Roles |
|------------|---------|-------|

| `gcp-organization-admins@acme.com` | org_admin | roles/resourcemanager.organizationAdmin |

| `gcp-billing-admins@acme.com` | billing_admin | roles/billing.admin |

| `gcp-network-admins@acme.com` | network_admin | roles/compute.networkAdmin |

| `gcp-security-admins@acme.com` | security_admin | roles/iam.securityAdmin |





### 2.3 Service Accounts


| Name | Project | Purpose | Roles |
|------|---------|---------|-------|

| `terraform-org-sa` | prj-seed-cicd | terraform | roles/resourcemanager.projectCreator, roles/resourcemanager.folderAdmin |

| `cicd-deploy-sa` | prj-seed-cicd | cicd | roles/clouddeploy.operator, roles/cloudbuild.builds.editor, roles/artifactregistry.writer |



---

## 3. Networking

### 3.1 Network Architecture

| Attribute | Value |
|-----------|-------|
| Architecture Type | Shared VPC |

### 3.2 VPC Networks


| VPC Name | Project | Routing Mode | Purpose |
|----------|---------|--------------|---------|

| `vpc-shared-prod` | prj-network-prod | GLOBAL | production |

| `vpc-shared-dev` | prj-network-dev | GLOBAL | non_production |



### 3.3 Subnets


| Subnet | VPC | Region | CIDR | Private Google Access |
|--------|-----|--------|------|----------------------|

| `sb-prod-us-east1` | vpc-shared-prod | us-east1 | `10.0.0.0/20` | Yes |

| `sb-dev-us-east1` | vpc-shared-dev | us-east1 | `10.1.0.0/20` | Yes |

| `sb-prod-us-west1` | vpc-shared-prod | us-west1 | `10.128.0.0/20` | Yes |

| `sb-dev-us-west1` | vpc-shared-dev | us-west1 | `10.129.0.0/20` | Yes |



---



## 4. Hybrid Connectivity

| Attribute | Value |
|-----------|-------|
| Connectivity Type | Partner Interconnect |

| VPN Type | HA VPN |
| Routing | Dynamic |




### 4.1 On-Premises Networks

| Network Name | CIDR Ranges |
|-------------|-------------|

| on-prem-network | 192.20.0.0/20 |




### 4.2 Hybrid DNS

| Setting | Value |
|---------|-------|
| Inbound Forwarding | Enabled |



---



## 5. Security Configuration

### 5.1 Organization Policies


10 organization policies configured:

| Constraint | Enforcement | Scope |
|---|---|---|

| compute.skipDefaultNetworkCreation | enforce | organization |

| compute.requireOsLogin | enforce | organization |

| compute.requireShieldedVm | enforce | organization |

| compute.disableSerialPortAccess | enforce | organization |

| compute.disableNestedVirtualization | enforce | organization |

| compute.vmExternalIpAccess | deny_all | organization |

| storage.uniformBucketLevelAccess | enforce | organization |

| storage.publicAccessPrevention | enforce | organization |

| sql.restrictPublicIp | enforce | organization |

| iam.disableServiceAccountKeyCreation | enforce | organization |



---

## 6. Logging & Monitoring

### 6.1 Log Retention

| Setting | Value |
|---------|-------|
| Default Retention Period | 2190 days |


### 6.2 Custom Retention Buckets

| Bucket Name | Retention (Days) | Locked |
|-------------|-----------------|--------|

| audit-logs | 365 | No |




### 6.3 Centralized Logging

| Setting | Value |
|---------|-------|
| Logging Project | prj-seed-logging |

| Aggregated Sinks | 1 configured |



---



## 7. Backup & Disaster Recovery

| Attribute | Value |
|-----------|-------|
| DR Strategy | Backup Restore |
| Failover Automation | Disabled |

| Default RPO | 24h |
| Default RTO | 4h |


| Primary Region | us-east1 |
| DR Region | us-west1 |



### 7.1 Backup Policies

| Policy Name | Resource Type | Frequency | Retention (Days) | Cross-Region |
|-------------|--------------|-----------|-----------------|-------------|

| daily-compute-snapshots | compute_disk | daily | 30 | No |

| daily-sql-backup | cloud_sql | daily | 30 | No |




### 7.2 Failover Testing

| Setting | Value |
|---------|-------|
| Testing Frequency | annually |
| Test Type | tabletop |


---



## 8. Cost Management

### 8.1 Budgets


| Budget Name | Amount | Scope |
|-------------|--------|-------|

| Production Budget | USD 5000 | folder |

| Non-Production Budget | USD 2000 | folder |



---

## What Was Generated

This wizard generated **FAST factory YAML data files** — structured configuration that plugs directly into Google Cloud's [FAST Fabric](https://github.com/GoogleCloudPlatform/cloud-foundation-fabric/tree/master/fast) landing zone framework.

| Attribute | Value |
|-----------|-------|
| Output Format | FAST Factory YAML |
| Framework | Cloud Foundation Fabric (FAST) |

| Stages Generated | 5 |



### FAST Stages Overview

| Stage | Directory | Description |
|-------|-----------|-------------|

| Organization Setup | `org-setup/` | Folders, IAM bindings, org policies, tags, billing |

| Networking | `networking/` | VPC networks, subnets, firewall rules, DNS, VPNs |

| Security | `security/` | KMS keyrings, security projects, SCC |

| Project Factory | `project-factory/` | Workload projects (GKE, data, apps, compute, ops) |

| VPC Service Controls | `vpcsc/` | Service perimeters, access levels, ingress/egress policies |



### How to Deploy

#### Prerequisites

- [Cloud Foundation Fabric](https://github.com/GoogleCloudPlatform/cloud-foundation-fabric) repository cloned
- GCP Organization with appropriate permissions
- Terraform >= 1.7 installed
- A service account or user with Organization Admin privileges

#### Deployment Steps

1. **Clone FAST Fabric** (if not already done):
   ```bash
   git clone https://github.com/GoogleCloudPlatform/cloud-foundation-fabric.git
   cd cloud-foundation-fabric/fast
   ```

2. **Place the generated data files** into the corresponding FAST stage directories:


   - Copy `org-setup/` contents into the FAST `org-setup/` stage data directory

   - Copy `networking/` contents into the FAST `networking/` stage data directory

   - Copy `security/` contents into the FAST `security/` stage data directory

   - Copy `project-factory/` contents into the FAST `project-factory/` stage data directory

   - Copy `vpcsc/` contents into the FAST `vpcsc/` stage data directory



3. **Deploy stages in order**:


   - **Stage 0**: Organization Setup (`org-setup/`)

   - **Stage 1**: Networking (`networking/`)

   - **Stage 2**: Security (`security/`)

   - **Stage 3**: Project Factory (`project-factory/`)

   - **Stage 4**: VPC Service Controls (`vpcsc/`)



4. **Review and apply** each stage with Terraform:
   ```bash
   terraform init
   terraform plan
   terraform apply
   ```

### FAST Data File Format

The generated YAML files use FAST's factory data format with `$`-interpolation tokens that are resolved at `terraform plan` time:

- `$iam_principals:...` — References to IAM identities
- `$project_ids:...` — References to project IDs from the FAST registry
- `$folder_ids:...` — References to folder IDs

These tokens ensure that cross-stage dependencies are resolved automatically by FAST.

### Getting Help

- **FAST Documentation**: See [FAST README](https://github.com/GoogleCloudPlatform/cloud-foundation-fabric/blob/master/fast/README.md)
- **Stage-specific docs**: Each FAST stage directory contains its own README
- **Community**: [r/googlecloud](https://reddit.com/r/googlecloud), [Stack Overflow](https://stackoverflow.com/questions/tagged/google-cloud-platform)


---

## Important Disclaimers

1. **No Warranty**: These configurations are generated based on your inputs. Review thoroughly before any deployment.

2. **Security Review Required**: Have your security team review IAM bindings and org policies before deployment.

3. **Cost Implications**: Deploying this infrastructure will incur GCP charges. Review the Cost Management section.


4. **Not Standalone**: The YAML data files require FAST Fabric modules to deploy. They are not standalone Terraform.


5. **Your Responsibility**: Actual deployment, testing, and maintenance are your responsibility.

---

## Contacts

| Role | Email |
|------|-------|
| Primary Contact | cloudteam@acme.com |


---

*Generated by Cloud Foundation Design Studio v1.0.0*