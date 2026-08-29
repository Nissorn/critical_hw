# MCP provider compatibility research note

**Research date:** 2026-08-29 (all links below were accessed on this date).  This
note is a compatibility and security scoping exercise, not legal advice or a
claim that any provider will keep a beta feature unchanged.  It separates
**protocol facts**, **provider facts**, **inferences/recommendations**, and
**unknowns**.

## Bottom line

**Protocol fact.** Model Context Protocol (MCP) is a JSON-RPC protocol and
architecture, not a data-processing agreement, identity provider, tenant model,
or guarantee that every client implements every primitive.  A server can expose
tools, resources, and prompts, but a particular host decides which of those it
discovers, renders, sends to a model, or permits the model to invoke.

**Provider fact.** The provider surfaces checked here have materially different
contracts:

* Claude custom remote connectors (including Claude Cowork) document tools,
  resources, and prompts, but not resource subscriptions, sampling, or advanced
  / draft capabilities.  Remote connector requests originate from Anthropic's
  cloud, including when Cowork or Claude Desktop is being used.
* ChatGPT developer-mode apps document remote MCP with both Streamable HTTP and
  SSE, and full tool support including write/modify actions is rolling out in
  beta to Business and Enterprise/Edu.  Local servers are not connected
  directly; OpenAI documents Secure MCP Tunnel for private servers.
* The OpenAI Responses API documents remote MCP as a built-in `mcp` tool.  It
  imports and calls tools, accepts either Streamable HTTP or legacy HTTP/SSE,
  defaults to approval for calls, and supports tool allow-lists.  The guide does
  **not** document `resources/list` or `prompts/list` consumption by the API.

**Inference/recommendation.** A generic server can be made to work across these
surfaces only by implementing a conservative, tested intersection rather than
assuming “MCP-compatible” means “feature-equivalent.”  Start with authenticated,
read-only tools (and optionally small, explicitly selected resources), keep
prompts optional, and use a provider-specific compatibility test matrix.  Treat
the current MCP revision and older provider revisions as a version-negotiation
problem.

For a teacher-facing product, do not send student PII in external prompts unless
the school has approved the purpose, legal basis, processor/subprocessor terms,
retention, residency, access, and incident process.  A provider's statement
that it does not train on workspace/API data does not make a connected
third-party MCP server subject to the same statement.

## 1. MCP protocol facts

### Architecture and primitives

The current specification revision is **2026-07-28**.  It describes a host
(the LLM application), one client per server, and servers that provide context
and capabilities.  The current revision is stateless: requests carry protocol
version and client capabilities in `_meta`.  Servers can be local processes or
remote services.  See [MCP Specification 2026-07-28](https://modelcontextprotocol.io/specification/2026-07-28)
and [Architecture](https://modelcontextprotocol.io/specification/2026-07-28/architecture)
(spec version date 2026-07-28; accessed 2026-08-29).

| Primitive | What a server may expose | Interaction model in the specification | Teacher-product interpretation |
|---|---|---|---|
| **Tools** | Named functions with descriptions, JSON Schema input, optional output schema, and results containing text, images, audio, resource links, embedded resources, or structured content. They can query databases, call APIs, compute, or mutate external systems. | **Model-controlled:** the model may discover and invoke a tool from context. The protocol does not prescribe the UI. | Keep read and write operations separate. A tool description or `readOnlyHint` is metadata, not an authorization boundary. |
| **Resources** | URI-identified context/data such as files, database schemas, or application-specific information; servers may provide list/read, templates, and (where supported) update notifications. | **Application-driven:** the host decides how to list, select, search, or inject a resource. | Expose only the class/teacher slice needed for the current request; resource discovery must not become bulk student export. |
| **Prompts** | Named prompt templates with arguments that resolve to structured user/assistant messages and can include resource links or media. | **User-controlled:** the user is intended to select a prompt; the server authors its content. | Make prompts optional and visible. A prompt is instruction content, not a policy enforcement mechanism. |

Sources: [Tools](https://modelcontextprotocol.io/specification/2026-07-28/server/tools),
[Resources](https://modelcontextprotocol.io/specification/2026-07-28/server/resources),
and [Prompts](https://modelcontextprotocol.io/specification/2026-07-28/server/prompts)
(spec version date 2026-07-28; accessed 2026-08-29).  Each server capability must
be advertised; clients can implement only a subset.  The specification says a
host must obtain consent before exposing user data or invoking tools, and the
tools guidance recommends a human who can deny invocations.  It also warns that
tool annotations are untrusted unless the server is trusted.

### Transport and version compatibility

The current [transport specification](https://modelcontextprotocol.io/specification/2026-07-28/basic/transports)
(version date 2026-07-28; accessed 2026-08-29) defines:

* **stdio:** newline-delimited JSON-RPC over a client-launched local
  subprocess.  This is the natural local-server binding.
* **Streamable HTTP:** one MCP HTTP endpoint; each client message is a POST and
  the response is either one JSON object or a request-scoped SSE stream.
* **Custom transports:** permitted only if they preserve JSON-RPC, message
  patterns, metadata, and documented framing/cancellation.

The 2026-07-28 changelog says that Streamable HTTP changed substantially from
2025-11-25: protocol sessions and `Mcp-Session-Id` were removed, the
`initialize` handshake was removed, per-request `_meta` is used, and the old
HTTP+SSE transport is deprecated.  See [2026-07-28 changelog](https://modelcontextprotocol.io/specification/2026-07-28/changelog)
(version date 2026-07-28; accessed 2026-08-29).  The same page describes
backward-compatibility behavior, but a client still has to implement the old
wire behavior when it claims an older revision.

**Inference/recommendation.** For the first release, implement Streamable HTTP
and exercise the 2025-03-26/2025-06-18/2025-11-25 compatibility paths required by
target clients.  Do not assume that a server speaking only the 2026 stateless
wire format works with a client whose documentation still names 2025 revisions;
do not assume that legacy HTTP+SSE will remain available indefinitely.  Keep the
server's primitive set and schemas stable, deterministic, and small.

### Authentication and authorization

The [current authorization specification](https://modelcontextprotocol.io/specification/2026-07-28/basic/authorization)
(version date 2026-07-28; accessed 2026-08-29) makes authorization optional.
For HTTP transports, an implementation that supports authorization should
conform to the MCP OAuth profile; stdio implementations should instead obtain
credentials from the environment.  When HTTP authorization is used, the key
requirements include:

1. The MCP server is an OAuth resource server and the MCP client is an OAuth
   client acting for the resource owner.
2. The server publishes OAuth 2.0 Protected Resource Metadata (RFC 9728), and
   the authorization server publishes OAuth Authorization Server Metadata
   (RFC 8414) or OpenID Connect Discovery.  The current revision requires
   clients to support both discovery mechanisms.
3. OAuth 2.1, PKCE, resource indicators, issuer validation, and audience-bound
   access tokens are part of the current flow.  The client sends
   `Authorization: Bearer <access-token>` on every HTTP request; access tokens
   must not be placed in a query string.  The server rejects tokens not issued
   for it.
4. The current revision favors Client ID Metadata Documents (CIMD); Dynamic
   Client Registration (DCR) remains for backward compatibility.  This matters
   because existing clients may support DCR while a newer client prefers CIMD.

The older [2025-06-18 authorization page](https://modelcontextprotocol.io/specification/2025-06-18/basic/authorization)
(version date 2025-06-18; accessed 2026-08-29) has the same central HTTP/OAuth
shape but describes DCR as the normal client-registration path.  This is a
concrete compatibility reason to support both registration approaches (or offer
an administrator-supplied pre-registered client), rather than implementing only
the newest path.

**Security fact.** MCP's security guidance prohibits token passthrough: a server
must not accept a token issued for another resource and forward it downstream.
It also calls out confused-deputy attacks, SSRF during metadata discovery, and
local-server compromise.  See [MCP Security Best Practices](https://modelcontextprotocol.io/docs/2026-07-28/tutorials/security/security_best_practices)
(accessed 2026-08-29).  These are server/client implementation duties; MCP does
not make a third-party server trustworthy.

## 2. Provider comparison (verified 2026-08-29)

The table reports only what first-party documentation states. “Not documented”
means an unresolved compatibility question, not evidence that a feature does
not exist.

| Surface | Remote/custom server support and network limits | Primitives and transport documented | Auth and tenant controls | Write-action behavior | Retention, audit, and deployment limits |
|---|---|---|---|---|---|
| **Claude custom remote connectors, including Cowork** | Custom remote MCP connectors are available on Claude, Cowork, and Claude Desktop for Free, Pro, Max, Team, and Enterprise plans (Free is limited to one custom connector). Requests originate from Anthropic's cloud IP ranges, including when using Cowork/Desktop; a private/VPN/localhost server is not reachable unless made reachable to Anthropic. Local `claude_desktop_config.json` MCP is a separate mechanism and is not available in Cowork or claude.ai. | Anthropic's builder docs say Streamable HTTP and legacy HTTP+SSE are supported (legacy is being deprecated); tools, prompts, text/image tool results, and text/binary resources are supported. Resource subscriptions, sampling, and advanced/draft capabilities are not yet supported in those docs. Claude.ai/Desktop tool-result limit is approximately 150,000 characters and timeout is 300 seconds. | OAuth DCR and CIMD are supported out of the box; Anthropic-held credentials and custom connection flows require review; static headers are beta; no pure machine-to-machine `client_credentials` flow because every connection requires user consent. PKCE S256, refresh, `offline_access`, callback URL, scope challenge, and Anthropic egress allowlisting are documented. | Remote servers may read, create, modify, or delete data. Claude says to review permissions, disable irrelevant tools, and avoid “Allow always” unless the server/tool is trusted. Cowork offers manually approve/automatically approve/skip approvals modes; Team/Enterprise can turn off “Always allow” for connector writes, off by default, and most custom tools without read-only annotation remain gated. | Commercial Claude for Work/API inputs and outputs are not used for training by default; consumer-plan data has different controls. Connector data is not raw training content unless directly copied into a conversation, subject to the stated policy. Cloud Cowork sessions are in the Claude account; local-session history is local, outside standard retention, and not centrally managed/exportable by admins (Enterprise can retrieve local content via Compliance API; deletion endpoint is not yet available). Team/Enterprise can stream Cowork events with OpenTelemetry and Cowork remote sessions are in Compliance API. |
| **ChatGPT developer-mode custom MCP apps (Business, Enterprise/Edu)** | Full MCP support, including modify/write actions, is rolling out in beta to Business and Enterprise/Edu; web only in the cited help article. Workspace admins/owners enable and publish; Enterprise/Edu can use RBAC. Custom apps require an endpoint and metadata. Local servers are not connected directly. | Developer-mode docs explicitly support SSE and streaming HTTP. They say all tools (read and write) are supported; the docs do not promise that ChatGPT consumes every MCP resource/prompt method. | OAuth, no-auth, and mixed-auth are supported. Hosted OAuth can use static credentials, CIMD, or DCR; OAuth providers should issue refresh tokens and advertise `offline_access` if long-lived connectivity is required. Enterprise/Edu admins can control app access and actions. | Write actions normally require confirmation, subject to app permissions, action context, and risk; especially risky actions can be blocked. `readOnlyHint` is used for read-only detection; tools without that hint are treated as writes. Enterprise/Edu can configure actions and new actions are disabled by default after refresh; Business published apps cannot be updated at launch and must be recreated/re-published. | ChatGPT Business says workspace data is excluded from training by default and encrypted in transit/at rest. Enterprise/Edu have Compliance Platform access; the Compliance Logs Platform retains data for 30 days, so an organization wanting longer retention must continuously export logs. Data sent onward to the custom MCP server is subject to that server's retention/residency policy. Apps are plan, role, region, model, and surface dependent. |
| **OpenAI Responses API remote MCP tool** | The `mcp` built-in tool accepts any remote MCP server on the public Internet via `server_url`; OpenAI documents Secure MCP Tunnel for private/on-premises/firewalled servers. API remote servers must support Streamable HTTP or HTTP/SSE. | The guide documents `tools/list` import and `tools/call` output items, tool filtering with `allowed_tools`, and deferred loading. It does not document API consumption of `resources/list` or `prompts/list`; treat those as unknown. | A remote server may require an OAuth access token in the per-request `authorization` field; the application handles OAuth separately. OpenAI says it does not store that field in the Response object, so it must be sent on every request. OpenAI-maintained connectors use connector IDs and OAuth and are a limited list, not arbitrary MCP servers. | By default, approval is requested before data is shared with a connector or remote MCP server. `require_approval` can require every call, allow selected tools without approval, or set `never`; OpenAI recommends approvals for sensitive actions and `allowed_tools` filtering. | OpenAI warns remote MCP servers are third-party/unverified and can exfiltrate anything in model context. API data is not used to train by default; Responses application state is generally retained 30 days when stored, and abuse monitoring is generally 30 days, subject to controls. ZDR/data residency cover the OpenAI side only; the third-party MCP server's policy still applies. Model/tool availability is model-dependent. |
| **A different generic MCP-capable client** | May support stdio, Streamable HTTP, legacy HTTP+SSE, or a proprietary transport; network reachability and local-process policy are client-specific. | Must negotiate/implement a common MCP revision and only the primitives it actually supports. No MCP requirement forces a client to support tools, resources, prompts, subscriptions, sampling, or newer extensions together. | May implement MCP OAuth discovery, pre-registered OAuth, API keys, or no auth. Never assume the client's token-registration or redirect behavior from another provider. | The protocol recommends consent/human denial, but the host owns the UX and policy. A generic client may ignore annotations or have no write confirmation. | Retention, training, residency, logging, rate limits, and workspace controls are outside MCP. Require a written provider contract and verify behavior in a test tenant. |

### Provider-source notes

* Anthropic: [Get started with custom connectors using remote MCP](https://support.claude.com/en/articles/11175166-get-started-with-custom-connectors-using-remote-mcp),
  [Building custom connectors](https://claude.com/docs/connectors/building),
  [Authentication for connectors](https://claude.com/docs/connectors/building/authentication),
  [Use Claude Cowork safely](https://support.claude.com/en/articles/13364135-use-claude-cowork-safely),
  and [Cowork on Team and Enterprise](https://support.claude.com/en/articles/13455879-use-claude-cowork-on-team-and-enterprise-plans).
  These pages do not show a stable publication date; accessed 2026-08-29.
* OpenAI: [ChatGPT Developer mode](https://developers.openai.com/api/docs/guides/developer-mode),
  [MCP and Connectors](https://developers.openai.com/api/docs/guides/tools-connectors-mcp),
  [Secure MCP Tunnel](https://developers.openai.com/api/docs/guides/secure-mcp-tunnels),
  [Apps in ChatGPT](https://help.openai.com/en/articles/11487775-connected-apps-in-chatgpt),
  and [Developer mode and MCP apps in ChatGPT](https://help.openai.com/en/articles/12584461-developer-mode-and-mcp-apps-in-chatgpt).
  The Help Center displays the latter as “Updated: 7 days ago” and Apps in
  ChatGPT as “Updated: 5 hours ago” at access; neither page prints an absolute
  publication date. Accessed 2026-08-29.

## 3. Security and privacy implications for a school product

### Data has at least three trust boundaries

1. **Our application:** class authorization, teacher role, student records,
   audit events, redaction, retention, and incident response.
2. **The AI host/provider:** Claude/Cowork, ChatGPT, OpenAI API, or another
   client receives whatever the host chooses to put in context and may retain
   conversations, tool traces, or logs according to its plan and policy.
3. **The MCP server and downstream systems:** the server sees authenticated
   requests and tool arguments/results and may call an LMS, storage system, or
   other API. Its retention, subprocessors, region, and breach obligations are
   separate from the host's.

**Provider fact.** OpenAI explicitly warns that a malicious remote MCP server can
exfiltrate anything in model context, recommends trusting official servers,
logging/reviewing data sent to servers, and states that ZDR/data residency do not
control the third-party server after handoff. Anthropic similarly warns that
custom connectors are unverified, may modify/delete data, and may contain prompt
injections. These warnings are in the provider links above and are not replaced
by OAuth authentication.

**Inference/recommendation.** For each request, log a redacted record in our own
system: actor and teacher role, class scope, provider/client, server identity,
protocol version, tool/resource/prompt name, data categories, approval decision,
timestamps, outcome/error, and provider request ID where available. Do not put
raw student answers or names in a general-purpose audit log. Make our audit log
the source of truth; provider Compliance APIs and Cowork local history have
different coverage and retention.

### Prompt injection and tool trust

Resources, tool results, documents, and email can contain attacker-controlled
instructions. A model may follow those instructions even when the teacher asked
for a benign report. An OAuth token proves who authorized access; it does not
prove that tool descriptions/results are safe or that a requested action is
appropriate.

**Minimum controls:**

* Do not treat tool annotations or names as permissions. Enforce authorization
  server-side on every call.
* Keep read and write tools separate and use an allow-list per provider and
  class. Never let a model choose an arbitrary URL, class ID, student ID, or
  downstream recipient.
* For writes, provide a preview/dry-run and require explicit confirmation in our
  UI immediately before execution. Include the exact target, changed fields,
  recipients, and data leaving the product. Do not equate provider “allow
  always” with school approval.
* Use short-lived, audience-bound per-user/per-class tokens. Never accept a
  provider token issued for another resource and pass it through to an LMS or
  storage API. Reject confused-deputy flows and bind any workflow handle to the
  authenticated principal.
* Treat metadata discovery and redirects as SSRF inputs. Require HTTPS in
  production, validate redirect destinations, and block private/link-local/cloud
  metadata addresses where our client performs discovery.
* Rate-limit, paginate, and cap result sizes. Avoid returning an entire class
  roster when a report needs aggregate counts.

### Data minimization and student PII

**Inference/recommendation (conservative default):** The first integration
should send only pseudonymous IDs, class-level aggregates, rubric dimensions,
and the minimum teacher-authored context needed to draft a report. Keep names,
emails, free-form student work, disability/health/family information,
disciplinary records, and precise behavioral traces inside the product unless a
school-approved governance review specifically authorizes the transfer. State
the purpose and retention to teachers/students/caregivers, provide correction
and deletion paths where required, and verify subprocessors, regions, and
cross-border transfers.

Do not infer safety from training commitments alone:

* OpenAI says Business, Enterprise, Edu, and API inputs/outputs are not used to
  train by default, but its API documentation says third-party MCP retention and
  residency remain the third party's responsibility.
* Anthropic says commercial Claude for Work/API inputs/outputs are not used for
  training by default; consumer plans have different controls. Its policy also
  says raw connector content is not included unless directly copied into a
  conversation. Confirm the exact plan, surface (Cowork vs Claude chat/API),
  connector, and contractual terms before relying on that distinction.

## 4. Conservative phased recommendation

### Phase 0: compatibility contract

Publish one small server contract and test it against each target surface:

1. Streamable HTTP over HTTPS, with a legacy HTTP+SSE compatibility path only
   while required by a target client. Negotiate and record the protocol version.
2. Tools limited to `list_class_summary`, `get_assignment_report`, and similar
   **read-only** operations. Use strict JSON Schemas, bounded pagination, and
   stable names. Do not expose arbitrary SQL, file paths, URLs, or generic
   “execute” tools.
3. Resources and prompts are optional in the first cut. If used, expose only
   explicitly scoped class resources and user-visible prompts; do not assume
   OpenAI API or ChatGPT will consume them.
4. OAuth with PKCE, protected-resource metadata, issuer/audience validation,
   short-lived tokens, refresh rotation, and either CIMD plus DCR fallback or a
   pre-registered per-school client. For stdio, use environment credentials and
   a local-process policy rather than sending secrets in arguments.
5. A documented error, timeout, pagination, size, and rate-limit contract.

### Phase 1: read-only teacher reports

* A teacher explicitly connects one provider account and selects one or more
  classes. The server derives allowed class IDs from the authenticated principal;
  never trust a model-supplied class ID alone.
* The UI shows which provider, server, scopes, fields, and class are involved;
  the teacher can disconnect and revoke access.
* Outputs are drafts. A teacher reviews accuracy, evidence, redactions, and
  recipients before publishing or sending. Keep generated reports separate from
  grades, discipline, admissions, or access decisions.
* Measure tool errors, unexpected data volume, prompt-injection blocks, redaction
  misses, teacher edits/rejections, and provider-specific latency/size failures.

### Phase 2: writes only after evidence

Add narrowly defined write tools (for example, save a private draft) only after
read-only operation is reliable. For each write:

* require an explicit, fresh confirmation with a human-readable preview;
* enforce per-class and per-user authorization in the server;
* use idempotency keys, optimistic concurrency/version checks, and an undo or
  compensating action where possible;
* record the approval, exact redacted arguments, target, result, and actor in our
  audit log; and
* keep provider-side approvals enabled for sensitive calls even if our UI also
  confirms. Provider controls are defense in depth, not a substitute for ours.

Do not begin with sends to families/students, grade changes, discipline,
permission changes, deletion, bulk export, or unattended scheduled work.

## 5. Compatibility test matrix and unresolved questions

These questions were **not verifiable from the cited first-party pages** and
must be answered in a sandbox/test tenant before launch:

1. Do Claude and ChatGPT clients accept the 2026-07-28 stateless request metadata
   and header changes, or do they require 2025-03-26/2025-11-25 session behavior?
2. Will OpenAI Responses API and ChatGPT consume our resources and prompts, or
   only `tools/list`/`tools/call`? The Responses guide documents only the latter.
3. Which 2026 MCP features, extensions, notifications, and structured result
   types are supported by each Claude/Cowork and ChatGPT release? Anthropic's
   current builder page explicitly excludes subscriptions, sampling, and
   advanced/draft capabilities; OpenAI's pages do not provide a full primitive
   matrix.
4. Does each target organization permit custom connectors/apps, the required
   OAuth registration mode, refresh tokens, and the needed data regions? Plan,
   role, rollout, model, interface, and geography can change availability.
5. What are the actual per-request timeouts, tool-result limits, rate limits,
   retry semantics, and duplicate-call behavior in the target plan? Anthropic
   documents 300 seconds and approximately 150,000 characters for
   Claude.ai/Desktop; other surfaces need measurement.
6. Which provider and MCP-server logs are exportable, for how long, and with
   what redaction? OpenAI Enterprise/Edu Compliance Logs and Anthropic
   Compliance API/OpenTelemetry have different scopes; local Cowork history has
   special limitations.
7. What contract covers the MCP server's downstream processors, retention,
   residency, deletion, breach notice, and school/student rights? MCP itself
   answers none of these.

## Source register (direct first-party links)

| Source | Publication/version date | Access date | What was used |
|---|---|---|---|
| [MCP Specification](https://modelcontextprotocol.io/specification/2026-07-28) | Revision dated 2026-07-28 | 2026-08-29 | Scope, JSON-RPC architecture, consent/security principles, server/client roles |
| [MCP Architecture](https://modelcontextprotocol.io/specification/2026-07-28/architecture) | Revision dated 2026-07-28 | 2026-08-29 | Host/client/server boundaries, stateless requests, capabilities |
| [MCP Transports](https://modelcontextprotocol.io/specification/2026-07-28/basic/transports) | Revision dated 2026-07-28 | 2026-08-29 | stdio, Streamable HTTP, custom transports |
| [MCP 2026-07-28 changelog](https://modelcontextprotocol.io/specification/2026-07-28/changelog) | Revision dated 2026-07-28 | 2026-08-29 | Removed sessions/initialize, deprecated HTTP+SSE, compatibility changes |
| [MCP Authorization](https://modelcontextprotocol.io/specification/2026-07-28/basic/authorization) and [2025-06-18 Authorization](https://modelcontextprotocol.io/specification/2025-06-18/basic/authorization) | Revisions dated 2026-07-28 and 2025-06-18 | 2026-08-29 | OAuth discovery, PKCE, resource indicators, token audience, CIMD/DCR evolution |
| [MCP Tools](https://modelcontextprotocol.io/specification/2026-07-28/server/tools), [Resources](https://modelcontextprotocol.io/specification/2026-07-28/server/resources), [Prompts](https://modelcontextprotocol.io/specification/2026-07-28/server/prompts) | Revision dated 2026-07-28 | 2026-08-29 | Primitive semantics, capability declarations, annotations and interaction models |
| [MCP Security Best Practices](https://modelcontextprotocol.io/docs/2026-07-28/tutorials/security/security_best_practices) | Page date not stated | 2026-08-29 | Token passthrough, confused deputy, SSRF, local-server and prompt-injection risks |
| [Anthropic: custom remote MCP connectors](https://support.claude.com/en/articles/11175166-get-started-with-custom-connectors-using-remote-mcp) | Page date not stated | 2026-08-29 | Cowork availability, cloud egress, plans, permissions, connector risk |
| [Anthropic: building custom connectors](https://claude.com/docs/connectors/building) | Page date not stated | 2026-08-29 | Supported transports, primitives, unsupported features, size/timeout limits |
| [Anthropic: connector authentication](https://claude.com/docs/connectors/building/authentication) | Page date not stated | 2026-08-29 | DCR/CIMD, PKCE, callback, static headers, no pure client credentials |
| [Anthropic: Cowork safety](https://support.claude.com/en/articles/13364135-use-claude-cowork-safely) and [Team/Enterprise Cowork](https://support.claude.com/en/articles/13455879-use-claude-cowork-on-team-and-enterprise-plans) | Page dates not stated | 2026-08-29 | Cloud/local sessions, approvals, prompt injection, audit/OpenTelemetry, local retention limits |
| [Anthropic commercial data policy](https://privacy.claude.com/en/articles/7996868-is-my-data-used-for-model-training) and [consumer data policy](https://privacy.claude.com/en/articles/10023580-is-my-data-used-for-model-training) | Page dates not stated | 2026-08-29 | Plan-dependent training and connector-content treatment |
| [OpenAI: ChatGPT Developer mode](https://developers.openai.com/api/docs/guides/developer-mode) | Page date not stated | 2026-08-29 | ChatGPT MCP tools, SSE/streaming HTTP, auth choices, write confirmation, read-only annotation |
| [OpenAI: MCP and Connectors](https://developers.openai.com/api/docs/guides/tools-connectors-mcp) | Page date not stated | 2026-08-29 | Responses API remote MCP, connector IDs, approval, allow-list, third-party risks, API retention caveats |
| [OpenAI: Secure MCP Tunnel](https://developers.openai.com/api/docs/guides/secure-mcp-tunnels) | Page date not stated | 2026-08-29 | Private/local server path and limits on public distribution |
| [OpenAI: Apps in ChatGPT](https://help.openai.com/en/articles/11487775-connected-apps-in-chatgpt) | “Updated: 5 hours ago” displayed at access; absolute date not shown | 2026-08-29 | App availability, action controls, workspace defaults, plugin migration |
| [OpenAI: Developer mode and MCP apps in ChatGPT](https://help.openai.com/en/articles/12584461-developer-mode-and-mcp-apps-in-chatgpt) | “Updated: 7 days ago” displayed at access; absolute date not shown | 2026-08-29 | Beta rollout, app publication, frozen snapshots, workspace controls, local-server limitation |
| [OpenAI Business data/privacy](https://help.openai.com/en/articles/8798634-managing-data-sharing-and-privacy-in-chatgpt-business), [Enterprise/Edu training setting](https://help.openai.com/en/articles/8983130-what-if-i-want-to-keep-my-history-on-but-disable-model-training), and [Compliance Platform](https://help.openai.com/en/articles/9261474-openai-compliance-platform-for-enterprise-and-edu-customers) | Business page “Updated: 3 days ago”; Compliance page “Updated: yesterday”; absolute dates not shown | 2026-08-29 | Workspace training defaults, encryption, Compliance API/log retention |
| [OpenAI API data controls](https://developers.openai.com/api/docs/guides/your-data) | Page date not stated | 2026-08-29 | API storage, 30-day defaults, ZDR, residency and third-party MCP boundary |

No application code, tests, or formatters were changed or run; this assignment
produced this Markdown research note only.
