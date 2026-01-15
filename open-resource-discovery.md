# Open Resource Discovery (ORD)

**Philosophy**: Describe integration requirements at the API/Event contract level for discovery and catalog purposes.

Open Resource Discovery (ORD) is a protocol for application and service metadata publishing and discovery. It serves as a foundation for metadata catalogs and marketplaces while improving integration automation and quality.

## Operational Properties
I wanted to be able to understand how the Open Resource Discovery specification operates.

- [Website](https://open-resource-discovery.org/)
- [Schema](https://open-resource-discovery.github.io/specification/spec-v1/interfaces/Document.schema.json)
- [OpenAPI](https://open-resource-discovery.github.io/specification/spec-v1/interfaces/DocumentAPI.oas3.yaml)
- [Extensions](https://open-resource-discovery.org/spec-extensions)
- [Use Cases](https://open-resource-discovery.org/overview#use-cases)
- [Policy Levels](https://open-resource-discovery.org/spec-extensions/policy-levels)
- [Examples](https://open-resource-discovery.org/spec-v1/examples)
- [Ecosystem](https://open-resource-discovery.org/ecosystem)
- [Help](https://open-resource-discovery.org/help)
- [Videos](https://open-resource-discovery.org/help/videos)
- [FAQ](https://open-resource-discovery.org/help/faq)
- [Logos](https://open-resource-discovery.org/help/logos)
- [Change Log](https://github.com/open-resource-discovery/specification/blob/main/CHANGELOG.md)
- [Code of Conduct](https://github.com/open-resource-discovery/specification/blob/main/CODE_OF_CONDUCT.md)
- [Contributing](https://github.com/open-resource-discovery/specification/blob/main/CONTRIBUTING.md)
- [Contributing Using Gen AI](https://github.com/open-resource-discovery/specification/blob/main/CONTRIBUTING_USING_GENAI.md)
- [License](https://github.com/open-resource-discovery/specification/blob/main/LICENSE)
- [Steering Committee](https://github.com/open-resource-discovery/steering#readme)

## Specification Properties
I wanted to flatten and understand all the properties of the Open Resource Discovery specification.

Access, APIs, Aspects, Awares, Bases, Booleans, Bundles, Callbacks, Capabilities, Categories, Changelogs, Compatibles, Consumptions, Correlations, Countries, Credentials, Customs, Datas, Dates, Defaults, Definitions, Dependencies, Deprecations, Described, Descriptions, Directions, Disableds, Discoveries, Documentations, Entities, Entries, Events, Exchanges, Exposeds, Extensibles, Groups, IDs, Implementations, Industries, Infos, Inputs, Instances, Integrations, JSON, Labels, Lasts, Levels, Lifecycles, Lines, Links, Locals, Mandatories, Mappings, Media, Mins, Multiples, Namespaces, Objects, OData, ORDs, Outputs, Packages, Parents, Partners, Parts, Perspectives, Pointers, Policies, Ports, Products, Protocols, Providers, References, Relateds, Relations, Releases, Removals, Resources, Responsibles, Restrictions, Runtimes, Schemas, Selectors, Sets, Shorts, Standards, Statuses, Strategies, Strings, Subsets, Successors, Sunsets, Supports, Supporteds, Systems, Tags, Targets, Titles, Tombstones, Types, Updates, URLs, Usages, Use Cases, Vendors, Versions, Visibilities

## Dependencies Evaluation

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
