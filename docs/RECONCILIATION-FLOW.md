# Flow

## 1. Platform Sync

### Source

This Repository (flux-capsule-multi-tenancy) + main Branch (this)

### Apply

> God reconciles this stuff.

#### Hierarchy

Clusters:
  - Staging Cluster:
    - Flux System
    - Infrastructure: Capsule infrastructure
    - Tenants: Staging tenants
  
    - Staging tenants:
      - Dev Team tenant
        - RBAC \*
        - Sync
          - Source: GitRepo the-same + Branch dev-team \**
          - Apply: Source above + Path `/staging` \***

> \* Here the Tenant and Tenant Owners are created.

> \** Here the Tenant configurations Source are referenced.

> \*** Here the Apply resources are declared and the impersonation configured.

## 2. Dev Team tenant Sync

### Source \**

This Repository (flux-capsule-multi-tenancy) + dev-team Branch

### Apply \***

> Tenant Owner reconciles this stuff via impersonation

#### Hierarchy

Staging configuration:
  - Namespaces
  - Aps
