# Git LFS
## Can we do better?

[Git LFS](https://git-lfs.com/) uses Git smudge/clean filters to not store large blobs directly in Git.
What is checked into the repo is instead small pointer files.

Problems:

* difficult to convert repo with large files to remove
* difficult to change rules after the fact
* git server doesn't understand natively

Would be nice if Git supported large files natively.

[Git Annex](https://git-annex.branchable.com/)?

Can use [remote helpers](https://git-scm.com/docs/gitremote-helpers) to access remotes. Can use it to store large objects on a GCP server.

What does this look like on the Git server that doesn't have all the objects anymore.

Christians series on the mailing list for promisor remotes (Aug 2024): https://lore.kernel.org/git/20240731134014.2299361-1-christian.couder@gmail.com/


A way to not delta compress large binary files?

You can partial clone with a filter. You can do a clone with a [blob size filter](https://git-scm.com/docs/git-rev-list#Documentation/git-rev-list.txt---filterltfilter-specgt).

```
git clone --filter=blob:limit=1k
```

Would be nice if you could say in the attributes how to handle large files for this repository so not every user does not have to set up settings.

There used to be an rsync remote helper.

Multiple version of large files locally is still an issue. `git repack --filter` to remove older large files that we don't need locally anymore.

Garbage collect stuff that's on the remote.

If you have large files in LFS and wanted to move to promisor remotes.

What should it look like for new repos?

Is the ability to obsolete old files a requirement of large file handling? What if we could have hole-y history? How would you calculate back your hashes? With Git LFS, you can change the content, it's not hashed, just the pointer.

If we allow blobs to vanish, we need to tell git fs check to ignore it in our check.

With promisors, you could have a recieve hook that looks for large files and offloads them to the other remote.

Could we have a push filter where large files could go directly to the promisor remote, then push a history missing those objects to the code server?

LFS could in theory move it's logic to core git and promisors.
