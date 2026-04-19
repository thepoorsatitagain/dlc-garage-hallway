# dlc-garage-hallway

HYDRA / OPENCLAW ARCHITECTURE
Working Papers: The Garage, The Digital Library Card, The Hallways Protocol
Robert Leroy Bates Jr. — Kalamazoo, MI
April 2026
Recovered from conversation history — March 24, March 27, April 4, April 13, 2026
RULE ZERO: Append-only. Nothing deleted. Everything versioned.
These papers describe ideas that are well-developed but not yet implementation-ready specifications. Open questions are documented in each paper.
 
WORKING PAPER 01
The Garage: A Capability Operating System for AI Agents
Author: Robert Leroy Bates Jr. — Kalamazoo, MI
Date: April 2026
Status: Working paper — recovered from conversation history
Rule Zero: Append-only. Nothing deleted. Everything versioned.

Abstract
Current AI agent deployments conflate three independent concerns: who the user is, what the agent is authorized to do, and which actions it can take in the world. The Garage is a capability operating system that separates these concerns into three independently managed layers — Keys, Vehicles, and Destinations — governed by an enforcement corridor that the agent cannot see or bypass. This paper describes the model, its components, and the reasoning behind its architecture.
The Analogy
A key does not drive a car. A car does not decide where you go. You decide where you go, you choose which car to take, and you use the key that starts that car.
That is the Garage model.
The user holds keys. Keys start vehicles. Vehicles have authorized destinations. The agent is the car — capable of reaching destinations — but it does not choose which car it is, and it cannot reach destinations that are not on its vehicle's authorized list.
Changing your destination does not change which vehicle you are in. Changing your vehicle does not change your keys. They are independent concerns, managed separately, enforced at different layers.
The Three Layers
1. Keys (DLC Tokens)
A key is an assembled DLC credential. It proves who you are, what AI you are using, and what entities you are affiliated with. It does not define what the agent can do — that is the vehicle's job. The key authorizes you to start a vehicle. The vehicle defines where you can go.
Keys are assembled from PIN components at the moment of use, held in memory only for the duration of the session, and then discarded. The agent never sees the key.
2. Vehicles (Capability Profiles)
A vehicle is a named capability profile. It contains four things:
•	Personality — how the agent behaves, its tone, its defaults, its guardrails
•	Permissions — which destinations are authorized for this vehicle
•	Context domain — what kind of work this vehicle is scoped to (personal, work, medical, etc.)
•	Memory boundary — what the agent can remember within this profile and cannot carry across profiles

A user might maintain several vehicles. A home vehicle that opens personal files and browsing. A work vehicle that opens email, calendar, and work documents. A locked vehicle for high-sensitivity tasks. A paranoid vehicle for anything that matters.
The agent does not choose the vehicle. The user selects the vehicle before the session starts. The agent operates inside the vehicle's constraints without knowing what those constraints are. It simply finds that certain actions succeed and others fail.
3. Destinations (Governed Actions)
A destination is a concrete, atomic thing the agent is allowed to do: read files, send email, browse the web, execute code, access the camera, write to calendar.
Destinations are the vocabulary of capability. The Garage defines this vocabulary. The vehicle selects which destinations are active. The Access Corridor enforces it mechanically.
The agent cannot reach a destination that is not on its vehicle's list. The vehicle cannot override this. The agent cannot override this. The corridor enforces it.
The Full Stack

┌─────────────────────────────────────────────────────────┐
│                    THE GARAGE (UI)                      │
│  Keyring panel, vehicle editor, corridor status,        │
│  denial log, vault browser, approval queue              │
├─────────────────────────────────────────────────────────┤
│              VEHICLE LAYER (capability profiles)        │
│  Named profiles: personality + permissions +            │
│  context domain + memory boundary                       │
├─────────────────────────────────────────────────────────┤
│           ACCESS CORRIDOR (enforcement)                 │
│  Cage boundary, destination checks, three-mode logging, │
│  vault mediation, DLC claim verification                │
├─────────────────────────────────────────────────────────┤
│              DLC — DIGITAL LIBRARY CARD                 │
│  Keys, keyring, claim assembly, PIN model               │
├─────────────────────────────────────────────────────────┤
│              EGL — EGRESS GOVERNANCE LAYER              │
│  Outbound gate, identity enforcement, replay            │
├─────────────────────────────────────────────────────────┤
│           GEL — GOVERNANCE ENFORCEMENT LAYER            │
│  Inbound gate, policy, envelopes, telemetry             │
├─────────────────────────────────────────────────────────┤
│              VCKB — KNOWLEDGE LAYER                     │
│  Versioned, attributed, governed knowledge              │
└─────────────────────────────────────────────────────────┘
                          ↕
              AI agent runtime (lives in the cage)

Practical Examples
Simple personal use
personal PIN  →  Home vehicle  →  read files, browse web, read email
Work context
personal PIN + work entity PIN  →  Work vehicle  →  + send email, calendar, work files
High-security mode
personal PIN  →  Locked vehicle  →  read files only, nothing else
Licensed professional
personal PIN + employer PIN + professional license PIN + union PIN
  →  Licensed Professional vehicle
  →  access to licensed VCKB modules + professional tools + employer systems

Glossary

Plain English	System Term	What it is
Key	DLC token	Cryptographic credential assembled from PINs
PIN	PIN component	4-16 character secret that unlocks one claim
Vehicle	Capability profile	Named bundle of personality + permissions + context + memory
Destination	Governed action	A concrete thing the agent is allowed to do
Garage	Permission Portal	Where profiles are managed and selected
Corridor	Access Corridor	The enforcement layer that checks destinations
Cage	Agent boundary	The wall between the agent and the outside world
Vault	Append-only revision store	Version history the agent can add to but never edit
Keyring	Key presets	Named pre-assembled DLC configurations for common contexts

Open Questions (as of April 2026)
•	Can users switch vehicles mid-session, or only at session boundaries?
•	How is personality enforced at the implementation layer — configuration object, system prompt fragment, or something else?
•	How is cross-profile memory bleed prevented technically when the same underlying model is used?
•	Are work and home vaults the same store with different key access, or physically separate?
•	When a vehicle config changes, should prior DLC tokens reference the version that was active when they were issued?

Recovered from conversation history — March 24 and April 4, 2026
Part of the Hydra / OpenClaw governance architecture
See also: Paper 02 (DLC Multi-PIN Architecture), Paper 03 (Hallways Protocol)
 
WORKING PAPER 02
The Digital Library Card: Multi-PIN Authority Architecture
Author: Robert Leroy Bates Jr. — Kalamazoo, MI
Date: April 2026
Status: Working paper — recovered from conversation history
Rule Zero: Append-only. Nothing deleted. Everything versioned.

Abstract
Authentication systems collapse three distinct concerns into one credential: who you are, what you are authorized to do, and who you are affiliated with. The Digital Library Card (DLC) separates these concerns into independently held PIN components that are assembled on demand into a session key, used once, and discarded. No single component holder possesses a complete credential. The assembled key exists only in memory during the session it governs. This architecture simultaneously solves authentication, capability authorization, economic attribution, and liability chain establishment in a single cryptographic operation.
The Name
A library card does not give you the library. It gives you access to specific sections, specific materials, for a specific duration. You cannot take books from the restricted collection. You cannot use someone else's card. The card does not persist — it is checked at the door each visit.
The Digital Library Card extends this model into AI agent authorization. Your DLC is assembled from components you hold, institutions you belong to, and AI systems you are authorized to use. It grants access to specific VCKB knowledge modules and specific agent destinations. It is assembled when needed and discarded when done.
The PIN Component Model
DLC is not a single token. It is a set of PIN components assembled into a key at the moment of use. There are three types of component, each of which may appear in variable quantity.
Model PIN
Identifies which AI system, which version, under which governance profile. Carries the liability information for that AI — what regulatory constraints it operates under, what safety envelope it has been certified to, what content policies govern it. Each AI model has its own PIN. Claude has a PIN. GPT has a PIN. If you are authorized to use multiple models, your keyring may include tokens assembled with different model PINs for different contexts.
Personal PIN
Identifies the user. Contains personal identity, personal VCKB permissions, payment rails, usage history, and personal liability handshake records. This is your identity layer — not your institution's identity, not your employer's identity, but yours specifically. The personal PIN is the one component that is present in every valid DLC token.
Entity PIN
Identifies an institutional affiliation. Your employer. Your university. Your professional licensing body. Your union. Your medical practice. Each entity holds its own PIN, which encodes what modules it has licensed, what capabilities it authorizes for its members, what ceiling it places on AI usage, and what liability it absorbs at the institutional layer.
A user may carry multiple entity PINs and include different combinations in different keys on their keyring. A key assembled with your employer PIN opens employer-licensed content. A key assembled with your medical license PIN opens medically-licensed content. The same person, different assembled keys, different access levels.
The Banking Analogy
The DLC is three-dimensional where a bank routing number is two-dimensional.
A bank routing number identifies the institution. An account number identifies you within that institution. Together they authorize a transaction.
The DLC adds a third dimension: the AI model identity. The routing number is your employer or institution. The account number is your personal identity. The AI model PIN is the third axis — which AI you are using and under what governance profile.
All three are required for the credential to be complete. A DLC assembled with only a personal PIN and no model PIN cannot authorize an AI interaction. A DLC with no entity PIN cannot authorize access to entity-licensed content. The completeness requirement is what makes the credential meaningful rather than just another access token.
What the DLC Collapses
A DLC token, when assembled and presented, simultaneously accomplishes all of the following in a single cryptographic operation:
•	Proves who you are (personal PIN)
•	Proves what AI you are using and its governance profile (model PIN)
•	Proves what entity you are affiliated with and what that entity has licensed (entity PIN)
•	Authorizes micropayment rails for VCKB content attribution
•	Opens paywalled knowledge modules the entity has licensed
•	Opens locally stored password-protected files within your access tier
•	Tracks usage for author compensation in the VCKB marketplace
•	Establishes the liability chain for this interaction

Without the DLC, each of these would require a separate handshake. The VCKB marketplace cannot function at scale with per-transaction authentication. The DLC is what makes it practical.
The Keyring
The user does not enter all their PINs every time. They maintain a keyring of pre-assembled DLC configurations — named keys that unlock different combinations of corridor checkpoints.

Key Name	Claims Included	What It Opens
Home Key	personal + Claude	Files (read), internet, basic tasks
Work Key	personal + Claude + work entity	Files (read/write), email (read/send), calendar, internet
Full Key	personal + Claude + all entities	Everything the profile allows
Read-Only Key	personal only	Read operations only, no writes or sends
Medical Key	personal + Claude + medical entity	Health files and medical APIs only
Paranoid Key	personal (16-char) + all entities	Full access, highest assurance level

The keyring lives with the user. The user grabs the key that fits the session. One unlock. The right checkpoints open.
The Combination Lock Model
The DLC token is not assigned per door. It is a profile — a signed keyring of claims. Each checkpoint in the Access Corridor is a combination lock. It checks which claims are present in the token. More sensitive checkpoints require more claims.

Checkpoint	Claims Required	Plain English
Main gate	personal	Just your personal PIN
Files read	personal + model	You + which AI is asking
Files write	personal + model + work	You + AI + your work entity
Email read	personal + model	You + which AI
Email send	personal + model + work	You + AI + your work entity
Shell/exec	personal + model + work + school	Four claims required
Nuclear option	all claims in profile	Every PIN you have

The agent does not know this table. The agent hits a wall. The corridor checks the claims silently. The agent gets a result or a denial. The credential structure is invisible to the model.
Security Properties
•	No single component is a complete credential — possession of one PIN does not compromise the token
•	Assembly happens in memory only — the assembled token is never persisted to disk
•	The agent never sees the credential — the model cannot inspect, copy, or transmit it
•	Entity PINs are held by institutions — a user cannot self-issue entity claims they have not been granted
•	Model PINs are held by AI providers — a user cannot substitute an uncertified model without a valid model PIN
•	Every transaction is attributable — the liability chain is established cryptographically, not by policy

Relationship to the VCKB Marketplace
The DLC is what makes the Versioned Cryptographic Knowledge Base marketplace viable at scale. Without it, every content access requires a separate authentication handshake and a separate payment authorization. With it, the assembled token carries all required authorizations simultaneously. The author receives attribution. The entity is billed correctly. The usage is logged against the correct identity. The liability chain is established. All in one operation.
This is the economic spine of the system. The Garage manages the vehicle. The Hallways enforce the corridor. The DLC makes the economic layer work.
Open Questions (as of April 2026)
•	Where does the keyring live? Options: encrypted local file, OS keychain, hardware security token, USB dongle for air-gapped scenarios.
•	What is the valid duration of an assembled token? Should tokens expire within a session, or at session end?
•	How are entity PINs provisioned? Who holds the entity PIN authority and how do they issue it to affiliated members?
•	How does the model PIN get certified? What body certifies that a given model version meets the governance profile its PIN claims?
•	First-run UX: what does the simplest possible setup look like? (Target: one command, one PIN, operational.)

Recovered from conversation history — March 24 and March 27, 2026
Part of the Hydra / OpenClaw governance architecture
See also: Paper 01 (The Garage), Paper 03 (The Hallways Protocol)
 
WORKING PAPER 03
The Hallways Protocol: Enforcement Architecture for AI Agent Boundaries
Author: Robert Leroy Bates Jr. — Kalamazoo, MI
Date: April 2026
Status: Working paper — recovered from conversation history
Rule Zero: Append-only. Nothing deleted. Everything versioned.

Abstract
AI agent governance based on rules given to the model in a system prompt fails for three reasons: the model can reason around rules, rules drift as context grows, and compliance cannot be audited. The Hallways Protocol replaces rules with architecture. Three enforcement layers — the Governance Enforcement Layer (GEL), the Access Corridor, and the Egress Governance Layer (EGL) — form a complete governed boundary around the agent process. The agent reasons inside the cage. Everything else is outside it. The hallways are how you get from one to the other, and the hallways have guards.
Why Rules Fail
The standard approach to AI agent governance is to write rules into the system prompt. Do not access files outside the project directory. Do not send email without confirmation. Do not execute shell commands.
This approach has three structural failures:
•	Models can reason around rules. A sufficiently capable model can find edge cases, exceptions, and framings that technically comply with the letter of a rule while violating its intent. Rules are language; language is the model's native domain.
•	Rules drift. As conversation context grows, system prompt instructions become less salient. Compliance becomes probabilistic over the course of a long session.
•	Rules cannot be audited. There is no record of whether the agent followed the rules in a given session. The user cannot verify compliance. The institution cannot prove accountability.

The Hallways Protocol replaces rules with architecture. The agent does not choose to comply. The cage, corridor, and egress layer enforce compliance mechanically. Compliance is not a property of the agent's behavior — it is a property of the system design.
This is the same logic as physical security. You do not rely on employees following the rule 'do not enter the server room.' You put a lock on the door. The architecture enforces the constraint.
The Three Enforcement Layers
GEL — Governance Enforcement Layer (Inbound Gate)
GEL sits at the entry point to the agent. Everything that enters the agent context — prompts, tool results, injected content, retrieved documents — passes through GEL first.
GEL enforces:
•	Policy compliance on inbound content
•	Execution envelope constraints (what the agent is permitted to reason about in this session)
•	Telemetry initiation for the session
•	Rejection of content that would violate governance constraints before it reaches the model

The agent does not know GEL exists. The agent receives content that has already been filtered. Violations do not produce errors the agent sees — they are absorbed at the gate.
Access Corridor (Cage Boundary)
The Access Corridor is the enforcement layer at the cage boundary. Everything the agent attempts to do — read a file, send a request, invoke a tool, write to a location — passes through the corridor before it reaches the outside world.
The corridor operates as a combination lock system. Each destination requires specific DLC claims to be present in the active token. The corridor checks the token. If the claims are present, the action proceeds. If they are not, the action is denied, logged, and surfaced to the appropriate tier.
Three logging modes operate simultaneously:
•	Tier 1 — expected behavior, passes through, logged at standard priority
•	Tier 2 — flagged behavior, logged at elevated priority, surfaced in denial log
•	Tier 3 — blocked and escalated to human decision queue

The agent does not know the corridor exists. The agent observes only that actions succeed or fail. It cannot determine the reason for a failure from inside the cage.
EGL — Egress Governance Layer (Outbound Gate)
EGL sits at the outbound channel from the system. Everything the agent sends to the outside world — API calls, emails, web requests, any outbound communication — passes through EGL before it leaves.
EGL enforces:
•	Identity verification: outbound communications carry the correct DLC-attributed identity, not an assumed or self-asserted identity
•	Content compliance: outbound content is checked against governance constraints before dispatch
•	Replay binding: outbound actions are recorded to the vault in a form that the Deterministic Replay Engine can reconstruct
•	Liability attribution: each outbound action is tied to the DLC token that authorized it

The EGL is the reason the agent cannot impersonate. The agent may generate content that appears to be from a person. That content passes through EGL before it reaches any recipient. EGL stamps the actual identity.
The Cage
The cage is the name for the governed boundary that the three enforcement layers collectively create. The agent process lives inside the cage. The outside world is outside it. The hallways — GEL, Corridor, EGL — are the only paths between them.
The cage has three properties:
•	Nothing enters without GEL approval
•	Nothing the agent does reaches the outside world without Corridor authorization
•	Nothing leaves the system without EGL verification

These three properties together close the governed boundary completely. There is no other path. The agent cannot construct one.
The Vault
The vault is the append-only record of what happened inside and at the boundary of the cage. Every action the agent attempts, every denial the corridor issues, every outbound dispatch EGL authorizes — all of it goes to the vault. Nothing is deleted. Nothing is edited. The vault is the audit trail.
The Deterministic Replay Engine (DRE) can reconstruct any session from vault records. Given the vault entry for a session, it can reproduce the exact sequence of agent actions, corridor decisions, and EGL dispatches that occurred. This is what makes the system auditable in a legally meaningful sense.
The Hallways as PALC
The Access Corridor and EGL are, in the broader framework, an implementation of Digital PALC — pattern-aware continuity detection at the boundary.
Every action the agent attempts is checked against the authorized pattern for this vehicle. Actions that fit the pattern — destinations the vehicle is authorized to reach, content that complies with the vehicle's policy — pass through. Actions that deviate from the authorized pattern are denied, logged, and surfaced.
The Hallways Protocol is PALC running on agent action space rather than on visual sensor data or document boundaries. Same mechanism, same logic, different substrate.
Relationship to the Full Stack

[client] → GEL :3100 → OpenClaw :3000
                           ↓
                    Access Corridor
                    (cage boundary)
                           ↓
              [files, email, internet, shell...]
                           ↓
                        EGL :3210
                    (outbound identity)
                           ↓
                       [internet]

GEL governs what comes into the agent. The Access Corridor governs what the agent can touch. EGL governs what leaves the system with which identity. All three are required for full governance. Any one alone is insufficient.
Without the corridor, the vehicle model is just a naming convention. Without EGL, the cage has an open outbound channel. Without GEL, the agent can be manipulated through crafted input. The three layers are interdependent.
Relationship to the Garage and DLC
The Hallways Protocol is what makes the Garage meaningful. The Garage is where the user manages vehicles, keys, and destinations. The DLC is the credential that authorizes access. The Hallways are what enforce it.
•	Garage — the user manages vehicles, keys, and destinations here
•	DLC — the credential that proves who the user is and what they are authorized to be
•	GEL — the inbound gate: nothing enters the agent context without governance approval
•	Access Corridor — the destination check: nothing the agent does passes without vehicle authorization
•	EGL — the outbound gate: nothing the agent sends passes without DLC authorization
•	Vault — the immutable record of what actually happened
•	DRE — the replay engine that can reconstruct any session from vault records

Open Questions (as of April 2026)
•	Shell/exec checkpoint: should this default to open-with-logging or locked-by-default even in simple mode?
•	Denial behavior: silent deny, logged deny, or real-time prompt to user? May differ per checkpoint.
•	How does EGL handle streaming responses — is the identity stamp applied to the stream or to the completed response?
•	What is the formal checkpoint taxonomy? The complete list of destinations needs a definitive enumeration before implementation.
•	How does the vault handle sessions that are interrupted or never properly closed?

Recovered from conversation history — March 24 and April 4, 2026
Part of the Hydra / OpenClaw governance architecture
See also: Paper 01 (The Garage), Paper 02 (The Digital Library Card)
