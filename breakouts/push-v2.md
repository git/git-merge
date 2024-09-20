# Push v2 Concepts

Document link: ?

## Auto Rebase Push

- if you push and it can cleanly rebase, server would accept, rebase, return new head and your local is updated
- could merge
- if there is a merge commit, can it rebase that too
- why not client side?
  - less locking

May be implementable by having a mechanism where you can push, the server can do something, then respond with a new sha. Then the client can fetch that data and update the local stuff. This could also work for things like Gerrit where Change Ids are generated and returned. Or it's linted and sent back?

## Commit Cloud

- ability to push to a server where it's not polluting the refs
- push into a namespace?
- lots of refs? (reftable helps?)
- fetches can already limit ref advertisement by namespace

## Resumable Pushes

- all in one go, so any failure makes you try again from scratch
- split a large number of commits into bunches and start pushing from the bottom, then rename the ref at the end
