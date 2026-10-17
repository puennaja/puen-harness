# Puen Harness

Puen Harness coordinates existing coding agents and bounded local text execution. Its durable task requirements remain independent of any provider's native conversation.

## Language

**Task**:
A durable user objective with approved scope, constraints and acceptance criteria, bound to one provider/model route once execution begins.
_Avoid_: Using session or Run to mean the complete objective.

**Run**:
One explicitly approved provider execution for a Task, bound to a context revision, route, billing profile, policy and permission envelope; native internal tool/model calls belong to that Run.
_Avoid_: Using Run for a single tool call or an entire multi-Run Task.

**Native Session**:
A provider-owned conversation and internal working state. It is neither the Task's requirements authority nor a portable conversation format.
_Avoid_: Portable session.

**Task Context**:
The canonical, versioned requirements and explicit source/evidence references of a Task. Portable Task Context v0 has one JSON authority and a generated Markdown view.
_Avoid_: Treating task pack and context pack as separate authorities.

**Route**:
The chosen provider and concrete model together with the endpoint, authentication and billing configuration under which they may execute.
_Avoid_: Model alone when the provider or billing destination matters.

**Conformance Profile**:
An exact provider/interface/version, OS/architecture, authentication/billing and effective configuration combination with evidence for its declared capabilities.
_Avoid_: Assuming adapter-wide support from one tested configuration.

**Eligible Route**:
A qualified route that also satisfies the current Task's data, capability, context, permission and admission constraints.
_Avoid_: Using installed, authenticated or discoverable as synonyms for eligible.

**Run Approval**:
The user's authorization of one previewed Run and its fixed context, route, limits, persistence and permission envelope.
_Avoid_: Treating project trust or an earlier Run approval as continuing authorization.

**Project Data Policy**:
A user-approved authorization for the data destinations and locality of the whole workspace accessible to an agent.
_Avoid_: Treating initial file selection as protection for other accessible files.

**Local-only**:
A constraint requiring inference and authorized processing to stay on the operator's machine.
_Avoid_: Equating loopback, local proxy or Ollama branding with proven local processing.

**Reported Model**:
The model identity a provider reports for an execution, recorded separately from the requested identity.
_Avoid_: Claiming verified backend identity solely from a requested model name.

**Task Acceptance**:
The user's recorded determination that the Task meets its acceptance criteria. It is distinct from provider completion and check results.
_Avoid_: Verified success based only on agent self-report.
