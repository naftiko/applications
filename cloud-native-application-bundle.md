# Cloud Native Application Bundle (CNAB)

Cloud Native Application Bundles (CNAB) are a package format specification that describes a technology for bundling, installing, and managing distributed applications, that are by design, cloud agnostic.

**Philosophy**: Package applications with their dependencies for reliable installation across environments.

## Operational Properties:
I wanted to be able to understand how the Cloud Native Application Bundle specification operates.

- [Website](https://cnab.io/)
- [GitHub Organization](https://github.com/cnabio/)
- [GitHub Repo](https://github.com/cnabio/cnab-spec)
- [Schema](https://cnab.io/schema/cnab-core-1.0/bundle.schema.json)
- [License](https://github.com/cnabio/cnab-spec/blob/main/LICENSE)
- [Contributing](https://github.com/cnabio/cnab-spec/blob/main/CONTRIBUTING.md)
- [Meetings](https://cnab.io/community-meetings/)
- [Meeting Notes](https://hackmd.io/s/SyGcBcwQ4#)
- [Slack Channel](https://github.com/cnabio/cnab-spec?tab=readme-ov-file#meetings)
- [Mailing List](https://lists.jointdevelopment.org/g/CNAB-Main)
- [Tools](https://cnab.io/community-projects/)
- [Community](https://github.com/cnabio/community)
- [Resources](https://cnab.io/resources/)
- [Governance](https://github.com/cnabio/community/blob/main/governance.md)
- [Registries](https://cnab.io/registries/)

## Specification Properties:
I wanted to flatten and understand all the properties of the Cloud Native Application Bundle specification.

Actions, Additionals, All, ANIs, Applies, Arrays, Booleans, Bundles, Claims, Comments, Components, Constants, Contains, Contents, Created, Credentials, Customs, Defaults, Definitions, Dependencies, Descriptions, Destinations, Digests, Elses, Emails, Encodings, Enums, Environments, Examples, Exclusives, Extensions, Formats, IDs, If, Images, Installations, Integers, Invocations, Items, Keywords, Labels, Lengths, Licenses, Maintainers, Mappings, Maximums, Medias, Messages, Metas, Mins, Minimums, Multiples, Names, Negatives, Nons, Nots, Objects, Ofs, Ones, Outputs, Parameters, Paths, Patterns, Prereleases, Priorities, Properties, Ranges, Reads, References, Relocations, Requireds, Requires, Results, Revisions, Schemas, Sequences, Simples, Sizes, Sources, Statuses, Strings, Thens, Titles, Tos, Types, Uniques, Unknowns, Urls, Versions

## Dependencies Evaluation

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
