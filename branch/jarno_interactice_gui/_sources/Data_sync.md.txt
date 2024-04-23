# Data sync

:::{objectives}
- Make a data plan before you start: plan for the syncing you may need
- You may need syncing for computing in multiple places, or for
  setup/backup.
- You can transfer data and make two copies.
- You can mount data and use it from place to place
:::


## Making a data plan

Make a plan before you start!  Or at least, understand how you may
need to adapt later.  You will almost always need some of:

1. Getting original data, making a copy for analysis
2. Archiving data after you are done
3. Moving data from place to place, *during* the project,
4. same as (3) but and *it may even be updated on both sides at the
   same time.*

The first three are common and relatively easy, the last requires a
lot of care!

Consider:
- **There are different kinds of data**.  Some can be re-constructed,
  some must be made from scratch each time.
- **Storage spaces have different properties**: Some are good for
  being same for archival.  Some are fast for analysis.  Not many are
  good at both at the same time.
- Will data be updated in multiple places, or in only one place at a
  time?


## Example data plan for a distributed project

:::{demo}
Our project uses a lot of computational resources, so we might run
simulations on multiple clusters.  Data may be created on any, so we
need to carefully manage our data.

- `project`: the main code of the project.  It is always synced with
  git.
- `project-data/`: The data for our project.  The code accesses this
  data by using `../project-data/...`.  This is synced manually.
  - `input/`: Our input data.  Data is *only* added on our
    workstation, syncing is always from the workstation to other
    computer clusters.  Input data should never be modified, but if it
    has to be, it's modified on the workstation and then re-synced.
  - `computed/NNN/`: This is the computed data on any cluster.  Each
    `NNN` is only computed on one cluster, and that is the only
    cluster should modify the `computed/NNN` files.  This data is only
    synced from cluster → workstation (and only the workstation has
    the complete view on everything).
  - `analyzed/`: contains the final analysis that combines all of the
    data.  This isn't very hard, so is always run on my local
    workstation so I can develop quickly.
:::


## Transferring data

Transfering data is relatively easy.
- `ssh` (and programs that use ssh) is the standard method
- When data is super-massive, there are other protocols, but you'll
  learn them if you need them.
- If you get two copies, you have to be careful *they don't get out of
  sync and you have to merge them together again*.


### rsync: smart file transfer

Rsync transfers files, but does so smartly:
- If only some parts of the file are changed, only transfer those
  parts
- Resume interrupted transfers
- Mirror entire directory trees, including options for permissions,
  timestamps, deleting files, etc.

Basic usage:

```{console}
$ rsync FILE_OR_DIRECTORY HOST:FILE_OR_DIRECTORY
```
There are many options for almost any problem you may have:
- `-t`: Preserve timestamps
- `-a`: Mirror everything fully (includes `-r`)
- `-r`: Recursive
- `--update` skip files that are newer on the receiving end

It can go either way, but it *can't* go between two remotes.

:::{demo} Demonstration of unison

Syncing a directory from one place to another.  Show Unison from
command line without profiles
:::


:::{exercise}: rsync

- Rsync a directory from one computer to another, using `-a`
- Modify one file and re-run rsync (now with the `-v`) option, and
  verify that only the modified file is transferred
:::



## Syncing data two-ways

By it's nature, *it has to keep records of the last state on each
side, so it knows which side is most recently updated, or if BOTH have
been updated* - protect these state files or you have to resolve
everything again.

If you need more than two ends, make a star topology.


### Unison: syncing data

Unison is a program that does bi-directional syncing.  It uses the
rsync protocol to do the actual transfers.

Unison has to be installed on both sides.  Since recently, the version
numbers don't have to exactly match.  You can download and use a
static binary.  It has a GPU that is good for seeing and resolving
changes, and a command line interface.

#### Using unison

The basic syntax looks like:

```console
unison word-count/ ssh://HOSTNAME//full/path/to/word-count/
```

It stores the state files in `~/.unison/`

:::{demo} Demonstration of unison

Syncing a directory from one place to another.  Show Unison from
command line without profiles
:::

:::{exercise} unison
- Install unison
- Sync one directory
- Modify two different files (one on each end) and re-sync
- Modify the same file on both ends. Re-sync and see how it doesn't
  touch that file until you specify how to resolve the conflict.
:::



## git-annex

[git-annex](https://git-annex.branchable.com/) lets git track large
files, but it *doesn't* store the contents of the files in the git
history: instead, there are other commands that move the files
directly from remote to remote. (`dvc` is similar)

git-annex is nice for some cases, but it can be rather hard to use.
Aalto SciComp has a [git-annex guide for
scientists](https://scicomp.aalto.fi/scicomp/git-annex/), which
explains things slightly more usably.

Personal opinion (RD): Ask for help if you want to set it up.  It
should probably be used more than it is, but for now let's consider it
a specialist's tool.

:::{exercise} git-annex
- Install git-annex
- Init a git-annex repository
- Record one file with git-annex
- Run `git-annex list`
:::



## Mounting data from place to place

You can mount data and use it from one place to another: it becomes
two views of the same data.
- Data isn't copied, but a view is made (technical term: filesystem mount)
- Commonly used within internal networks (this is how all files on all nodes on a cluster look the same)
- Useful for (for example) viewing output figures and so on.
- Common and easy to use: sshfs
- These are as slow as the internet connection between the files.
  Userspace tools like sshfs are even slower.

:::{demo}
Show SSHFS from desktop to cluster
:::



## Summary

There are four ways of transferring data:
- Copy data one-way
- Synchronize data two-ways
- Use git-annex to track data
- Do a filesystem mount (sshfs)
