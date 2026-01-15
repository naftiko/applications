# Score

**Philosophy**: Platform-agnostic workload specification with abstract resource dependencies.


## Score
Score is a developer-centric and platform-agnostic workload specification. It ensures consistent configuration between local and remote environments.

### Operational Properties
I wanted to be able to understand how the Score specification operates.

- [CNCF Project](https://www.cncf.io/projects/score/)
- [Website](https://score.dev/)
- [Blog](https://score.dev/blog/)
- [Getting Started](https://score.dev/get-started/)
- [Community](https://docs.score.dev/docs/community/)
- [Linting](https://docs.score.dev/docs/score-specification/ide-linter-autocomplete/)
- [Specification](https://docs.score.dev/docs/score-specification/score-spec-reference/)
- [Implementations](https://docs.score.dev/docs/score-implementation/)
- [Examples](https://docs.score.dev/docs/examples/)
- [Guides](https://docs.score.dev/docs/how-to/)
- [Community Meetings](https://github.com/score-spec/spec?tab=readme-ov-file#-get-in-touch)
- [Slack Channel](https://cloud-native.slack.com/archives/C07DN0D1UCW)
- [GitHub Organization](https://github.com/score-spec)
- [GitHub repository](https://github.com/score-spec/spec)
- [Adopters](https://github.com/score-spec/spec/blob/main/ADOPTERS.md)
- [Governance](https://github.com/score-spec/spec/blob/main/GOVERNANCE.md)
- [Contributing](https://github.com/score-spec/spec/blob/main/CONTRIBUTING.md)
- [License](https://github.com/score-spec/spec/blob/main/LICENSE)
- [Road Map](https://github.com/score-spec/spec/blob/main/roadmap.md)
- [JSON Schema](https://github.com/score-spec/spec/blob/main/score-v1b1.json)
- [Platform Engineering](https://platformengineering.org/tools/score)

### Specification Properties
I wanted to flatten and understand all the properties of the Score specification.

Annotations, Apis, Args, Arrays, Binaries, Booleans, Classes, Commands, Containers, Contents, Cpus, Execs, Expands, Files, Gets, Headers, Hosts, Https, IDs, Images, Integers, Limits, Livenesses, Memories, Metadatas, Modes, Names, Nos, Objects, Ofs, Ones, Params, Paths, Ports, Probes, Protocols, Readinesses, Reads, Onlies, Refs, Requests, Resources, Schemes, Services, Sources, Strings, Targets, Types, Unknowns, Values, Variables, Versions, Volumes

## Dependencies Evaluation

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
