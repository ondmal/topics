[← Ondřej Maléř · all projects](https://ondmal.github.io)

# Documentation as a source artefact

*How to document software components in the age of AI.*

Documentation is a source artefact: it lives next to the code, changes with it,
and reaches every reader from one source. An AI coding agent makes that
affordable — as a tool doing the mechanical part, with the developer in charge.

**Written:** October 2026.

- **Three audiences** — the development team, the AI agents working on the
  code, and the other roles; one form serves all three.
- **What to write** — not what a reader can easily get from the code, even with
  AI, but the understanding: context, decisions and their trade-offs, business
  rules, contracts, debt, end-to-end topics, guides.
- **How it travels** — it lives in the repository, is written during the
  change, is reviewed in the pull request and is published on merge. Task,
  specification and plan are working artefacts that leave with the task; what
  remains is a new state of the documentation.
- **What keeps it honest** — a few rules an agent can check: status, not
  history; the code is the reference; one fact, one home. What no diff reaches
  — context, requirements, the premises behind decisions — has no such check;
  a reviewer agent catches some of it, people the rest.
- **Where to start** — small: the repository, an index, and the decisions
  nobody wrote down.

## Why documentation goes stale — and why now

Documentation used to be the expensive part of the work, so it was written
late, by someone else, in another tool — and from then on every change to the
code made it a little less true. Readers learned to distrust it and asked the
developer instead, which made writing it look pointless the next time.

An AI coding agent changes the economics. Drafting a page, or updating three
paragraphs to match a changed rule, takes a minute. Checking is as cheap: an
agent can compare documentation with code at any time, and an inconsistency it
finds is simply a bug. The check is not complete, but it is cheap enough to run
every time, as no human check was. What stays scarce is not writing but three
other things: judgement about what deserves to be written, the discipline to
update and check every time, and a way to get the result to people who are
mostly not developers.

None of this is about letting AI write the documentation. The agent is a tool
for the mechanical part, used against the oldest problem documentation has —
going stale — while the people who know the system stay in charge of what it
says. I have put this way of working in place — documentation in the
repository, written with the agent during the change, reviewer agents on every
pull request, publishing on merge — on several backend components, new and
long-existing; three have public pages, linked at the end.

## Three audiences

- **The development team** reads it in the repository and changes it in the
  same pull request as the code.
- **AI agents** read it as context for every task — writing code, tests or
  documentation, reviewing, investigating. An agent takes what it reads as true:
  a stale page is not quietly discounted, as an experienced developer would,
  but acted on.
- **Other roles** — analysts, product owners and managers, testers, support —
  read the published copy. They need the business rules, the contracts and the
  operational knowledge, in their language, and current.

**One form, not two.** A separate version for agents would drift like any other
copy. What makes a document good for an agent — an index that says when to read
what, one subject per file, the point first, the current state only — makes it
good for a busy person too. The difference is the cost of length: an agent pays
for every word on every task, so what is always loaded stays small and the rest
is read on demand. Agents get only a thin layer of their own — how to work here,
what to read before changing what, the rules a reviewer checks — and it points
into the documentation instead of repeating it.

## What is worth writing

**Do not document what a reader can easily get from the code.** Almost
everything is *in* the code; the question is whether the reader can get it out.
A technology's reference — every type and call, like generated API docs — can be
distilled from its source, but nobody learns a technology from its API
reference. To use it you need the rest: how the parts fit together, why it is
built that way, how to do the common things, what goes wrong. That is what
documentation is for. A software
component is no different: a mechanism such as asynchronous command processing
runs through many files, each showing one piece and none the whole.

An AI agent moves the line. Anyone with the code and an agent can get an
endpoint's parameters, or every place a rule is checked, in seconds. What no
agent can extract is what was never written into the code — why it is built this
way, what was rejected, what the business meant. So the question I ask of every
paragraph is: **can this be got from the code easily — by the reader, or by an
agent on their behalf?** At the scale of one line, the same test decides
whether a code comment earns its place.

A mental model helps — not about who writes the documentation, but about what
belongs in it. Imagine an assistant with the code and the documentation in hand,
answering anyone who drives the product: a product manager, support, sales, a
tester, a developer from another team. *What is this business rule? How does it
behave in this situation? Did we decide on A or B, and why?* Such questions
travel through people until they reach someone who knows. What the assistant
can answer from the code alone needs no document — that is how the roles
without the code get the reference. What it cannot, because the answer lives in
someone's head, is a document worth writing. Where the answer is live state — *is this feature switched on in
production?* — the documentation says where to look.

![What goes where: for each paragraph, ask whether it can be got from the code easily, by the reader or by an agent on their behalf; if yes, it is reference that can be distilled from the code on demand and is not written by hand; if no, it is understanding and belongs in the documentation, written once — context and requirements, design decisions, technical debt, business rules, contracts, operational knowledge, end-to-end topics and investigations, and guides for the people who use, extend or integrate](images/documentation-as-code-what-goes-where.svg)

What passes the test:

- **Context and requirements.** A component does not live in a vacuum: where it
  sits, who calls it and what it calls, which business flows it serves, what it
  owns and what it does not. Its requirements belong with that context — the
  business need behind each, who asked and why — or they get met to the letter
  and missed in spirit.
- **Design decisions** — the reason, the premises it rests on, the trade-offs
  (what it gains, costs and gives up), the alternatives rejected, and the date. The code shows what was
  chosen; only the record shows why not the obvious alternative. Without it the
  next team reverses a decision without knowing what it protected, or keeps one
  long after its reason has gone; the date shows a choice was made at a tenth of
  today's traffic.
- **Technical debt**, as a register — what is missing, why it is accepted, what
  it costs, and the threshold at which acting becomes worthwhile. Debt with a
  threshold is a plan; debt without one is a complaint.
- **Business rules** in plain language, one section per concept — the rule
  written from the source, its reason from the people who asked for it, both
  kept in step with the code. This is the reference product owners, testers and
  support actually use.
- **Contracts** — what a consumer may rely on and what it must do. The schema
  can be generated from the code; the obligations cannot.
- **Operational knowledge** — how it is deployed, observed and tested, and what
  an alert means.
- **Topics, end to end** — a mechanism walked through from start to finish, with
  the reasons for its shape. An agent can be asked to trace one at any time, but
  in ordinary work it uses only the context it has: unless told to distil the
  whole first, it changes one piece knowing one piece. A topic in the
  documentation is in its context by default. Investigations belong here too,
  while they describe the current system.
- **Guides for the people who use, extend or integrate** — handbooks, how-tos,
  a troubleshooting index ordered by symptom.

What does not belong is anything that goes stale on the next refactoring: links
to lines of code, code blocks copied from the source, lists of class and file
names. A guide sometimes has to name a type or a file, but a concept is named in
preference to the type that implements it, and a document points at contracts
and headings — never at file paths in other repositories.

## How it travels with the code

Four moves carry the documentation from the first edit to every reader. None is
new on its own; together, with an agent doing the mechanical part, they make
staying current the default rather than an effort.

![Lifecycle: everything lives in the repository; a developer and an AI coding agent write code and documentation together in one change; a pull request is reviewed by peers for the decisions and by reviewer agents for consistency; the change merges as one version and the pipeline renders it; the development team and AI agents read the merged source in the repository, and the pipeline publishes the documentation to a wiki, Confluence or a static site for analysts, product owners, testers and support](images/documentation-as-code-lifecycle.svg)

### 1. It lives in the repository, as text

Text diffs, merges and is reviewed like code, and an agent reads and writes it as
easily. Markdown for the prose; **Mermaid** for diagrams; **SVG** for pictures
that need more design — still text, unlike PNG or JPEG, which a reviewer can only
look at and an agent can only guess from. Formats change; what matters is that
every part is text.

Each component has one **documentation index** — a short page naming every
document and what it is for. The agent's instructions point at it, so the agent
reads the right documents before changing the code they describe, and knows
which to update. The documentation of a release is then simply the
documentation at its commit.

### 2. It is written during the change

A decision is recorded when it is taken; a business rule is updated in the same
change that alters the code enforcing it. The agent takes the mechanical part —
drafting, updating every place a change touches, checking text against code —
because it has the code, the diff and the documents in front of it. The
developer decides, case by case, how much to hand over: most of it for a routine
change, little for a delicate decision. People are the source of what the code
cannot hold — the broader context, why a decision was taken, what the business
meant — and they explain it to the agent. They also set the shape: which
documents exist, what matters in each, what is left out.

A change also produces documents that are not meant to last. A **task
description**, a **specification** and an **implementation plan** are working
artefacts: they live on the branch, and their value ends with the task — what
was true in them has moved into the code and the documentation, and what was not
is misleading. So they leave before the merge: deleted, with the pull request as
the record, or archived out of the way for teams that want the full evidence.
Either way they are never read as the current state.

The specification can be a gate of its own, reviewed in its own pull request
before any code is written: gaps are caught while fixing them means changing a
paragraph, and the code is then checked against something agreed. Whether that
pays depends on the size of the task.

![Working artefacts and the documentation state: on the branch, a task description leads to a specification, reviewed at an optional first gate in its own pull request, then to an implementation plan, then to the change itself — code and documentation — reviewed at the second gate, the pull request; the working artefacts are removed before merge — deleted, or archived aside — and never read as the current state; on the main branch, the documentation moves from its previous state to a new one](images/documentation-as-code-working-artefacts.svg)

### 3. It is reviewed in the pull request

Updated documentation is an acceptance criterion, like tests; a change without
it goes back. The pull request is the one point where the change is small, its
author is present and someone else is looking.

**Reviewer agents** — separate from the agent that made the change, and
suspicious of it — check consistency on every pull request: does the text still
describe the code, does anything break the rules below. Beyond that, running the
check over an area or the whole set is the developer's call. **People** review
what an agent cannot judge — not typos, but whether the decision is right and
the trade-offs honest.

### 4. Merging publishes it

On merge, the pipeline renders the documentation and publishes it where the
other roles read — a wiki, Confluence, a static site. Published pages name their
source and are never edited there — the next publish overwrites them — so a
correction is a change in the repository. Nothing is copied by hand, and the
published version is always the one that ships.

## The rules that keep it honest

Most are short enough for a reviewer agent to check — and a rule an agent can
check is a rule that holds.

- **Status, not history.** The current design, in the present tense; when a
  mechanism goes, its documentation goes with it. History belongs in Git.
- **The code is the reference.** When documentation and code disagree, one of
  them is wrong — usually the documentation, because the code is what runs;
  sometimes the code, and the documentation has just caught a defect. Either
  way the gap is closed, and superseded drafts are deleted, not kept "for
  context".
- **Durable language.** No phases, ticket numbers or "for now" — they mean
  something for a month and mislead for years. The one date that belongs is a
  decision's.
- **One fact, one home.** Written once, linked from elsewhere. Two copies are a
  future contradiction.
- **Self-standing text.** No "as agreed in review", no reference to a rule by
  its number, nothing the reader cannot see.
- **The generic apart from the specific.** An engine apart from its variants, a
  component apart from one consumer's use of it — otherwise the one case reads
  as the only case.

## What no diff reaches

The four moves and the reviewer agents keep honest what is connected to the
code: a rule changes, the diff shows it, the text follows. The most valuable
documents have no such trigger. Context, requirements and the reasons behind
decisions describe the world around the component — the neighbouring systems,
the business flows, the load, the regulation — and that world changes without a
line of code changing here. A decision taken for a system that has since been
replaced reads as current, and an agent acts on it.

This is inherent, and I have no good solution for it. What helps, in part: a
decision names the premises it rests on, so there is something to check at all;
and the reviewer agents, with the operational documentation, the configuration
and the contracts of neighbours in front of them, catch some of what has stopped
being true — some, not all. The rest is on the people who notice the world has
changed, and a pull request with no code in it.

## What it costs, and when not to

- **The review burden moves; it does not vanish.** Pull requests grow, and a
  decision record takes more thought than a rename. Splitting the review keeps
  it affordable.
- **The agent writes too much unless constrained.** The rules, a reviewer agent
  and a concision pass keep the documentation short enough to stay true.
- **Publishing is a pipeline to own**, and refusing edits on the published side
  takes discipline.
- **It takes the whole team.** One pull request merged without its
  documentation is how drift starts.
- **When not to:** a spike, a throwaway prototype, a one-off script —
  documentation that outlives its code is waste.

## Where to start

Not with a tool, and not with all four moves at once:

1. **Produce the first version as a task of its own.** The agent drafts it from
   the code and from whatever old documents still hold true; people review it
   like any other change. Write the index, and point the agent's instructions
   at it.
2. **Write what only people know, first** — context, design decisions, business
   rules. This is what the team and the agent miss most, and what leaves with the
   people who know it.
3. **Make the documentation part of done** — a line in the pull-request
   template, and the habit of sending back a change without it.
4. **Add a reviewer agent with a handful of rules** on every pull request.
5. **Publish on merge** once there is something worth reading outside the team;
   add the specification gate once tasks are large enough for a gap to be
   expensive.

What a component needs varies; the kinds recur. Of the components I documented
this way, three have public pages: the
[Multi-Party Document Signing Service](https://ondmal.github.io/multi-party-document-signing/)
carries a consumer integration guide and explanations of the domain the code
assumes; the
[Document Template Rendering Service](https://ondmal.github.io/document-template-rendering/)
a technical-debt register and a guide for template authors; the
[Account Onboarding Service](https://ondmal.github.io/account-onboarding/) a
business-rules reference and a handbook for building new variants on its engine.

The documents differ; the idea does not. Write what the code cannot tell — enough
that anyone's question about the product could be answered from the code and the
documentation together — keep it next to the code, and let the agent carry the
mechanical part.
