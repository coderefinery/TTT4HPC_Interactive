# Data sync

:::{objectives}
- Make a data plan before you start: plan for the syncing you may need
- You may need syncing for computing in multiple places, or for
  setup/backup.
- You can transfer data and make two copies.
- You can mount data and use it from place to place
:::


## Making a data plan

Make a plan before you start!  *Syncing everything to everywhere will
result in unmanageable copies.*

1. Getting original data, making a **copy for analysis**
2. **Archiving data** after you are done
3. **Moving** data from place to place, *during* the project,
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
- `ssh` is the standard transport, and `scp`, `sftp`, `rsync`, and
  more all use `ssh`.
  - Set up a [SSH config file](https://scicomp.aalto.fi/scicomp/ssh/#ssh-config).
- When data is super-massive, there are other protocols, but you'll
  learn them if you need them.
- If you get two copies, you have to be careful *they don't get out of
  sync and you have to merge them together again*.


### rsync: smart file transfer

Rsync transfers files, but does so smartly:
- If only some parts of the file are changed, only transfer those
  parts.
- Resume interrupted transfers.
- Mirror entire directory trees, including options for permissions,
  timestamps, deleting files, etc.

Basic usage:

```console
$ rsync FILE_OR_DIRECTORY HOST:FILE_OR_DIRECTORY
```
There are many options for almost any problem you may have:
- `-r`: Recursive
- `-t`: Preserve timestamps
- `-a`: Mirror everything fully (includes `-r` and `-t`)
- Recommendation: `-a` is usually good, but `-rt` may be good on
  clusters where you can't (or don't want to) preserve users, groups,
  and permissions.
- `-v` and `-i` print out more information on exactly what is being
  transferred
- `-n` (`--dry-run`) prints out what would happen, but doesn't do it.
- `--update` skip files that are newer on the receiving end
- To put files exactly where you want, I recommend giving two full
  paths, both with a `/` on the end.

It can go either way, but it *can't* go between two remotes.

:::{demo} Demonstration of rsync

Syncing a directory from one place to another.  Show rsync from
command line
:::


:::{exercise} Homework: rsync

1. Install rsync if you don't already have it (It is in all package
   managers on Linux.  It's default on MacOS.  I'm not sure the best
   option on Windows, but it is possible.  And it's almost certainly
   on any HPC cluster out there.)

Do the following, and as your assignment, save all of the console log.

2. Find, or make, a directory with 5-10 files in it.  (You can have
   more, but 5-10 keeps the console logs more reasonable.  Your choice.)

3. Rsync the directory from one computer to the cluster, using `-rtiv`

4. Modify one file and re-run rsync (still using `-rtiv`) option, and
   verify that only the modified file is transferred.

At each step, write (in your own free-form words) what is happening.
Include your console input and output in your submission.
:::



## Syncing data two-ways

By it's nature, *it has to keep records of the last state on each
side, so it knows which side is most recently updated, or if BOTH have
been updated* - protect these state files or you have to resolve
everything again.

If you need more than two ends, make a star topology.


### Unison: syncing data

Unison is a program that does bi-directional syncing.  It uses the
rsync protocol to do the actual transfers.  (There are multiple things
called unison, this is https://www.cis.upenn.edu/~bcpierce/unison/ and
https://github.com/bcpierce00/unison and *not* Intel Unison.

Unison has to be installed on both sides.  Since recently, the version
numbers don't have to exactly match.  You can download and use a
static binary.  It has a GPU that is good for seeing and resolving
changes, and a command line interface.

#### Using unison

The basic syntax looks like:

```console
$ unison word-count/ ssh://HOSTNAME//full/path/to/word-count/
$ unison word-count/ ssh://HOSTNAME/relative/path/to/word-count/
```

Useful options:
- `-servercmd PATH` tells a path to unison on the remote end (if it's
  not in `$PATH`
- `unison-gtk` provides a graphical application which makes resolving
  conflicts quite nice, including diffing them.

It stores the state files in `~/.unison/` on both ends.

:::{demo} Demonstration of unison

Syncing a directory from one place to another.  Show Unison from
command line without profiles
:::

:::{exercise} Homework: unison

1. Install unison on your computer and the HPC cluster.

   - On Linux it's likely to be in package managers, but only versions
     2.52 and higher support syncing without local and remote being
     the exact same versions.  Check this first (2.52+ is in Debian
     stable but not Ubuntu 22.04 LTS, for example).

   - There are downloads for other operating systems on the [releases
     page](https://github.com/bcpierce00/unison/releases).

   - You probably need to install it on the cluster yourself.  On the
     [releases page](https://github.com/bcpierce00/unison/releases),
     the `unison-XXXXXX-ubuntu-x86_64-static.tar.gz ` is statically
     built and thus probably works on your cluster.  Download and copy
     just the `bin/unison` program over - that is all that's needed.
     Use `-servercmd path/to/unison` to tell where to find it.

2. Sync one directory

3. Modify two different files (one on each end) and re-sync

4. Modify the same file on both ends. Re-sync and see how it doesn't
   touch that file until you specify how to resolve the conflict.

Send all the commands you ran, their console outputs, and reflections
on what each step was doing (free-form text)
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

Installation: Linux package manager, conda-forge, or download from the
website.

Personal opinion (RD): Ask for help if you want to set it up.  It
should probably be used more than it is, but for now let's consider it
a specialist's tool.

:::::{demo}
- Show the CodeRefinery [video-processing
  repository](https://github.com/coderefinery/video-processing).  You
  can see this online, but the online version doesn't include the files.
  ```console
  $ cd video/video-processing
  ```

- Run `git-annex list` in it to see the files:

  ```console
  $ git annex list ttt4hpc-2024/
  here
  |origin
  ||triton
  |||allas
  ||||web
  |||||bittorrent
  ||||||
  X_XX__ ttt4hpc-2024/out/day1-intro.mkv
  X_XX__ ttt4hpc-2024/out/day1-io.mkv
  X_XX__ ttt4hpc-2024/out/day1-resources.mkv
  X_XX__ ttt4hpc-2024/out/day1.1-icebreaker.mkv
  X_X___ ttt4hpc-2024/raw/day1-obs.mkv
  ```

  This shows that the ttt4hpc-2024 videos are "here" (my desktop), on
  `triton` (the cluster), and the processed videos (`out/`) are in
  Allas (a national S3 object storage).

- Get a video.  This repository is set to auto-init the S3 special
  remote, so you can download videos without YouTube.  You can even
  help improve the captions if there was something wrong (or if you
  are fast enough, help with the processing after the course by
  writing descriptions or debugging captions).

  ```console
  $ git annex get ttt4hpc-2024/out/day1.1-icebreaker.mkv
  get ttt4hpc-2024/out/day1.1-icebreaker.mkv (from allas...)

  (checksum...) ok
  (recording state in git...)
  ```

:::::

:::{exercise} Homework: git-annex (optional, advanced)

This exercise is advanced and thus not recommended for most people.

1. Install git-annex
2. Clone the [video-processing repository](https://github.com/coderefinery/video-processing).
3. Run `git-annex init` to set up your copy for git-annex (no date is
   sent anywhere else)
4. Run `git-annex list` and see what files are available for getting
   (all the files are also broken symbolic links)
5. Run `git-annex get FILENAME` and see it appear.
6. Figure out how git-annex knows where to automatically store every file.
   Hint: check the git-annex manual page.  List the rules that exist
   for the "triton" and "allas" remotes.

Then, try to make your own repository (you'll need to follow the [walkthrough](https://git-annex.branchable.com/walkthrough/):

7. Create a new git repository
8. Initialize git-annex inside of it
9. Add a file to git-annex
10. See how the directory changed (`ls -l`)

:::



## Mounting data from place to place

You can mount data and use it from one place to another: it becomes
two views of the same data.
- Data isn't copied, but a view is made (technical term: filesystem mount)
- Commonly used within internal networks (this is how all files on all nodes on a cluster look the same)
- Useful for (for example) viewing output figures and so on.
- Common and easy to use: sshfs (it uses ssh!)
- These are as slow as the internet connection between the files.
  Userspace tools like sshfs are even slower.

:::::{demo}
We will show SSHFS from desktop to cluster.  This assumes I've made
the directory `mnt/triton/` (my `$HOME/mnt/` directory contains places
for all the things I usually sshfs mount).

```console
## This mounts the relative directory git/ , which is $HOME/git/
$ sshfs triton:git/ mnt/triton/

$ ls mnt/triton | head -3
clean-shell/
envkernel/
fancyquota/
```
:::::

:::::{exercise} Homework: sshfs
1. Install SSHFS on whatever computer you use for daily local work.
   - You'll have to figure out if this is easy (it might not be
     possible).

2. SSHFS mount a remote directory that has a large file in it. (you
   can make a 1GB file with `dd if=/dev/urandom of=some_file bs=1M count=1000`)
3. Run `time md5sum` (or some other checksum program) on both the
   mounted file, and via ssh to the cluster.
4. How does the run time compare on the sshfs mounted file, and
   directly on the cluster?  How fast of network speed should you have
   between your computer and the cluster?
:::::



## Summary

There are three main ways of transferring data, and each has its own place:
- Copy data one-way
- Synchronize data two-ways
- Do a filesystem mount (sshfs) to make another view of the same data
