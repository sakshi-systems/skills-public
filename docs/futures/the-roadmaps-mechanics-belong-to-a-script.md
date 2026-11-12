# The roadmap's mechanics belong to a script

This document describes an undetermined possible future: nothing runs it, nothing is planned,
and the user has not decided it. It holds ideas, not a governing spec, and no other document
leans on it.

A roadmap is two things written in one file. One is its meaning: the phases' intros, their
notes, the task specs, the user's words. That is the distilled knowledge of the project, and it
is text. The other is its mechanics: phase and task numbers, the highest pruned number, the
order, the markers, the checkboxes, the prune's sweeps, the `Spec:` tags, the pull-request
and release markers. That part is a state machine, and the skills describe it in prose for an agent to
execute by hand.

## What the user sees

In the user's words: «мы учим архитектора, эмулировать канбан доску. Бред какой то. Это всё
скриптами делается в базе данных. А не текстом. уже не первый раз эта мысль приходит.
Архитектору некогда думать о структуре проекта, он только и думает, как роадмап оформить.»

A head's attention spent on the format is attention not spent on the structure the project is
turning into. And a rule written for a case the structure already excludes adds text no one
needed. A marker the roadmap's own order already guarantees is not taught to a skill: it is a
script's matter.

## Why not a board

The one reason the user gives for not moving the roadmap into a board: «роадмап и его история
в гите, по которой мы учиться собрались. По канбану, как учиться?» The roadmap's history in git
is the material the [retrospective](../philosophy/retrospective.md) folds, and a board's
database holds its present.

## A script that writes the same file

The answer that keeps both: a script owns the mechanics and writes the same markdown into git.
A phase is added, moved or removed, a task closed, a prune run, each by a command, and the file
it leaves is the file the skills read today. The history stays in git, and the
heads' chronicle keeps what a diff does not: who caught what, and where the user
turned the vector.

What goes to the script is told by one question: does this need judgment? A number, a place, a
checkbox does not. Where to cut a phase, what contradicts the direction, what a finding means
does, and stays with the head. `roadmap-prune`'s emptied-phase sweep deletes a phase's header and intro and leaves a group's `---PR---` marker behind, a defect the script would answer.

The first step would be small: a script beside the one `roadmap-engine` already carries for the
user's slug, taking over one mechanical rule at a time from the skill's text.
