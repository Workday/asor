# Agent Analytics and Metrics Schema 

We will be providing a process for your organization to send operational data to Workday, allowing administrators to view all agent analytics unified in ASOR, across all agents, as well as per agent. More information about how to send the data to Workday will come soon. Once the custom agent analytics setup is complete, the operational analytics shown will be the same for Workday Built Agents and your organization's custom-built agents. As part of our initial release, we will be displaying operational usage related metrics, such as number of sessions, users, agent, and skill invocations.

Our logs schema and ingestion mechanisms will align the OpenTelemetry observability standard. Once the runtime data for your agents are sent to us, we will ingest, parse, aggregate, and visualize the metrics in your customer Workday tenant. The data that you send to our system should align with the following schema.

## Generic Properties

Common to all operations

| Property | Required | Type | Description | Examples |
|----------|----------|------|-------------|----------|
| timestamp | Yes | DateTime (UTC) | Timestamp when the operation started. Auto inserted. | 2025-07-21T10:32:53.6482311Z |
| id | Yes | string | Id of the trace line. Auto inserted. | ad8e60573927c2aa |
| operation_Id | Yes | string | Id of the operation. Auto inserted. | ca8b5315adf0385c4eced3b83ee20d5e |
| operation_ParentId | | string | Id of the parent trace line. Auto inserted. | 54e5e17086a16c9c |
| gen_ai.operation.name | | string | Name of the operation | invoke_agent, execute_tool |

## Invoke Agent

Represents an agent invoking another agent, often referred to as A2A

| Property | Required | Type | Description | Examples |
|----------|----------|------|-------------|----------|
| gen_ai.agent.id | Yes | string | Platform Id of the agent being invoked | 30ed5699-b157-4e87-bb45-9b0cfb13b8e5 |
| gen_ai.agent.name | Yes | string | Name of the target agent | GraphAgent, ScrumAssistant |
| gen_ai.agent.description | | string | Description of the target agent | Looks up Graph; Helps with scrum |
| gen_ai.agent.userid | | string | Platform user ID of the agent | 77679922-4623-4b74-81df-9cb111735e3c |
| server.address | | string | Endpoint of the target agent | app-web-aj5i4udhgrqgm.azurewebsites.net |
| server.port | | string | Port of the target agent | 80; 8080; 443; |
| gen_ai.input.messages | | string | Request from the agent call | Here is the org structure: ... |
| gen_ai.output.messages | | string | Response from the agent call | |
| error.type | | string | Describes a class of error the operation ended with. | timeout; java.net.UnknownHostException; server_certificate_invalid; 500 |
| error.message | Yes | string | Message containing human-readable detail about an error | Unexpected input type: string |
| gen_ai.caller.id | | string | ID of the agent/user that invokes the agent | 16679922-4623-4b74-81df-9cb111735e3c |
| gen_ai.caller.name | | string | Name of the agent/user that invokes the agent | Listening Framework; Retail Agent; agentuser01 |
| gen_ai.execution.type | | string | The type of agent invocation | EventToAgent, AgentToAgent, HumanToAgent |
| gen_ai.execution.sourceMetadata.id | | string | ID of the event that triggered agent invocation. | cd6e7d06-ca25-4b1b-ae0b-dea38c8d06dd |
| gen_ai.execution.sourceMetadata.name | | string | Name of the event that triggered agent invocation. | NewChatMessage |

## Execute Tool

Traces of tool calls, such as invoking a function.

| Property | Required | Type | Description | Examples |
|----------|----------|------|-------------|----------|
| gen_ai.agent.id | Yes | string | Platform Id of the agent performing the operation | 30ed5699-b157-4e87-bb45-9b0cfb13b8e5 |
| gen_ai.agent.name | Yes | string | Name of the agent | GraphAgent, ScrumAssistant |
| gen_ai.agent.description | | string | Description of the agent | Looks up Graph; Helps with scrum |
| gen_ai.agent.userid | | string | Platform user ID of the agent | 77679922-4623-4b74-81df-9cb111735e3c |
| gen_ai.tool.call.id | | string | Id of the tool call | call_xmS8WyT3sBrSVbX1uedGw2CA |
| gen_ai.tool.name | | string | Name of the tool | get-current-user-email |
| gen_ai.tool.description | | string | Description of the tool | get current user email address |
| gen_ai.tool.call.arguments | | string | Arguments of the tool call | {'name':'input',data:{'value':'x','type':'string'}} |
| gen_ai.tool.call.result | | string | Response from the tool execution | {'value':'user@somedomain.com','type':'string'} |
| gen_ai.tool.type | | string | Type of the tool utilized | function; extension; datastore |
| error.type | | string | Class of error the tool call ended with | java.net.UnknownHostException |
| error.message | Yes | string | Message containing human-readable detail about an error | Unexpected input type: string |

## Inference Calls

Traces of inference calls to AI models, such as OpenAI, Azure OpenAI, or other LLMs.

| Property | Required | Type | Description | Examples |
|----------|----------|------|-------------|----------|
| gen_ai.agent.id | Yes | string | Platform Id of the agent performing the operation | 30ed5699-b157-4e87-bb45-9b0cfb13b8e5 |
| gen_ai.agent.name | Yes | string | Name of the agent | GraphAgent, ScrumAssistant |
| gen_ai.agent.description | | string | Description of the agent | Looks up Graph; Helps with scrum |
| gen_ai.agent.userid | Yes | string | Platform user ID of the agent | 77679922-4623-4b74-81df-9cb111735e3c |
| gen_ai.provider.name | | string | AI System used | openai |
| gen_ai.request.model | | string | Model used for inference | gpt-4o |
| gen_ai.usage.input_tokens | | int | | 133 |
| gen_ai.usage.output_tokens | | int | | 24 |
| gen_ai.response.finish_reasons | | string[] | | [ToolCalls] |
| gen_ai.input.messages | | string | | |
| gen_ai.output.messages | | string | | |
| [gen_ai.tool.definitions](https://opentelemetry.io/docs/specs/semconv/registry/attributes/gen-ai/) | | | | |
