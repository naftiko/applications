# Radius

**Philosophy**: Define runtime connections between application components and backing infrastructure.


## Radius
Radius is an open-source, cloud-native, application platform that enables developers and the operators that support them to define, deploy, and collaborate on cloud-native applications across public clouds and private infrastructure

### Operational Properties
I wanted to be able to understand how the Radius specification operates.

- [Website](https://radapp.io/)
- [Documentation](https://docs.radapp.io/)
- [CNCF Project](https://www.cncf.io/projects/radius/)
- [Getting Started](https://docs.radapp.io/quick-start/)
- [Concepts](https://docs.radapp.io/concepts/)
- [Tutorials](https://docs.radapp.io/tutorials/)
- [Guides](https://docs.radapp.io/guides/)
- [Recipes](https://github.com/radius-project/recipes)
- [Samples](https://github.com/radius-project/samples)
- [CLI](https://docs.radapp.io/guides/tooling/rad-cli/)
- [VSCode](https://docs.radapp.io/guides/tooling/vscode/)
- [Dashboard](https://docs.radapp.io/guides/tooling/dashboard/)
- [Press](https://docs.radapp.io/community/media-coverage/)
- [Blog](https://blog.radapp.io/posts)
- [Discord](https://discord.com/channels/1113519723347456110/1114312962929348668)
- [Community](https://github.com/radius-project/community)
- [Community Meetings](https://github.com/radius-project/community?tab=readme-ov-file#community-meetings)
- [Youtube](https://www.youtube.com/@radapp_io)
- [Threads](https://www.threads.com/@radapp_io)
- [GitHub Organization](https://github.com/radius-project)
- [GitHub Repository](https://github.com/radius-project/radius)
- [Schema](https://docs.radapp.io/reference/resource-schema/)
- [Configuration File](https://docs.radapp.io/reference/config/)
- [Radius API](https://docs.radapp.io/reference/api/)
- [Trademark](https://www.linuxfoundation.org/legal/trademark-usage)

### Specification Properties
I wanted to flatten and understand all the properties of the Radius specification.

Accesses, ACLs, Addresses, Aliases, Annotations, Apps, Applications, Args, Arns, Arrays, Auths, Authentications, Awses, Azures, Backends, Bases, Biceps, Blobs, Booleans, Brokers, Caches, Callbacks, Certificates, Clients, Commands, Components, Computes, Configs, Configurations, Connections, Containers, Contexts, Credentials, Ctys, Daprs, Datas, Databases, Defaults, Delays, Destinations, Digests, Directories, Directions, Disables, Disableds, Durations, Elements, Enables, Encodings, Environments, Environments, Ephemerals, Execs, Extenders, Extensions, Failures, Filesystems, Formats, Froms, Fullies, Gateways, Generics, Gets, Git, Graphs, Groups, Headers, Healths, Hosts, Hostnames, Https, Iams, IDs, Identities, Images, Initials, Injects, Instances, Integers, Internals, Irsas, Issuers, JSON, Jwts, Keys, Kinds, Kubernetes, Labels, Livenesses, Lists, Locals, Locations, Manageds, Managedidentities, Manuals, Maximums, Metadatas, Methods, Mins, Minimums, Mongos, Mqs, Mounts, Names, Namespaces, Nexts, Nons, Numbers, Objects, Ocis, Oidcs, Outputs, Pageds, Parameters, Passthroughs, Passwords, Pats, Paths, Periods, Permissions, Persistents, Plains, Planes, Pods, Podspecs, Policies, Ports, Prefixes, Principals, Probes, Properties, Protocols, Providers, Provisionings, Pubsubs, Pulls, Qualifieds, Queues, Rabbits, Rabbitmqs, Radiuses, Readinesses, Recipes, Redundants, Redises, Refs, References, Registries, Replicas, Replaces, Requests, Resources, Responses, Restarts, Results, Roles, Routes, Runtimes, Scalings, Schemas, Schemes, Secrets, Secretstores, Selectors, Servers, Services, Settings, Sidecars, Simulateds, Specs, Sqls, Ssls, States, Statuses, Storages, Stores, Strings, Subs, Summaries, Systems, Tags, Tcps, Templates, Tenants, Terraforms, Thresholds, Timeouts, Tlses, Types, Typs, Updates, Uris, Urls, Usernames, Values, Variables, Versions, Vhosts, Volumes, Websockets, Workings, Workloads

## Dependencies Evaluation

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