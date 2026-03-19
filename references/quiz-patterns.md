# Quiz Patterns by Chapter Type

## Chapter 1: Overview & Mental Model

**Recall**
- "In one sentence, what does this service do?"
- "What's the tech stack? Name the main language and framework."
- "What's the first thing you'd run to start this service locally?"

**Application**
- "If a teammate asked you what this service is responsible for, what would
  you say?"
- "You're getting a bug report about X. Is this service responsible for that,
  or would you look elsewhere?"

**Reasoning**
- "Why do you think this was built as a separate service rather than part of
  the main monolith?" (if inferable)
- "What surprised you most about how this service is structured?"

---

## Chapter 2: API Surface

**Recall**
- "What HTTP method does the [endpoint] use?"
- "What does a successful response from [endpoint] look like?"
- "Which endpoints require authentication?"

**Application**
- "Write a curl command to create a new [resource]."
- "What would happen if you sent a request to [endpoint] without the
  `Authorization` header?"
- "You want to get all [resources] for a specific user. Which endpoint do you use?"

**Reasoning**
- "Why do you think this endpoint returns a 202 instead of a 200?"
- "The API uses [pagination strategy]. What's a downside of this approach?"
- "Why might the authors have chosen [REST/GraphQL/gRPC] for this service?"

---

## Chapter 3: Data Model

**Recall**
- "What are the main tables/collections in this service?"
- "What's the primary key of the [table] table?"
- "Which fields on [model] are required vs optional?"

**Application**
- "A user deletes their account. Based on the schema, what else would get
  deleted (cascade) vs left behind?"
- "You need to find all [resources] created in the last 7 days. Which table
  and field would you query?"
- "You want to add a `tags` field to [model]. What type would you use and why?"

**Reasoning**
- "Why is [field] stored here rather than in [other service/table]?"
- "The `status` field is an enum. What's the tradeoff vs a string?"
- "There's no soft-delete here — records are hard-deleted. What's the risk?"

---

## Chapter 4: Core Business Logic

**Recall**
- "What triggers the [main workflow]?"
- "Walk me through what happens step by step when [key event] occurs."
- "What happens when [edge case]?"

**Application**
- "You want to add a new [rule/step] to the [workflow]. Where in the code
  would you add it?"
- "There's a bug where [scenario]. Where would you start debugging?"

**Reasoning**
- "Why do you think this logic uses a [strategy pattern / state machine / etc]?"
- "The code retries 3 times on failure. What could go wrong with this?"

---

## Chapter 5: Infrastructure & Ops

**Recall**
- "What environment variables are required to run this service?"
- "How is this service deployed? (Docker? Lambda? Bare metal?)"
- "What queue/broker does this service listen to? What topics/queues?"

**Application**
- "The service is crashing on startup. Where would you look first in the logs?"
- "You need to scale this service horizontally. What concerns would you have?"

**Reasoning**
- "Why is [config value] an environment variable rather than hardcoded?"
- "This service uses [queuing approach]. What failure mode does that protect against?"

---

## General Quiz Tips

- **Always pull examples from the actual repo** — use real endpoint names,
  real table names, real env var names. This grounds the quiz in reality.
- **For wrong answers**: show the relevant code snippet and explain why
  the correct answer follows from it.
- **For reasoning questions**: there's often no single right answer.
  Engage with the user's reasoning — agree/disagree/extend it.
- **Don't quiz on trivia** — quiz on things that matter for day-to-day
  work with the service.
