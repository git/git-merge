# Branching in a Monorepo

## Background

Some organizations have oscillated between using a monorepo approach and using separate repos for each product.  This suggests that the trade-offs between monorepos and non-monorepos may not be suitable for everyone.

In this discussion, we considered a specific case:

An organization has 6 teams, each working on their own component.  Teams A, B, and C work on libA, libB, and libC.  Teams X, Y, and Z work on appX, appY, and appZ.  AppX uses all 3 libraries.  AppY and appZ use only libA.

The organization uses a monorepo with a top-level structure like:

```
    monorepo/
        libA/
        libB/
        libC/
        appX/ # depends on libA, libB, libC
	appY/ # depends on libA
	appZ/ # depends on libA
```

This structure allows all the teams to continue working on trunk, and their build system handles the dependencies.  Team A are working on brand new features for their library that will be important in the long-term.  Team Z have a critical release coming up.  It does not require the new features that team A are working on, and it is vital that inadvertent breakages from developing those features do not impact AppZ in any way.  Team Z would like to somehow "freeze" the version of libA that they use until the release.

In a non-monorepo scenario, team Z would avoid updating their dependent version of libA.  The organization is invested in monorepos, so doesn't want to move away from them.

## Workflow 1: Cut a branch anyway

_Suggested by Paul._

Cut a release branch of the whole monorepo.  Development of appZ and bugfixes in libA should be made on trunk and cherry-picked to the release branch.

Problems: Miss out on the velocity of changes on the main branch.  Cherry-picking changes can become difficult the further things get behind.

These sort of branches should not live long.

## Workflow 2: Vendor yourself

_Suggested by Kiril._

Copy-paste the library to a new directory.  Essentially the same as vendoring, but you are vendoring yourself.

Problems: hard to manage divergence between the vendored and original copy.  Hard to backport changes.

Could git subtree be used to do the copy?

Would like to be able to see the differences between the two copies.

Codemods can still hit the copy.  May need some kind of opt-out or owners enforcement to prevent changes to the frozen copy.

Two copies side-by-side is annoying, *but* it reflects reality: there are two copies of the code in-flight at the organization.  Makes it clear to users that there is a cost of this on the org.

Temptation management needs to be included.  Doing this too much hurts the overall ecosystem.

## Workflow 3: Merge many branches to assemble the monorepo

_Suggested by Pierre-Yves._

Rather than an ordinary monorepo, each team lives on its own branch, but has the same file structure as the monorepo (i.e. libA lives on branchA but is still located inside `/libA/`.  Use merges to express dependencies (`libA` is merged into `appX`).  All branches can be merged together to give the monorepo "view".

Why merges and not graft?  Graph shows correct history - won't need to merge the same thing twice.

Observation by Mark: sounds a bit like AOSP's structure, but keeping everything in the same repo and using branch merges instead of using the `repo` tool to stitch together lots of little repos.

What about conflicts?  They could be shared.  Would result in criss-cross merges, which can be fun and interesting.

Wouldn't that approach invert to freezing by default, not actually a monorepo, and losing the always-at-trunk advantage.  Would need lock-step upgrades, and teams would all need to take on board the chore of upgrading their dependencies.

## Workflow 4: Use package management

_Suggested by Helio._

Release from the monorepo like you would an external library, and then import that into the code.  You can configure the build tool to refer to previous versions rather than the latest.

Problems: Lose the abilty to make hotfixes (need to build a new package somehow).  Somewhat abandoning the monorepo and going back to higher-friction workflows.

## Workflow 5: Submodules

_Suggested by Pierre._

Create a monorepo of submodule pointers.  The actual code lives on separate branches.

If submodules were extended to allow subpaths to be imported, could use yourself as the submodule source repo and avoid having external state.

This is similar to the vendor approach, but using submodules.

Problems: Submodules are hard.

## Other questions

Could a build tool or CI be used to test things in libA before they land so they don't break appZ?  CI can't catch everything. If there is Manual QA for the product, that can't be run on every change.

Is this just a one-off, or do you want to make it a first class citizen?  If one-off, could take a quick-and-dirty approach.

## Feedback from orgs that have oscillated in the past

Primary learning: don't do this.  Make up your mind and stick to it.

There is O(repo) work every time you change.

Some orgs end up with a combination: monorepo is absorbing SRE time to make it scale, and meanwhile projects not in the monorepo get neglected.

Monorepo approach requires investment.  Without this you can see it in the metrics: engineers cannot get to production in a timely fashion.
