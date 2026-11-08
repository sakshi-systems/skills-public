# Agents on different machines talk as the heads do

This document describes an undetermined possible future: nothing runs it, nothing is planned,
and the user has not decided it. It holds ideas, not a governing spec, and no other document
leans on it.

Each developer's machine hosts its own agents, mostly sessions people open themselves. Today the
only road between two of them is the developer, who copies what one said into the other. This
document describes agents that reach one another directly, and the parts that would make that safe.

## The want

Agents should talk to one another the way the heads in this family already do
([paired-loop](../paired-loop.md)): a head finds a peer through the address file in the peer's
folder and sends it a message, and the peer answers from its own context. The want is the same
past the edge of one repository and one machine: whether the two agents belong to the same
account or not, and whatever system each was born in.

A possible first setting on one machine is [two architect trios as two
hemispheres](two-architect-trios-as-two-hemispheres.md).

The reason lies past any one conversation. When the agents of different people talk, the agent
system of the whole company evolves: a spontaneous upgrade that may come to nothing, or may
strengthen fronts no one can predict in advance. It is the same mixing by which skills loaded
together produce behaviour none of them states, at the scale of teams and their practices. Its
gains appear only as they happen, and they are written into the docs afterwards, which is what the
docs hold: behaviour that no skill states and that is there all the same.

Without it the developer is the slow message broker between his agents. Anthropic's own feature
request says exactly this: https://github.com/anthropics/claude-code/issues/28300 ("developer
becomes a slow, error-prone message broker").

Not everyone wants it. People decline: another person's agent, they say, can read their artifacts
in the store the team shares instead, and what an agent is thinking now does not matter to the
team. What an agent holds and no file yet records is exactly what such reading misses.

## What exists inside one account

Claude Code's cross-session messaging reaches the sessions of the same account and no others;
sessions on other machines are reached through Remote Control. Its semantics are already the
right ones. A peer's message never counts as the user's consent, and the receiver has inbound
controls: accept, hold, refuse.

The rest is what the account boundary leaves out: agents of other people, other machines without
a prepared channel, and agents born in other systems.

## The parts of the system

**A directory.** An agent announces itself when its session starts and withdraws when it ends, by
a hook, the way the user's slug is given to a session today. The team finds each other there, as
heads find each other by their address file.

**A transport between machines.** Something carries a message from one machine to another without
either developer standing in between; what it is and where it lives is told in
[where it would live](#where-it-would-live). Whatever a reader gets back of another person's text
arrives inside an envelope that marks it as data from an unknown author.

**Delivery into the live session.** The message reaches the receiving head's session if one is
running, and otherwise waits and reaches that head when its next session starts. Either way it is
delivered into a session, never left as a file for an agent to happen upon.

**A head of its own receives.** The agent that receives is the musafir, described in
[paired-loop](../paired-loop.md#agents-of-other-systems-reach-ours-through-the-musafir); the transport delivers to it.

**A message is never the user's go.** The receiver raises any decision a message implies to its own
human as a card: who asks, what, and what would change. The sender's message approves nothing on
the receiver's side.

**Talk in a group.** The user decides which head joins which room. A head that speaks in a room
knows that what it says reaches the human of every member. In a room everyone writes, and the voice
of whoever leads the company sets the direction for the teams. A change of the vector is each
developer's to confirm, and the agents carry such a change to their own human themselves. A request
phrased as "ask" a named person reaches that person's own chat directly. The members are real
agents, on the machines of the developers and of those who lead them.

**Adapters for systems that run agents on a heartbeat.** Paperclip
(https://github.com/paperclipai/paperclip) is one: its agents talk through task comments on one
server and resume sessions on a schedule, which is a different model from a live chat. An adapter
turns a message into a task or a comment there, and the reply returns to the mailbox.

## A candidate core

openmsg (https://github.com/marciob/openmsg, MIT) already holds most of this shape:
- nothing is published by default;
- a new sender's first message stays outside the model until the owner accepts the sender;
- each message carries a header saying it is from another agent and approves nothing;
- a path recorded in the message stops loops;
- the envelope is sealed end to end through a relay;
- addresses look like `claude:agent@person`;
- delivery goes through the Claude Code session's own inbox socket, so Claude Code's inbound
  controls apply.

Its own caveat is "No team has run it across the internet yet". It is young, and its code is to be
read whole before anything relies on it.

## A model for memory

MCP Agent Mail (https://github.com/Dicklesworthstone/mcp_agent_mail) keeps identities, inboxes
and history, in git and SQLite. That repository is deprecated in favour of a Rust successor, and
both carry a licence rider that withholds every right from AI companies and from anyone acting for
them. It stays a model for what a conversation between agents leaves behind, and for where it
lives: a design to read, never code to take.

## What would be the family's own layer

What the candidates do not give, and the family would:
- the head that receives: a head of its own, always writable, where messages park;
- the delivery bridge from the transport into the receiving head's session, as
  [where it would live](#where-it-would-live) places it;
- the single decision card, the same form wherever a message asks for a decision;
- the directory and the conversation history kept in git, as part of the family's memory;
- the adapters for systems with another rhythm.

## Three roads an agent of another developer can take today

Take one case. Another developer's agent reviews the user's pull request. It does not look into
the shared store where the work's roadmap and specs sit, because nothing points it there: it reads
the diff and the description, and the store is outside both. Three roads need little or nothing that
does not exist, and each lies where its reader already looks.

**The pull request's description.** The description names where the work's roadmap and specs live
in the shared store. A reviewer reads the description anyway, so an address placed there is met at
the moment the decision to look is made, while an instruction kept away from that moment does not
act ([instruction-in-data](../instruction-in-data.md),
[principles-at-their-moment](../philosophy/principles-at-their-moment.md)). It needs no one's
consent.

**The author is the one to ask.** A pull request arrives with its author attached, so the question
a reviewing agent naturally has goes to whoever made it, and the author's receiving head, the head
of its own among [the parts of the system](#the-parts-of-the-system), answers about the work. It is
the road the heads of this family already take, to the one whose work lies in front of the asker.

**The thread.** A team that already accepts an automatic reviewer answering in the threads of pull
requests has accepted an agent speaking there. A head that answers a review question in the thread
uses a channel already consented to, so no protocol has to be argued for first.

**A road that already runs elsewhere: the change itself.** One team the user saw works this way
already: a pull request is reviewed by the agents of the people it touches, so whatever the author
wants such an agent to consider travels inside the pull request. Identity, addressing, consent and
history come from the forge and the team's review practice, and no transport of its own carries any
of it. The pull request is the one door that team leaves its agents to meet by.

It is one intent with the protocol this document describes, reaching an agent on another person's
machine, at a different level, and the two meet at a seam of the architecture: the same purpose, two
protocols, and neither replaces the other. The change door needs a change to hang on. The direct
door, with its mailbox and its bridge, is for a conversation that someone starts with an agent on
another machine when there is no change to carry it, and it is the one this document holds as a
possible future.

The argument that agents should talk to one another does not land with a team whose agents each
live one session and hold no identity: the value of asking an agent appears only once it remembers
and has a role, and until then there is nothing in it to ask.

## Where it would live

The protocol lives inside the family's own system, between developers who use it, each with a
[musafir](../paired-loop.md#agents-of-other-systems-reach-ours-through-the-musafir). The transport
is the family's own long-running service, the one described in
[a monthly analysis sprint, and the service that holds the environment](a-monthly-analysis-sprint-and-the-service-that-holds-the-environment.md):
a mailbox between the musafirs of different developers. It keeps read copies of the repositories it
watches and an episodic memory with search, so the memory of conversations is something it is
shaped to hold. The piece still to be built is the bridge from that service, on a server, into a
head's live session on a developer's machine. The same bridge is what the service needs to hand the
user's own task to a head, so one bridge serves both. The candidate core named above still holds the
envelope.
The mailbox takes any agent of any system, not only those that run in the family's own system.
Registration is behind a password, and which of a developer's projects a registered agent reaches is
that developer's own choice, per project.
