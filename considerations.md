## Agent developer considerations for Workday APIs

Use this guidance when creating an Agent that interacts with Workday APIs. The goal is to make your Agent understandable to consumers, compatible with A2A discovery, and properly registered with the required Workday identifiers.

### Describe your Agent clearly

Provide a concise, consumer-focused description so users can quickly decide if the Agent is relevant. Include:

- Purpose: what the Agent does and the outcomes it delivers in the name and description fields.
- Scope: what systems and Workday domains it can access or operate within. 
- Inputs and outputs: key data it requires and what it returns or changes.

Keep the description short but specific.

### Tag the Agent for A2A

Tagging improves discoverability and interoperability for A2A consumers. Add tags that:

- Reflect Workday domains or business areas (for example, HCM, Finance, Time Tracking).
- Indicate major capabilities (for example, create, update, report, approval).
- Communicate risk or sensitivity (for example, read-only, PII, SOX).

Use consistent, lowercase tags and avoid creating one-off variants.

### Gather Workday identifiers before registration

If your Agent uses Workday APIs, you must populate the Agent Resource section of the Registration API (see `versions/1.2.md`). Collect the required IDs up front to avoid registration delays.

The workdayConfig section contains a lot of detail, and for you to register the usage of a Workday API you must have a section using the same Skill ID that you put in the skills section. Within the workdayConfig, you'll put one or more "workdayResource". This Workday Resource includes a name and description, which as indicated above should describe WHY you are using this particular API in your Agentic context. The "agentResource" needs to be the WID (Workday ID) of the API you want to use. Future versions will attempt pattern matching literal endpoints, but for clarity and unambiguous assignment you need the Workday ID.

Do this before registering:

- Use the Workday API Explorer to find the specific APIs, endpoints, and resource identifiers you will reference.
- Read the Workday admin guide for your tenant to understand where those IDs are defined and how to retrieve them.
- Verify each ID in the target tenant or environment to ensure it matches the intended integration scope.

If you cannot confidently source an ID from the API Explorer or admin guide, do not guess. Resolve it with your Workday admin or integration owner first.
