# attention-at-the-moment-of-decision — what agents build is set by where their attention points when they decide

The quality of what agents build comes from where their attention points at the moment a decision is
made — what they see and what they ask of it — and not from the volume of planning laid before that
moment. The family's skills, docs and maps are, taken together, a way of aiming attention: they
decide what is in front of an agent when a choice is made and which question it puts to what it sees.

## More planning does not raise the code

A spec can pin more and more of a task: every edge named, every value fixed, every check described.
Tasks written that way pass their reviews, and the code they produce stays as procedural as it was
before: a branch on a kind where the design holds an engine, a flag read by an `if` after the object
exists. The planning made the task land; it did not change what the author of the code was looking at
when it chose the branch.

The user's position is plain: «Наращивать мощь планирования, я больше не хочу». And what he wants
in its place: «я хочу что б агенты писали код лучше меня! На недостижимом для меня уровне, и я знаю,
что они так могут.» Agents already know the principles that make code good
([principles-at-their-moment](principles-at-their-moment.md)); what they lack is not instruction but
the right view at the right time.

## Attention has two parts

At a decision, attention is two things: **what is in view**, and **the question asked of it**.

A question with nothing in view asks into a void. A lens that fires when a kind gains its second
member has nothing to be asked of in a project whose map names no kinds
([names-and-reasons-not-laws](../names-and-reasons-not-laws.md), "A question asked at its moment works
only if the map names what it asks about"). And what is in view with no question put to it is read and
not used: the agent has seen the whole picture and decides as if it had seen a corner.

Quality is the two together. Widening the view without asking, or asking without a view, leaves the
decision where it was.

## The decision points and their vectors

Each point at which the work is decided has its own view and its own question.

**The outline, the deepening and the decomposition** decide what the roadmap holds, what a phase
needs and how it is cut. Their view is the roadmap wide — its headings and the phase intros, the
compact layer through which the whole is seen — so that what other phases build bears on the phase in
hand ([the cycle's outline step](../sakshi-harness/skill-cycle.md#phase--roadmap-outline)). A cut made
with one phase in view and the rest out of it is made without knowing what the rest will multiply.

**The plan** is where a task first has a visible code surface. Everything before it, the roadmap
and the specs, is a picture of how the thing will be; in the plan it shows how it is about to be
built. That is the moment to read the plan against the architecture the project declares, whatever
that architecture is, and to ask whether the plan follows it. This is the direction the family moves
in — a plan review that gives each vector of attention its own reader — and not a thing already
built. The declared architecture says where not to abstract as well as where to: a plan that puts an
interface over a single implementation departs from it as much as one that branches on a kind, and
both are the same failure. The measure already lives in
[polymorphism-philosophy](../../src/skills/polymorphism-philosophy/SKILL.md), which fires only when a
kind gains its second member, never on style. The user: «я считаю во всём нужен баланс. И в плане
архитектуры тоже можно уйти в фанатизм, где каждый класс станет абстракцией. Такой код я тоже видел,
его поддерживать порой ещё сложней , чем код на ифах. Но мы пока что плавно и постепенно движемся к
точке баланса. Где движок строится на инжекшенах а не на ифах..»

**A principle** is carried as one question asked where the decision it governs is being made
([principles-at-their-moment](principles-at-their-moment.md)). That is this doc's claim at the scale of
a single principle: the knowledge is the agent's, the moment and the view are the system's.

## Why it was missed

The family's grounding discipline teaches depth along one branch, to the leaf: a contract line is
read with its spec, a spec with the code it names ([context-tree](context-tree.md)). For code that is
right, because the meaning is in the leaf. The same narrowing was carried onto the roadmap, where the
meaning is in breadth: how the phases relate, where the project goes, what one phase builds that
another multiplies.

So the heads read a list of headings and one phase's block, never the whole, and held the direction
from memory. The roadmaps were long on purpose, to give an agent perspective, and the perspective did
not arrive. The user's own account of it: «Я понял, что роадмап я пишу, но агент остался слепым».
The remedy is not a longer roadmap but a layer short enough to be read whole at the moment it is
needed, which the roadmap already has in its headings and intros.

## What this asks of a text

A durable text aims attention when it names what is to be in view and the one question to ask of it,
and stops there. A text that instead states the answer in advance is the law
[names-and-reasons-not-laws](../names-and-reasons-not-laws.md) describes, and it fails the same way: it
decides before the case is known. Before adding a line to a skill, a spec or a doc, the question is
what the reader will have in front of them when the decision comes, and what they will ask of it.
