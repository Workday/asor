# Agent Analytics Data Schema

Deploying AI agents across your organization is only the first step; understanding their true impact is critical. To maximize your investment, you need actionable insights into agent performance and usage. Key questions organizations must answer include:

* **Usage & Context:** Who is using the agents, how are they being used, and in what context?  
* **Productivity & ROI:** Are the agents genuinely improving organizational productivity?  
* **Employee Experience:** Are they driving positive outcomes for your workforce?  
* **Cost Management:** What is the total cost of operating these agents?

Workday agent analytics is designed to help you answer these questions by providing comprehensive analytics and visualization capabilities.

### A Unified, Standardized Approach

To ensure a seamless analytics experience—whether your agents are built by Workday, our partners, or your internal teams—we have established a unified underlying data schema. Because we want this schema to be as frictionless as possible for developers and providers to adopt, it is built directly on the **OpenTelemetry (OTel)** observability standard.

### Effortless Integration

When analyzing agents built outside of Workday, all you need to do is configure your agents to emit traces using our specified OTel format. Workday handles the rest of the data pipeline:

1. **Ingestion & Transformation:** We ingest your raw traces and transform them into a processed format optimized for analytics.  
2. **Unified Visualization:** We map this data to our pre-built metrics and charts, visualizing your custom or third-party agent performance in the exact same dashboards used for Workday-native agents.

### Schema Requirements

Below, you will find the required input data schema for OTel traces (spans) and logs (events), along with the specific attributes and information required for successful integration.

## [Create Agent Span](https://opentelemetry.io/docs/specs/semconv/gen-ai/gen-ai-agent-spans/#create-agent-span)

| Span Attributes | Requirement Level | Value Type | Description | Example Values |
| :---- | :---- | :---- | :---- | :---- |
| **Span Info** |  |  |  |  |
| start\_time | Required | datetime | Start timestamp | 2021-10-22 16:04:01.209458162 \+0000 UTC |
| end\_time | Required | datetime | End timestamp | 2021-10-22 16:04:01.209514132 \+0000 UTC |
| trace\_id | Required | string | The unique identifier of the trace. | 7bba9f33312b3dbb8b2c2c62bb7abe2d |
| span\_id | Required | string | The unique identifier of the span. | 086e83747d0e381e |
| **Default Attributes** |  |  |  |  |
| [error.type](https://opentelemetry.io/docs/specs/semconv/registry/attributes/error/) | Conditionally Required if the operation ended in an error | string | Describes a class of error the operation ended with. | timeout; java.net.UnknownHostException; server\_certificate\_invalid; 500 |
| [gen\_ai.agent.description](https://opentelemetry.io/docs/specs/semconv/registry/attributes/gen-ai/) | Required | string | Free-form description of the GenAI agent provided by the application. | Helps with math problems; Generates fiction stories |
| [gen\_ai.agent.id](https://opentelemetry.io/docs/specs/semconv/registry/attributes/gen-ai/) | Required | string | The unique identifier of the GenAI agent. | asst\_5j66UpCpwteGg4YSxUnt7lPY |
| [gen\_ai.agent.name](https://opentelemetry.io/docs/specs/semconv/registry/attributes/gen-ai/) | Required | string | Human-readable name of the GenAI agent provided by the application. | Math Tutor; Fiction Writer |
| [gen\_ai.operation.name](https://opentelemetry.io/docs/specs/semconv/registry/attributes/gen-ai/) | Required | string | The name of the operation being performed. | chat; generate\_content; text\_completion |
| **Additional Attributes** |  |  |  |  |
| [browser.mobile](https://opentelemetry.io/docs/specs/semconv/registry/attributes/browser/#browser-mobile) | Recommended if available | boolean | A boolean that is true if the browser is running on a mobile device |  |
| [session.id](https://opentelemetry.io/docs/specs/semconv/registry/attributes/session/#session-id) | Required | string | A unique id to identify a session. | 00112233-4455-6677-8899-aabbccddeeff |
| [gen\_ai.conversation.id](https://opentelemetry.io/docs/specs/semconv/registry/attributes/gen-ai/#gen-ai-conversation-id) | Required | string | The unique identifier for a conversation/thread, used to store and correlate messages within this conversation. | conv\_5j66UpCpwteGg4YSxUnt7lPY |
| [user.id](https://opentelemetry.io/docs/specs/semconv/registry/attributes/user/#user-id) | Required | string | Unique identifier of the user. | S-1-5-21-202424912787-2692429404-2351956786-1000 |
| [user.name](https://opentelemetry.io/docs/specs/semconv/registry/attributes/user/#user-name) OR [user.email](https://opentelemetry.io/docs/specs/semconv/registry/attributes/user/#user-email) (to retrieve worker ID) | Required | string | Short name or login/username of the user. OR User email address. | a.einstein OR a.einstein@example.com |

## [Invoke Agent Client Span](https://opentelemetry.io/docs/specs/semconv/gen-ai/gen-ai-agent-spans/#invoke-agent-client-span)

| Span Attributes | Requirement Level | Value Type | Description | Example Values |
| :---- | :---- | :---- | :---- | :---- |
| **Span Info** |  |  |  |  |
| start\_time | Required | datetime | Start timestamp | 2021-10-22 16:04:01.209458162 \+0000 UTC |
| end\_time | Required | datetime | End timestamp | 2021-10-22 16:04:01.209514132 \+0000 UTC |
| trace\_id | Required | string | The unique identifier of the trace. | 7bba9f33312b3dbb8b2c2c62bb7abe2d |
| span\_id | Required | string | The unique identifier of the span. | 086e83747d0e381e |
| **Default Attributes** |  |  |  |  |
| [error.type](https://opentelemetry.io/docs/specs/semconv/registry/attributes/error/) | Conditionally Required if the operation ended in an error | string | Describes a class of error the operation ended with. | timeout; java.net.UnknownHostException; server\_certificate\_invalid; 500 |
| [gen\_ai.agent.description](https://opentelemetry.io/docs/specs/semconv/registry/attributes/gen-ai/) | Required | string | Free-form description of the GenAI agent provided by the application. | Helps with math problems; Generates fiction stories |
| [gen\_ai.agent.id](https://opentelemetry.io/docs/specs/semconv/registry/attributes/gen-ai/) | Required | string | The unique identifier of the GenAI agent. | asst\_5j66UpCpwteGg4YSxUnt7lPY |
| [gen\_ai.agent.name](https://opentelemetry.io/docs/specs/semconv/registry/attributes/gen-ai/) | Required | string | Human-readable name of the GenAI agent provided by the application. | Math Tutor; Fiction Writer |
| [gen\_ai.conversation.id](https://opentelemetry.io/docs/specs/semconv/registry/attributes/gen-ai/#gen-ai-conversation-id) | Required | string | The unique identifier for a conversation/thread, used to store and correlate messages within this conversation. | conv\_5j66UpCpwteGg4YSxUnt7lPY |
| [gen\_ai.operation.name](https://opentelemetry.io/docs/specs/semconv/registry/attributes/gen-ai/) | Required | string | The name of the operation being performed. | chat; generate\_content; text\_completion |
| **Additional Attributes** |  |  |  |  |
| [browser.mobile](https://opentelemetry.io/docs/specs/semconv/registry/attributes/browser/#browser-mobile) | Recommended if available | boolean | A boolean that is true if the browser is running on a mobile device |  |
| [gen\_ai.response.id](https://opentelemetry.io/docs/specs/semconv/registry/attributes/gen-ai/#gen-ai-response-id) | Required (if not available send gen\_ai.response.finish\_reasons) | string | The unique identifier for the completion. | chatcmpl-123 |
| [gen\_ai.response.finish\_reasons](https://opentelemetry.io/docs/specs/semconv/registry/attributes/gen-ai/#gen-ai-response-finish-reasons) | Required (if [gen\_ai.response.id](http://gen_ai.response.id/) is not available) | string\[\] | Array of reasons the model stopped generating tokens, corresponding to each generation received. | \["stop"\]; \["stop", "length"\] |
| [session.id](https://opentelemetry.io/docs/specs/semconv/registry/attributes/session/#session-id) | Required | string | A unique id to identify a session. | 00112233-4455-6677-8899-aabbccddeeff |
| [user.id](https://opentelemetry.io/docs/specs/semconv/registry/attributes/user/#user-id) | Required | string | Unique identifier of the user. | S-1-5-21-202424912787-2692429404-2351956786-1000 |
| [user.name](https://opentelemetry.io/docs/specs/semconv/registry/attributes/user/#user-name) OR [user.email](https://opentelemetry.io/docs/specs/semconv/registry/attributes/user/#user-email) (to retrieve worker ID) | Required | string | Short name or login/username of the user. OR User email address. | a.einstein OR a.einstein@example.com |

## [Invoke Agent Internal Span](https://opentelemetry.io/docs/specs/semconv/gen-ai/gen-ai-agent-spans/#invoke-agent-internal-span)

| Span Attributes | Requirement Level | Value Type | Description | Example Values |
| :---- | :---- | :---- | :---- | :---- |
| **Span Info** |  |  |  |  |
| start\_time | Required | datetime | Start timestamp | 2021-10-22 16:04:01.209458162 \+0000 UTC |
| end\_time | Required | datetime | End timestamp | 2021-10-22 16:04:01.209514132 \+0000 UTC |
| trace\_id | Required | string | The unique identifier of the trace. | 7bba9f33312b3dbb8b2c2c62bb7abe2d |
| span\_id | Required | string | The unique identifier of the span. | 086e83747d0e381e |
| **Default Attributes** |  |  |  |  |
| [error.type](https://opentelemetry.io/docs/specs/semconv/registry/attributes/error/) | Conditionally Required if the operation ended in an error | string | Describes a class of error the operation ended with. | timeout; java.net.UnknownHostException; server\_certificate\_invalid; 500 |
| [gen\_ai.agent.description](https://opentelemetry.io/docs/specs/semconv/registry/attributes/gen-ai/) | Required | string | Free-form description of the GenAI agent provided by the application. | Helps with math problems; Generates fiction stories |
| [gen\_ai.agent.id](https://opentelemetry.io/docs/specs/semconv/registry/attributes/gen-ai/) | Required | string | The unique identifier of the GenAI agent. | asst\_5j66UpCpwteGg4YSxUnt7lPY |
| [gen\_ai.agent.name](https://opentelemetry.io/docs/specs/semconv/registry/attributes/gen-ai/) | Required | string | Human-readable name of the GenAI agent provided by the application. | Math Tutor; Fiction Writer |
| [gen\_ai.conversation.id](https://opentelemetry.io/docs/specs/semconv/registry/attributes/gen-ai/#gen-ai-conversation-id) | Required | string | The unique identifier for a conversation/thread, used to store and correlate messages within this conversation. | conv\_5j66UpCpwteGg4YSxUnt7lPY |
| [gen\_ai.operation.name](https://opentelemetry.io/docs/specs/semconv/registry/attributes/gen-ai/) | Required | string | The name of the operation being performed. | chat; generate\_content; text\_completion |
| **Additional Attributes** |  |  |  |  |
| [browser.mobile](https://opentelemetry.io/docs/specs/semconv/registry/attributes/browser/#browser-mobile) | Recommended if available | boolean | A boolean that is true if the browser is running on a mobile device |  |
| [session.id](https://opentelemetry.io/docs/specs/semconv/registry/attributes/session/#session-id) | Required | string | A unique id to identify a session. | 00112233-4455-6677-8899-aabbccddeeff |
| [user.id](https://opentelemetry.io/docs/specs/semconv/registry/attributes/user/#user-id) | Required | string | Unique identifier of the user. | S-1-5-21-202424912787-2692429404-2351956786-1000 |
| [user.name](https://opentelemetry.io/docs/specs/semconv/registry/attributes/user/#user-name) OR [user.email](https://opentelemetry.io/docs/specs/semconv/registry/attributes/user/#user-email) (to retrieve worker ID) | Required | string | Short name or login/username of the user. OR User email address. | a.einstein OR a.einstein@example.com |

## [Invoke Workflow Span](https://opentelemetry.io/docs/specs/semconv/gen-ai/gen-ai-agent-spans/#invoke-workflow-span)

| Span Attributes | Requirement Level | Value Type | Description | Example Values |
| :---- | :---- | :---- | :---- | :---- |
| **Span Info** |  |  |  |  |
| start\_time | Required | datetime | Start timestamp | 2021-10-22 16:04:01.209458162 \+0000 UTC |
| end\_time | Required | datetime | End timestamp | 2021-10-22 16:04:01.209514132 \+0000 UTC |
| trace\_id | Required | string | The unique identifier of the trace. | 7bba9f33312b3dbb8b2c2c62bb7abe2d |
| span\_id | Required | string | The unique identifier of the span. | 086e83747d0e381e |
| **Default Attributes** |  |  |  |  |
| [error.type](https://opentelemetry.io/docs/specs/semconv/registry/attributes/error/) | Conditionally Required if the operation ended in an error | string | Describes a class of error the operation ended with. | timeout; java.net.UnknownHostException; server\_certificate\_invalid; 500 |
| [gen\_ai.operation.name](https://opentelemetry.io/docs/specs/semconv/registry/attributes/gen-ai/) | Required | string | The name of the operation being performed. | chat; generate\_content; text\_completion |
| [gen\_ai.workflow.name](https://opentelemetry.io/docs/specs/semconv/registry/attributes/gen-ai/) | Conditionally Required when available | string | Human-readable name of the GenAI workflow provided by the application. | multi\_agent\_rag; customer\_support\_pipeline |
| **Additional Attributes** |  |  |  |  |
| [browser.mobile](https://opentelemetry.io/docs/specs/semconv/registry/attributes/browser/#browser-mobile) | Recommended if available | boolean | A boolean that is true if the browser is running on a mobile device |  |
| [gen\_ai.agent.description](https://opentelemetry.io/docs/specs/semconv/registry/attributes/gen-ai/) | Required | string | Free-form description of the GenAI agent provided by the application. | Helps with math problems; Generates fiction stories |
| [gen\_ai.agent.id](https://opentelemetry.io/docs/specs/semconv/registry/attributes/gen-ai/) | Required | string | The unique identifier of the GenAI agent. | asst\_5j66UpCpwteGg4YSxUnt7lPY |
| [gen\_ai.agent.name](https://opentelemetry.io/docs/specs/semconv/registry/attributes/gen-ai/) | Required | string | Human-readable name of the GenAI agent provided by the application. | Math Tutor; Fiction Writer |
| [session.id](https://opentelemetry.io/docs/specs/semconv/registry/attributes/session/#session-id) | Required | string | A unique id to identify a session. | 00112233-4455-6677-8899-aabbccddeeff |
| [user.id](https://opentelemetry.io/docs/specs/semconv/registry/attributes/user/#user-id) | Required | string | Unique identifier of the user. | S-1-5-21-202424912787-2692429404-2351956786-1000 |
| [user.name](https://opentelemetry.io/docs/specs/semconv/registry/attributes/user/#user-name) OR [user.email](https://opentelemetry.io/docs/specs/semconv/registry/attributes/user/#user-email) (to retrieve worker ID) | Required | string | Short name or login/username of the user. OR User email address. | a.einstein OR a.einstein@example.com |

## [Inference Span](https://opentelemetry.io/docs/specs/semconv/gen-ai/gen-ai-spans/#inference)

| Span Attributes | Requirement Level | Value Type | Description | Example Values |
| :---- | :---- | :---- | :---- | :---- |
| **Span Info** |  |  |  |  |
| start\_time | Required | datetime | Start timestamp | 2021-10-22 16:04:01.209458162 \+0000 UTC |
| end\_time | Required | datetime | End timestamp | 2021-10-22 16:04:01.209514132 \+0000 UTC |
| trace\_id | Required | string | The unique identifier of the trace. | 7bba9f33312b3dbb8b2c2c62bb7abe2d |
| span\_id | Required | string | The unique identifier of the span. | 086e83747d0e381e |
| **Default Attributes** |  |  |  |  |
| [error.type](https://opentelemetry.io/docs/specs/semconv/registry/attributes/error/) | Conditionally Required if the operation ended in an error | string | Describes a class of error the operation ended with. | timeout; java.net.UnknownHostException; server\_certificate\_invalid; 500 |
| [gen\_ai.conversation.id](https://opentelemetry.io/docs/specs/semconv/registry/attributes/gen-ai/#gen-ai-conversation-id) | Required | string | The unique identifier for a conversation/thread, used to store and correlate messages within this conversation. | conv\_5j66UpCpwteGg4YSxUnt7lPY |
| [gen\_ai.operation.name](https://opentelemetry.io/docs/specs/semconv/registry/attributes/gen-ai/) | Required | string | The name of the operation being performed. | chat; generate\_content; text\_completion |
| [gen\_ai.response.id](https://opentelemetry.io/docs/specs/semconv/registry/attributes/gen-ai/) | Required | string | The unique identifier for the completion. | chatcmpl-123 |
| **Additional Attributes** |  |  |  |  |
| [browser.mobile](https://opentelemetry.io/docs/specs/semconv/registry/attributes/browser/#browser-mobile) | Recommended if available | boolean | A boolean that is true if the browser is running on a mobile device |  |
| [gen\_ai.agent.description](https://opentelemetry.io/docs/specs/semconv/registry/attributes/gen-ai/) | Required | string | Free-form description of the GenAI agent provided by the application. | Helps with math problems; Generates fiction stories |
| [gen\_ai.agent.id](https://opentelemetry.io/docs/specs/semconv/registry/attributes/gen-ai/) | Required | string | The unique identifier of the GenAI agent. | asst\_5j66UpCpwteGg4YSxUnt7lPY |
| [gen\_ai.agent.name](https://opentelemetry.io/docs/specs/semconv/registry/attributes/gen-ai/) | Required | string | Human-readable name of the GenAI agent provided by the application. | Math Tutor; Fiction Writer |
| [gen\_ai.response.id](https://opentelemetry.io/docs/specs/semconv/registry/attributes/gen-ai/#gen-ai-response-id) | Required (if not available send gen\_ai.response.finish\_reasons) | string | The unique identifier for the completion. | chatcmpl-123 |
| [gen\_ai.response.finish\_reasons](https://opentelemetry.io/docs/specs/semconv/registry/attributes/gen-ai/#gen-ai-response-finish-reasons) | Required (if [gen\_ai.response.id](http://gen_ai.response.id/) is not available) | string\[\] | Array of reasons the model stopped generating tokens, corresponding to each generation received. | \["stop"\]; \["stop", "length"\] |
| [gen\_ai.tool.call.id](https://opentelemetry.io/docs/specs/semconv/registry/attributes/gen-ai/) | Recommended if available | string | The tool call identifier. | call\_mszuSIzqtI65i1wAUOE8w5H4 |
| [gen\_ai.tool.description](https://opentelemetry.io/docs/specs/semconv/registry/attributes/gen-ai/#gen-ai-tool-description) | Recommended if available | string | The tool description. | Multiply two numbers |
| [gen\_ai.tool.name](http://gen_ai.tool.name/) | Required | string | Name of the tool utilized by the agent. | Flights |
| [session.id](https://opentelemetry.io/docs/specs/semconv/registry/attributes/session/#session-id) | Required | string | A unique id to identify a session. | 00112233-4455-6677-8899-aabbccddeeff |
| [user.id](https://opentelemetry.io/docs/specs/semconv/registry/attributes/user/#user-id) | Required | string | Unique identifier of the user. | S-1-5-21-202424912787-2692429404-2351956786-1000 |
| [user.name](https://opentelemetry.io/docs/specs/semconv/registry/attributes/user/#user-name) OR [user.email](https://opentelemetry.io/docs/specs/semconv/registry/attributes/user/#user-email) (to retrieve worker ID) | Required | string | Short name or login/username of the user. OR User email address. | a.einstein OR a.einstein@example.com |

## [Embeddings Span](https://opentelemetry.io/docs/specs/semconv/gen-ai/gen-ai-spans/#embeddings)

| Span Attributes | Requirement Level | Value Type | Description | Example Values |
| :---- | :---- | :---- | :---- | :---- |
| **Span Info** |  |  |  |  |
| start\_time | Required | datetime | Start timestamp | 2021-10-22 16:04:01.209458162 \+0000 UTC |
| end\_time | Required | datetime | End timestamp | 2021-10-22 16:04:01.209514132 \+0000 UTC |
| trace\_id | Required | string | The unique identifier of the trace. | 7bba9f33312b3dbb8b2c2c62bb7abe2d |
| span\_id | Required | string | The unique identifier of the span. | 086e83747d0e381e |
| **Default Attributes** |  |  |  |  |
| [error.type](https://opentelemetry.io/docs/specs/semconv/registry/attributes/error/) | Conditionally Required if the operation ended in an error | string | Describes a class of error the operation ended with. | timeout; java.net.UnknownHostException; server\_certificate\_invalid; 500 |
| [gen\_ai.operation.name](https://opentelemetry.io/docs/specs/semconv/registry/attributes/gen-ai/) | Required | string | The name of the operation being performed. | chat; generate\_content; text\_completion |
| **Additional Attributes** |  |  |  |  |
| [browser.mobile](https://opentelemetry.io/docs/specs/semconv/registry/attributes/browser/#browser-mobile) | Recommended if available | boolean | A boolean that is true if the browser is running on a mobile device |  |
| [gen\_ai.agent.description](https://opentelemetry.io/docs/specs/semconv/registry/attributes/gen-ai/) | Required | string | Free-form description of the GenAI agent provided by the application. | Helps with math problems; Generates fiction stories |
| [gen\_ai.agent.id](https://opentelemetry.io/docs/specs/semconv/registry/attributes/gen-ai/) | Required | string | The unique identifier of the GenAI agent. | asst\_5j66UpCpwteGg4YSxUnt7lPY |
| [gen\_ai.agent.name](https://opentelemetry.io/docs/specs/semconv/registry/attributes/gen-ai/) | Required | string | Human-readable name of the GenAI agent provided by the application. | Math Tutor; Fiction Writer |
| [gen\_ai.tool.call.id](https://opentelemetry.io/docs/specs/semconv/registry/attributes/gen-ai/) | Recommended if available | string | The tool call identifier. | call\_mszuSIzqtI65i1wAUOE8w5H4 |
| [gen\_ai.tool.description](https://opentelemetry.io/docs/specs/semconv/registry/attributes/gen-ai/#gen-ai-tool-description) | Recommended if available | string | The tool description. | Multiply two numbers |
| [gen\_ai.tool.name](http://gen_ai.tool.name/) | Required | string | Name of the tool utilized by the agent. | Flights |
| [session.id](https://opentelemetry.io/docs/specs/semconv/registry/attributes/session/#session-id) | Required | string | A unique id to identify a session. | 00112233-4455-6677-8899-aabbccddeeff |
| [user.id](https://opentelemetry.io/docs/specs/semconv/registry/attributes/user/#user-id) | Required | string | Unique identifier of the user. | S-1-5-21-202424912787-2692429404-2351956786-1000 |
| [user.name](https://opentelemetry.io/docs/specs/semconv/registry/attributes/user/#user-name) OR [user.email](https://opentelemetry.io/docs/specs/semconv/registry/attributes/user/#user-email) (to retrieve worker ID) | Required | string | Short name or login/username of the user. OR User email address. | a.einstein OR a.einstein@example.com |

## [Retrievals Span](https://opentelemetry.io/docs/specs/semconv/gen-ai/gen-ai-spans/#retrievals)

| Span Attributes | Requirement Level | Value Type | Description | Example Values |
| :---- | :---- | :---- | :---- | :---- |
| **Span Info** |  |  |  |  |
| start\_time | Required | datetime | Start timestamp | 2021-10-22 16:04:01.209458162 \+0000 UTC |
| end\_time | Required | datetime | End timestamp | 2021-10-22 16:04:01.209514132 \+0000 UTC |
| trace\_id | Required | string | The unique identifier of the trace. | 7bba9f33312b3dbb8b2c2c62bb7abe2d |
| span\_id | Required | string | The unique identifier of the span. | 086e83747d0e381e |
| **Default Attributes** |  |  |  |  |
| [error.type](https://opentelemetry.io/docs/specs/semconv/registry/attributes/error/) | Conditionally Required if the operation ended in an error | string | Describes a class of error the operation ended with. | timeout; java.net.UnknownHostException; server\_certificate\_invalid; 500 |
| [gen\_ai.operation.name](https://opentelemetry.io/docs/specs/semconv/registry/attributes/gen-ai/) | Required | string | The name of the operation being performed. | chat; generate\_content; text\_completion |
| **Additional Attributes** |  |  |  |  |
| [browser.mobile](https://opentelemetry.io/docs/specs/semconv/registry/attributes/browser/#browser-mobile) | Recommended if available | boolean | A boolean that is true if the browser is running on a mobile device |  |
| [gen\_ai.agent.description](https://opentelemetry.io/docs/specs/semconv/registry/attributes/gen-ai/) | Required | string | Free-form description of the GenAI agent provided by the application. | Helps with math problems; Generates fiction stories |
| [gen\_ai.agent.id](https://opentelemetry.io/docs/specs/semconv/registry/attributes/gen-ai/) | Required | string | The unique identifier of the GenAI agent. | asst\_5j66UpCpwteGg4YSxUnt7lPY |
| [gen\_ai.agent.name](https://opentelemetry.io/docs/specs/semconv/registry/attributes/gen-ai/) | Required | string | Human-readable name of the GenAI agent provided by the application. | Math Tutor; Fiction Writer |
| [gen\_ai.tool.call.id](https://opentelemetry.io/docs/specs/semconv/registry/attributes/gen-ai/) | Recommended if available | string | The tool call identifier. | call\_mszuSIzqtI65i1wAUOE8w5H4 |
| [gen\_ai.tool.description](https://opentelemetry.io/docs/specs/semconv/registry/attributes/gen-ai/#gen-ai-tool-description) | Recommended if available | string | The tool description. | Multiply two numbers |
| [gen\_ai.tool.name](http://gen_ai.tool.name/) | Required | string | Name of the tool utilized by the agent. | Flights |
| [session.id](https://opentelemetry.io/docs/specs/semconv/registry/attributes/session/#session-id) | Required | string | A unique id to identify a session. | 00112233-4455-6677-8899-aabbccddeeff |
| [user.id](https://opentelemetry.io/docs/specs/semconv/registry/attributes/user/#user-id) | Required | string | Unique identifier of the user. | S-1-5-21-202424912787-2692429404-2351956786-1000 |
| [user.name](https://opentelemetry.io/docs/specs/semconv/registry/attributes/user/#user-name) OR [user.email](https://opentelemetry.io/docs/specs/semconv/registry/attributes/user/#user-email) (to retrieve worker ID) | Required | string | Short name or login/username of the user. OR User email address. | a.einstein OR a.einstein@example.com |

## [Execute Tool Span](https://opentelemetry.io/docs/specs/semconv/gen-ai/gen-ai-spans/#execute-tool-span)

| Span Attributes | Requirement Level | Value Type | Description | Example Values |
| :---- | :---- | :---- | :---- | :---- |
| **Span Info** |  |  |  |  |
| start\_time | Required | datetime | Start timestamp | 2021-10-22 16:04:01.209458162 \+0000 UTC |
| end\_time | Required | datetime | End timestamp | 2021-10-22 16:04:01.209514132 \+0000 UTC |
| trace\_id | Required | string | The unique identifier of the trace. | 7bba9f33312b3dbb8b2c2c62bb7abe2d |
| span\_id | Required | string | The unique identifier of the span. | 086e83747d0e381e |
| **Default Attributes** |  |  |  |  |
| [error.type](https://opentelemetry.io/docs/specs/semconv/registry/attributes/error/) | Conditionally Required if the operation ended in an error | string | Describes a class of error the operation ended with. | timeout; java.net.UnknownHostException; server\_certificate\_invalid; 500 |
| [gen\_ai.operation.name](https://opentelemetry.io/docs/specs/semconv/registry/attributes/gen-ai/) | Required | string | The name of the operation being performed. | chat; generate\_content; text\_completion |
| [gen\_ai.tool.description](https://opentelemetry.io/docs/specs/semconv/registry/attributes/gen-ai/) | Recommended if available | string | The tool description. | Multiply two numbers |
| [gen\_ai.tool.name](https://opentelemetry.io/docs/specs/semconv/registry/attributes/gen-ai/) | Required | string | Name of the tool utilized by the agent. | Flights |
| **Additional Attributes** |  |  |  |  |
| [browser.mobile](https://opentelemetry.io/docs/specs/semconv/registry/attributes/browser/#browser-mobile) | Recommended if available | boolean | A boolean that is true if the browser is running on a mobile device |  |
| [gen\_ai.agent.description](https://opentelemetry.io/docs/specs/semconv/registry/attributes/gen-ai/) | Required | string | Free-form description of the GenAI agent provided by the application. | Helps with math problems; Generates fiction stories |
| [gen\_ai.agent.id](https://opentelemetry.io/docs/specs/semconv/registry/attributes/gen-ai/) | Required | string | The unique identifier of the GenAI agent. | asst\_5j66UpCpwteGg4YSxUnt7lPY |
| [gen\_ai.agent.name](https://opentelemetry.io/docs/specs/semconv/registry/attributes/gen-ai/) | Required | string | Human-readable name of the GenAI agent provided by the application. | Math Tutor; Fiction Writer |
| [gen\_ai.response.id](https://opentelemetry.io/docs/specs/semconv/registry/attributes/gen-ai/#gen-ai-response-id) | Required (if not available send gen\_ai.response.finish\_reasons) | string | The unique identifier for the completion. | chatcmpl-123 |
| [gen\_ai.response.finish\_reasons](https://opentelemetry.io/docs/specs/semconv/registry/attributes/gen-ai/#gen-ai-response-finish-reasons) | Required (if [gen\_ai.response.id](http://gen_ai.response.id/) is not available) | string\[\] | Array of reasons the model stopped generating tokens, corresponding to each generation received. | \["stop"\]; \["stop", "length"\] |
| [session.id](https://opentelemetry.io/docs/specs/semconv/registry/attributes/session/#session-id) | Required | string | A unique id to identify a session. | 00112233-4455-6677-8899-aabbccddeeff |
| [user.id](https://opentelemetry.io/docs/specs/semconv/registry/attributes/user/#user-id) | Required | string | Unique identifier of the user. | S-1-5-21-202424912787-2692429404-2351956786-1000 |
| [user.name](https://opentelemetry.io/docs/specs/semconv/registry/attributes/user/#user-name) OR [user.email](https://opentelemetry.io/docs/specs/semconv/registry/attributes/user/#user-email) (to retrieve worker ID) | Required | string | Short name or login/username of the user. OR User email address. | a.einstein OR a.einstein@example.com |

## [Client Inference Operation Details Event](https://opentelemetry.io/docs/specs/semconv/gen-ai/gen-ai-events/#event-gen_aiclientinferenceoperationdetails)

| Event Attributes | Requirement Level | Value Type | Description | Example Values |
| :---- | :---- | :---- | :---- | ----- |
| **Event Info** |  |  |  |  |
| timestamp | Required | datetime | The timestamp of the event | 2024-08-04T12:34:56.789Z |
| trace\_id | Required | string | The unique identifier of the trace. | 7bba9f33312b3dbb8b2c2c62bb7abe2d |
| span\_id | Required | string | The unique identifier of the span. | 086e83747d0e381e |
| **Default Attributes** |  |  |  |  |
| [error.type](https://opentelemetry.io/docs/specs/semconv/registry/attributes/error/) | Conditionally Required if the operation ended in an error | string | Describes a class of error the operation ended with. | timeout; java.net.UnknownHostException; server\_certificate\_invalid; 500 |
| [gen\_ai.operation.name](https://opentelemetry.io/docs/specs/semconv/registry/attributes/gen-ai/) | Required | string | The name of the operation being performed. | chat; generate\_content; text\_completion |
| [gen\_ai.response.id](https://opentelemetry.io/docs/specs/semconv/registry/attributes/gen-ai/) | Required | string | The unique identifier for the completion. | chatcmpl-123 |
| [gen\_ai.tool.definitions](https://opentelemetry.io/docs/specs/semconv/registry/attributes/gen-ai/) | Opt-In | any | The list of tool definitions available to the GenAI agent or model. | \[ { “type”: “function”, “name”: “get\_current\_weather”, “description”: “Get the current weather in a given location”, “parameters”: { “type”: “object”, “properties”: { “location”: { “type”: “string”, “description”: “The city and state, e.g. San Francisco, CA” }, “unit”: { “type”: “string”, “enum”: \[ “celsius”, “fahrenheit” \] } }, “required”: \[ “location”, “unit” \] } } \] |
| userId | Required | string | The unique identifier of the user. | 12345 |
| username (or jwt token) | Required | string | The unique identifier of the worker for Workday login. Might be inferable from the jwt token. | johndoe |
| [gen\_ai.input.messages](https://opentelemetry.io/docs/specs/semconv/registry/attributes/gen-ai/) | Opt-In | any | The chat history provided to the agent as an input. | \[ { “role”: “user”, “parts”: \[ { “type”: “text”, “content”: “Weather in Paris?" } \] }, { “role”: “assistant”, “parts”: \[ { “type”: “tool\_call”, “id”: “call\_VSPygqKTWdrhaFErNvMV18Yl”, “name”: “get\_weather”, “arguments”: { “location”: “Paris” } } \] }, { “role”: “tool”, “parts”: \[ { “type”: “tool\_call\_response”, “id”: " call\_VSPygqKTWdrhaFErNvMV18Yl”, “result”: “rainy, 57°F” } \] } \] |
| [gen\_ai.output.messages](https://opentelemetry.io/docs/specs/semconv/registry/attributes/gen-ai/) | Opt-In | any | Messages returned by the agent where each message represents a specific agent response (choice, candidate). | \[ { “role”: “assistant”, “parts”: \[ { “type”: “text”, “content”: “The weather in Paris is currently rainy with a temperature of 57°F." } \], “finish\_reason”: “stop” } \] |

