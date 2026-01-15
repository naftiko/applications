# Dependency Definition Comparison: ORD, Radius, Score, CNAB, and OCM

## Overview Table

| Aspect | ORD | Radius | Score | CNAB | OCM |
|--------|-----|--------|-------|------|-----|
| **Primary Use Case** | API/Service discovery & catalog | Cloud-native app deployment | Platform-agnostic workload spec | Application packaging & installation | Software component lifecycle |
| **Dependency Granularity** | API/Event level | Resource connections | Abstract resource types | Image/invocation level | Component references |
| **Version Constraints** | `minVersion` on aspects | None explicit | None | SemVer ranges | Exact versions or ranges |
| **Shared Dependencies** | Via `correlationIds`, `partOfGroups` | Via resource IDs | Via matching `type`+`class`+`id` | Via parameter injection | Via component references |
| **Direction Modeling** | Explicit (`inbound`/`outbound`) | Implicit via `connections` | Implicit via placeholders | Implicit via credentials | Explicit source→target |

---

## 1. ORD (Open Resource Discovery)

**Philosophy**: Describe integration requirements at the API/Event contract level for discovery and catalog purposes.

**Key Structures**:

```yaml
integrationDependencies:
  - ordId: "sap.foo:integrationDependency:CustomerOrder:v1"
    mandatory: true
    aspects:
      - title: "Get Customer Data"
        mandatory: true
        supportMultipleProviders: false
        apiResources:
          - ordId: "sap.s4:apiResource:API_BUSINESS_PARTNER:v1"
            minVersion: "1.2.0"
        eventResources:
          - ordId: "sap.s4:eventResource:BusinessPartnerEvents:v1"
            minVersion: "1.0.0"
            subset:
              - eventType: "sap.s4.BusinessPartner.Changed.v1"
```

**Characteristics**:

- **Structured integration model**: Dependencies grouped into `IntegrationDependency` → `Aspects` → `ApiResources`/`EventResources`
- **AND/OR logic**: Aspects are AND conditions; multiple resources within an aspect are OR alternatives
- **Minimum versions**: `minVersion` specifies compatibility requirements
- **Event subsets**: Can specify only specific event types needed
- **System type restrictions**: Can limit which system types can fulfill the dependency
- **Bidirectional awareness**: Resources have explicit `direction` (inbound/outbound/mixed)
- **Rich metadata**: Includes visibility, release status, deprecation info

**Strengths**: Excellent for API catalogs, service discovery, and documenting complex enterprise integrations.

---

## 2. Radius

**Philosophy**: Define runtime connections between application components and backing infrastructure.

**Key Structures**:

```yaml
# ContainerResource
properties:
  application: "/planes/radius/.../applications/myapp"
  environment: "/planes/radius/.../environments/prod"
  container:
    image: "myapp:latest"
  connections:
    database:
      source: "/planes/radius/.../mongoDatabases/mydb"
      iam:
        kind: azure
        roles: ["Reader", "Writer"]
    cache:
      source: "/planes/radius/.../redisCaches/mycache"
      disableDefaultEnvVars: false
```

**Application Graph**:

```yaml
# ApplicationGraphConnection
- id: "/planes/radius/.../mongoDatabases/mydb"
  direction: "Outbound"  # This resource points to the target
```

**Characteristics**:

- **Named connections**: Dependencies are named maps allowing semantic meaning
- **Full resource IDs**: Uses complete resource paths (UCP format)
- **IAM integration**: Can specify roles/permissions for the connection
- **Environment variable injection**: Automatic env var population from connections
- **Hierarchical linking**: Resources linked to `environment` and `application`
- **Recipe-based provisioning**: Dependencies can be auto-provisioned via recipes
- **Graph visualization**: `ApplicationGraphConnection` supports dependency visualization

**Strengths**: Excellent for cloud-native deployments with built-in IAM and infrastructure provisioning.

---

## 3. Score

**Philosophy**: Platform-agnostic workload specification with abstract resource dependencies.

**Key Structures**:

```yaml
apiVersion: score.dev/v1b1
metadata:
  name: my-workload

containers:
  main:
    image: myapp:latest
    variables:
      DB_HOST: "${resources.database.host}"
      DB_PORT: "${resources.database.port}"
      DB_PASSWORD: "${resources.database.password}"
    volumes:
      /data:
        source: "${resources.storage}"

resources:
  database:
    type: postgres
    class: large
    id: shared-database  # Enables sharing across workloads
    params:
      extensions:
        - postgis
  storage:
    type: volume
  cache:
    type: redis
    class: default
```

**Characteristics**:

- **Type-based abstraction**: Dependencies defined by `type` (not specific implementations)
- **Placeholder resolution**: `${resources.name.property}` syntax resolved at deployment
- **Class specialization**: Optional `class` for variants (e.g., `large`, `small`)
- **Shared resources**: Same `type`+`class`+`id` = same resource across workloads
- **Provisioning parameters**: `params` passed to the Score implementation
- **No version constraints**: Platform implementation decides versions
- **Implementation-agnostic**: Works with score-compose, score-k8s, score-humanitec

**Strengths**: Maximum portability across platforms; simple, developer-friendly syntax.

---

## 4. CNAB (Cloud Native Application Bundle)

**Philosophy**: Package applications with their dependencies for reliable installation across environments.

**Key Structures**:

```json
{
  "schemaVersion": "1.0.0",
  "name": "my-application",
  "version": "1.0.0",
  "invocationImages": [
    {
      "imageType": "docker",
      "image": "myregistry/installer:1.0.0"
    }
  ],
  "images": {
    "backend": {
      "image": "myregistry/backend:2.1.0",
      "imageType": "docker"
    },
    "frontend": {
      "image": "myregistry/frontend:1.5.0",
      "imageType": "docker"
    }
  },
  "dependencies": {
    "requires": {
      "database-bundle": {
        "bundle": "myregistry/postgres-bundle:1.x",
        "version": ">=1.2.0 <2.0.0"
      },
      "messaging-bundle": {
        "bundle": "myregistry/rabbitmq-bundle",
        "version": "^3.8.0"
      }
    }
  },
  "credentials": {
    "db_password": {
      "env": "DB_PASSWORD",
      "required": true
    }
  },
  "parameters": {
    "database_host": {
      "type": "string",
      "destination": {
        "env": "DATABASE_HOST"
      }
    }
  }
}
```

**Characteristics**:

- **Bundle-to-bundle dependencies**: References other CNAB bundles
- **SemVer ranges**: Full semantic versioning with range operators (`>=`, `^`, `~`, etc.)
- **Image inventory**: Lists all container images in the bundle
- **Credential requirements**: Declares what credentials the bundle needs
- **Parameter passing**: Outputs from dependencies can become inputs
- **Invocation images**: Installer logic packaged as a container
- **Thick bundles**: Can include all images for air-gapped installation

**Strengths**: Reliable installation in enterprise environments; supports air-gapped deployments.

---

## 5. OCM (Open Component Model)

**Philosophy**: Describe software components and their relationships for lifecycle management and compliance.

**Key Structures**:

```yaml
apiVersion: ocm.software/v3alpha1
kind: ComponentVersion
metadata:
  name: acme.org/myapplication
  version: 1.0.0
  provider:
    name: acme.org
spec:
  resources:
    - name: backend-image
      type: ociImage
      version: 2.1.0
      access:
        type: ociRegistry
        imageReference: ghcr.io/acme/backend:2.1.0
    - name: helm-chart
      type: helmChart
      version: 1.0.0
      access:
        type: ociRegistry
        imageReference: ghcr.io/acme/chart:1.0.0
  references:
    - name: postgres-component
      componentName: acme.org/postgres
      version: "14.5.0"
    - name: redis-component  
      componentName: acme.org/redis
      version: ">=7.0.0"
  sources:
    - name: source-code
      type: git
      access:
        type: github
        repository: acme/myapplication
        ref: refs/tags/v1.0.0
```

**Characteristics**:

- **Component references**: Dependencies are references to other OCM components
- **Semantic versioning**: Exact versions or version constraints
- **Resource aggregation**: Components bundle multiple resources (images, charts, configs)
- **Source traceability**: Links to source code for compliance/auditing
- **Signing support**: Components can be signed for verification
- **Transport agnostic**: Components can be moved between registries
- **Localization**: References can be resolved to location-specific artifacts

**Strengths**: Software supply chain management, compliance, and multi-environment distribution.

---

## Detailed Comparison

### Dependency Declaration Style

| Schema | Style | Example |
|--------|-------|---------|
| **ORD** | Structured objects with explicit metadata | `aspects[].apiResources[].ordId` |
| **Radius** | Named connection map with resource URIs | `connections.database.source` |
| **Score** | Type-based abstract resources | `resources.database.type: postgres` |
| **CNAB** | Bundle references with version ranges | `dependencies.requires.db-bundle.version: ">=1.2.0"` |
| **OCM** | Component references with versions | `references[].componentName` + `version` |

### Version Constraint Support

| Schema | Exact Version | Min Version | Version Ranges | No Version |
|--------|--------------|-------------|----------------|------------|
| **ORD** | ❌ | ✅ `minVersion` | ❌ | ✅ (optional) |
| **Radius** | ❌ | ❌ | ❌ | ✅ (implicit) |
| **Score** | ❌ | ❌ | ❌ | ✅ (always) |
| **CNAB** | ✅ | ✅ | ✅ Full SemVer | ❌ |
| **OCM** | ✅ | ✅ | ✅ Constraints | ❌ |

### Dependency Resolution

| Schema | Resolution Time | Resolution Method |
|--------|-----------------|-------------------|
| **ORD** | Discovery/Design time | Aggregator matches by ORD ID |
| **Radius** | Deployment time | Resource ID lookup or Recipe provisioning |
| **Score** | Deployment time | Score implementation provisions by type |
| **CNAB** | Installation time | Bundle resolver fetches dependent bundles |
| **OCM** | Build/Deploy time | Component descriptor resolution |

### Sharing Dependencies Across Workloads

```yaml
# ORD - Via correlation IDs or groups
partOfGroups:
  - "sap.foo:domain:sap.foo:Sales"
correlationIds:
  - "sap.s4:csnService:BusinessPartner"

# Radius - Via same resource ID
connections:
  shared-db:
    source: "/planes/radius/.../mongoDatabases/shared-instance"

# Score - Via matching type+class+id
resources:
  database:
    type: postgres
    class: large
    id: shared-database  # Same id = same resource

# CNAB - Via parameter/credential passing
parameters:
  database_host:
    type: string
    source:
      dependency: database-bundle
      output: host

# OCM - Via same component reference
references:
  - componentName: acme.org/shared-postgres
    version: "14.5.0"
```

### Optional vs Required Dependencies

| Schema | How to Express |
|--------|----------------|
| **ORD** | `mandatory: true/false` on IntegrationDependency and Aspect |
| **Radius** | All connections are implicitly required |
| **Score** | All resources are implicitly required if referenced |
| **CNAB** | Separate `requires` vs `provides` in dependencies |
| **OCM** | All references are required; optional via labels/annotations |

---

## Summary: When to Use Each

| Schema | Best For |
|--------|----------|
| **ORD** | Enterprise API catalogs, service discovery, integration documentation |
| **Radius** | Cloud-native app deployment with infrastructure provisioning |
| **Score** | Developer-friendly, platform-agnostic workload definitions |
| **CNAB** | Packaged application distribution, air-gapped enterprise deployments |
| **OCM** | Software supply chain, compliance, multi-environment component management |

### Complementary Usage

These schemas can work together:

- **Score** workloads deployed via **Radius** infrastructure
- **OCM** components containing **CNAB** bundles
- **ORD** documenting APIs exposed by applications deployed via any of the others
- **CNAB** bundles installing **Radius** environments

---

## Quick Reference: Dependency Properties by Schema

### ORD

| Property | Location | Purpose |
|----------|----------|---------|
| `integrationDependencies` | Document root | Top-level dependency declarations |
| `aspects` | IntegrationDependency | Constituent parts (AND logic) |
| `apiResources` | Aspect | API dependencies (OR alternatives) |
| `eventResources` | Aspect | Event dependencies (OR alternatives) |
| `ordId` | Resource reference | Target resource identifier |
| `minVersion` | Resource reference | Minimum compatible version |
| `mandatory` | IntegrationDependency, Aspect | Required vs optional |
| `supportMultipleProviders` | Aspect | Multi-source support |

### Radius

| Property | Location | Purpose |
|----------|----------|---------|
| `connections` | ContainerProperties | Named dependency map |
| `source` | ConnectionProperties | Target resource ID |
| `iam` | ConnectionProperties | Permission configuration |
| `environment` | Various resources | Environment linkage |
| `application` | Various resources | Application linkage |
| `resources` | Various resources | Associated resource references |

### Score

| Property | Location | Purpose |
|----------|----------|---------|
| `resources` | Workload root | Resource dependency declarations |
| `type` | Resource | Resource type (postgres, redis, etc.) |
| `class` | Resource | Type specialization |
| `id` | Resource | Shared resource identifier |
| `params` | Resource | Provisioning parameters |
| `${resources.X.Y}` | Variables, volumes | Placeholder references |

### CNAB

| Property | Location | Purpose |
|----------|----------|---------|
| `dependencies.requires` | Bundle root | Required bundle dependencies |
| `bundle` | Dependency | Bundle reference |
| `version` | Dependency | SemVer constraint |
| `images` | Bundle root | Container image inventory |
| `credentials` | Bundle root | Required credential declarations |
| `parameters` | Bundle root | Input/output parameters |

### OCM

| Property | Location | Purpose |
|----------|----------|---------|
| `references` | ComponentVersion spec | Component dependencies |
| `componentName` | Reference | Target component identifier |
| `version` | Reference | Version or constraint |
| `resources` | ComponentVersion spec | Bundled artifacts |
| `sources` | ComponentVersion spec | Source code references |