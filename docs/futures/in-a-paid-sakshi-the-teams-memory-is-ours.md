# In a paid Sakshi the team's memory is ours

This document describes an undetermined possible future: nothing runs it, nothing is planned, and the user has not decided it. It holds ideas, not a governing spec, and no other document leans on it.

A team that builds a project keeps a memory of its own: the heads' buffers and snapshots, the records of its runs, the history it compacts. Whether the roadmap and the task specs belong to it too is left open below. Whose that memory is, and who may look into it, has one answer today and would have another if Sakshi were sold.

## What the user sees

In the user's words:

«с другой стороны - у строительной команды своя память.. и то, где память лежит, зависит от того, как проект этого требует. То есть наша строительная память не нужна сакши строителям, которые живут на сервере.. Интересный слой смыслов вскрывается.»

«тут ещё одна дыра. Если это хорсис - я хочу видеть эту память, значит это мой репо. Если сакши запускают на стороннем репо, это уже их дело, хотят они смотреть в память или нет.. А если мы захотим сакши сделать платным, нам вообще память нельзя будет показывать..»

«платную версию в будущее.»

The last line is the ruling that orders this document: the paid version waits.

## The operator owns the memory's place and visibility

The memory is the operator's, and the operator is whoever runs the team on a project. On the user's own family of repositories that is the user, and he wants to see the memory, so it lies in his repository. On a third party's install it is that party, who decides whether the memory is read at all. In a paid Sakshi it is the family: the builders live on a server, the memory is the product's working state, and showing it to the client is not something the family could do.

Today this is already one mechanism. The memory is `.ai-factory`, and it sits in the project's repository or is linked into another one, as the operator lays it. The same folder serves all three cases; what differs is whose repository it is in and who has the key to it.

## The shape for the paid case

One alternative is a second root beside `.ai-factory`: the project's knowledge under one root, the team's memory under another. Every skill path would then have to know which root it lives under, a mapping nobody holds. The shape that needs no such mapping is smaller. The project's two knowledge files, `ARCHITECTURE.md` and `RULES.md`, move beside the project's `docs/`, where the project already keeps what describes it. Then `.ai-factory` holds the memory alone, and where that memory lies stays one link, wherever the operator puts it.

## The fork left for the user

Two questions stay open, and nothing here answers them.

**Whose are the roadmap and the task specs in that case?** They are the team's working memory, and they are also what the client paid to have built. Whether they sit with the memory the client never sees, or beside the project the client owns, is not settled.

**What becomes of `## Features`?** In `ARCHITECTURE.md` it holds the team's compacted history, anchored to commits, and it stands beside the module map, which is the project's. With the two files moved beside `docs/`, one file would carry the team's history and the project's structure together. `roadmap-prune` is where the team's history flows to the client: it folds closed work into `## Features`. Whether that flow is wanted, and where the history lands if it is not, is the user's to decide.
