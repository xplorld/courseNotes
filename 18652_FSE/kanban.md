# kanban

Kanban (看板) is a tool to manage workflow.

## description

we have many productions or features in different status. Organize them, grouping by status. The resulting table is kanban.

| backlog | test design | implementing | testing | deploy | online (maintain) |
| -- | -- | -- | -- | -- | -- |
| A, B | C | D, E | F | G | H, I, J |

**A kanban workflow is a number of priority queue (with each own size limit).**

The point is:

- monitor everything in same table
- limit # of items in each stage
- explicit status change: When a stage is not full, **pull** one item from prev stage and do it. When one is congested, prev stage items must wait.

## roles

### Manager

- compose new items into first stage
- dynamically change priorities of all items
- schedule man into stages

### Worker

- assigned into a stage
- work in priority
- report congestion