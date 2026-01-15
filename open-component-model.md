# Open Component Model (OCM)

**Philosophy**: Describe software components and their relationships for lifecycle management and compliance.

The Open Component Model (OCM) is an open standard that enables teams to describe software artifacts and their lifecycle metadata in a consistent, technology-agnostic way. It’s built to support secure, reliable delivery and deployment of software—across cloud, on-prem, hybrid, and even air-gapped environments.

## Operational Properties:
I wanted to be able to understand how the Open Component Model specification operates.

- [Website](https://ocm.software/)
- [Documentation](https://ocm.software/docs/)
- [Getting Started](https://ocm.software/docs/getting-started/)
- [Concepts](https://ocm.software/docs/concepts/)
- [Tutorials](https://ocm.software/docs/tutorials/)
- [GitHub Organization](https://github.com/open-component-model)
- [GitHub Repository](https://github.com/open-component-model/ocm-spec)
- [Schema](https://github.com/open-component-model/ocm-website/tree/main/static/schemas)
- [CLI](https://ocm.software/docs/reference/ocm-cli/)
- [Dev Version](https://ocm.software/dev/)
- [Default Version](https://ocm.software/)
- [Community](https://ocm.software/community/engagement/)
- [Slack](https://kubernetes.slack.com/archives/C05UWBE8R1D)
- [Community Meetings](https://ocm.software/community/engagement/#schedule)
- [Contributing](https://github.com/open-component-model/ocm-spec/blob/main/CONTRIBUTING.md)
- [License](https://github.com/open-component-model/ocm-spec/blob/main/LICENSE)
- [Road Map](https://github.com/orgs/open-component-model/projects/10/views/5)
- [Security](https://github.com/open-component-model/.github/blob/main/SECURITY.md)

## Specification Properties:
I wanted to flatten and understand all the properties of the Open Component Model specification.

Access, Algorithms, All, Artifacts, Arrays, Bases, Blobs, Booleans, Commits, Components, Configurations, Context, Creations, Defaults, Definitions, Digests, Elements, Extras, Files, File Systems, Generics, Githubs, Hashes, HTTP, Identities, Images, Inputs, Keys, Labels, Locals, Mappings, Media, Merges, Metas, Names, Nested, Nones, Normalisations, Nulls, Numbers, Objects, OCIs, Of, Providers, References, Relations, Relaxed, Repos, Repositories, Resources, Schemas, Selectors, Semvers, Signatures, Signings, Sizes, Sources, Specifcations, Sources, Strings, Times, Types, Unknowns, URLs, Values, Versions

## Dependencies Evaluation

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
