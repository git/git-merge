# Rethinking Git UI (based on learnings from sapling and interactive smartlog)

## Notes

- Re:stacking - Not necessarily new (i.e. patch series)
    - Reinvigorating an "old" workflow after having popularized PR/MR workflows for the past decade or so.
- Timemachine for git (?) 
- We want to constantly be able to rewrite our branches

## Questions

- How much should these other tools prioritize Git interop?
- Stack based workflows?
- What is missing in the metadata / headers to enable a more stack-based / patch series workflow?
- Is it git's reposibility to enable these additional workflows?
- Do we even want to keep the intermediate stuff from every change from 5 years ago?
    - Sapling trims those intermediate changes at merge
- What implications do non-linear patch series / stacks have?
- What if a git branch can point to multiple commits?
    - If a branch has multiple heads?
        - How do we represent this to users so that it makes sense?


## Issues

- Tracking commits between changes; i.e. 
    - `change-id`
    - Pointer to previous version to track history (adding another DAG)
        - mercurial using an "obscelesnce marker" in addition to track commit history
    - Tooling to view this history (?) 
        - mercurial-ui slowly introduces the user to the concept of this history; 
    - Issues with unwanted GC
        - Need a better solution than the reflog hack Scott demoed
- Meta: How do we avoid tools wiping each others headers and/or additional directories?
    - Official add some space and support 3rd party headers, directories, etc.


## Next Steps

- Reach out to caleb@gitbutler.com or nico@gitbutler.com to get added to the email thread
