# llm-d Components

This document defines how llm-d organizes its components, what it means for a
component to be part of the project, and how a component moves toward and into
the project. It complements [PROJECT.md](./PROJECT.md), which defines the
project's mission, principles, and process.

## Two questions, two axes

A component's status answers two independent questions.

1. **Is the project committed to it?** Reflected by where it lives.
   - **Incubation** (the [`llm-d-incubation`](https://github.com/llm-d-incubation)
     org): early or experimental work the project has not yet committed to
     maintain. Opt-in and isolated (principle #3). No stability promise.
   - **Graduated** (the [`llm-d`](https://github.com/llm-d) org): the project has
     committed to maintain it. Graduation is a deliberate step, not a default.

2. **Is it on the inference request path?** Reflected by a role label applied to
   graduated components.
   - **Core**: sits inline in serving a live inference request.
   - **Ecosystem**: a committed, maintained component that plans, measures,
     operates, or extends llm-d, but is not required inline to serve a request.

These axes are orthogonal. A component graduates from Incubation into the main
org and is labeled **Core** or **Ecosystem** at that time. A component may later
be promoted from Ecosystem to Core if it moves onto the request path and clears
the higher bar.

## The critical-path test

A component is **Core** if a well-lit path cannot serve inference requests
without it, or if it sits inline in the request or data path at runtime, such
that its failure or regression directly degrades or breaks live inference.

A component is **Ecosystem** if it enhances, plans, measures, or operates llm-d
but is not required inline to serve a request. Design-time and deploy-time
tools, benchmarking and analysis, simulators, and adjacent services are
Ecosystem.

When a component is genuinely on the boundary, for example an opt-in service
that feeds live scheduling decisions, the project maintainers decide its role
and record the rationale in the roster below.

## Graduation criteria

Graduation is a commitment by the project to maintain a component. The bar
depends on the role the component will hold, because a Core component carries
the reliability expectations of the request path (principle #5) while an
Ecosystem component does not.

### Every graduated component (Core or Ecosystem)

- An active owning team named in `OWNERS`, committed to maintenance.
- A README and user-facing docs; a guide or well-lit path where applicable.
- Working CI, tests, and standard project linting and PR sign-off.
- A real use case or a clear strategic gap it fills.
- A sponsoring project maintainer.

### Additional bar for Core

- **Bus factor greater than one.** At least two active maintainers, preferably
  across more than one organization. The request path must not depend on a
  single person.
- **Release discipline.** Ships on the llm-d release train with semantic
  versioning; no breaking changes to published APIs (principle #7).
- **Reliability and security bar.** Meaningful test coverage, a security policy
  and contacts, and a high review bar consistent with production code.

An Ecosystem component with a small team, including a bus factor of one, is
acceptable precisely because it is off the request path. The same component
would not qualify as Core until it meets the Core bar.

## Process

- **Proposal.** A component's maintainers open a PR proposing graduation, naming
  the target role (Core or Ecosystem) and showing how the component meets the
  relevant criteria.
- **Decision.** Project maintainers approve by
  [lazy consensus](https://community.apache.org/committers/decisionMaking.html#lazy-consensus),
  with explicit sign-off given the significance. Disagreement is resolved per
  [PROJECT.md](./PROJECT.md#process).
- **Promotion (Ecosystem to Core).** Same process, evaluated against the Core
  bar.
- **Review.** The roster is reviewed at least annually. A component that no
  longer meets its bar may be moved to a lower state or archived. Demotion and
  archival are routine hygiene, not a judgment of the people involved.

## Representation

- Graduated components live in the [`llm-d`](https://github.com/llm-d) org;
  incubation components in
  [`llm-d-incubation`](https://github.com/llm-d-incubation).
- Role is recorded with the `core` or `ecosystem` GitHub repo topic and in the
  roster below.
- The main [README](./README.md) groups components by role so users can see at a
  glance what is on their request path.

## Component roster

This roster is the source of truth for component status and is updated by PR
under the process above.

### Core

| Component | Purpose |
|-----------|---------|
| `llm-d-router` | Intelligent request routing and endpoint picking (EPP / gateway) |
| `llm-d-kv-cache` | Distributed KV cache scheduling and offloading |
| `llm-d-routing-sidecar` | Prefill/decode routing sidecar |
| `llm-d-async` | Asynchronous processor and queue orchestration for the inference gateway |

### Ecosystem

| Component | Purpose |
|-----------|---------|
| `llm-d-planner` | Design and deploy-time capacity planning and configuration |
| `llm-d-benchmark` | Benchmarking framework and tooling |
| `llm-d-inference-sim` | GPU-free vLLM simulator |
| `llm-d-prism` | Performance analysis for distributed inference |
| `llm-d-inference-cost` | Inference cost analysis |
| `hermes` | Cluster configuration scanning and self-test generation |
| `llm-d-semantic-classifier` | Request semantic classification |

### Boundary cases

Role recorded with rationale; to be confirmed by the project maintainers.

| Component | Proposed role | Rationale |
|-----------|---------------|-----------|
| `llm-d-latency-predictor` | Core or Ecosystem (decide) | Feeds live scheduling scores but is opt-in |
| Autoscaler (KEDA + EPP path) | Ecosystem (decide) | Control loop affecting live capacity, not inline in a single request |
