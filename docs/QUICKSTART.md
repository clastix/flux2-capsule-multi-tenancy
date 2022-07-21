# Quickstart

## Flux

### The process

#### 1. Bootstrap

1.1. Install flux controller on the specified cluster (--context arg value)

1.2. Flux controller pushes basic Flux configuration to the specified repository and path (where the Infra (Flux system itself included) and platform (i.e. tenants) desired state is declared):

- Flux GitOps ToolKit operators and related RBAC.
- Sync (`Source` and `Kustomiaztion`) for the GOTK resources themselves (previous point).

	Example commits:
	
	```
	Add Flux sync manifests
	Flux committed 1 hour ago
	
	Add Flux v0.30.2 component manifests
	Flux committed 1 hour ago 
	```

> From now on, it no longer needs to write to the repository. The PAT can be revoked and will be discared.

1.3. Flux controller generate and pushes a deploy key (r/o access) for the gitops Platform admin repository, to reconcile on the cluster the desired state (on Git) with the actual state (on K8s)

1.4. Flux controller reconciles actual state on linked cluster (--context arg value) based on what it reads above ^^^

#### 2. Updates

2.1. Push to repo

2.2. Flux reconciles based on Kustomizations which reference Source (e.g. GitRepository)

### The desired state configuration hierarchy

#### 1. Platform admin repo/branch (Source)

1.1. Cluster configuration with Flux System

1.2. (optional) Cluster configuration with infra (e.g. policies)

1.3. Cluster configuration with Tenants (i.e. dev-team) (the sync source repo/branch + kustomization)

- Tenant Sync's RBAC configuration should configured with least privilege (service account that the kustomization controller should **impersonate**)

- Tenant Sync's Kustomization configuration should make the Kustomize controller impersonate the service account above ^^^ for least privilege principle for soft MULTI-TENANCY 

- Tenant Sync's Source configuration should refer to the tenant-dedicated repo/branch Source

#### 2. Tenant (e.g. dev-team) repo/branch (Source)

2.1. kustomization on the sync source repo/branch specified by the Platform admin (i.e. same repo, dev-team branch)
