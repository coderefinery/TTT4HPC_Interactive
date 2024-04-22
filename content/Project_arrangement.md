# Project arrangement

:::{objectives}
- Project arrangement matters to keep things organized for the future
- Choose names that represent contents of project directories
- Keep types of files organized within directories
:::

Project arrangement is little discussed (beyond "make directories"),
but setting this up will can be a good start to everything that comes
later. A good arrangement makes syncing, backup, and reproducibility
much easier.



## Why project arrangement matters

- Anything that makes sense now won't make sense in a few months or years
- If you arrange projects well, then moving them around is easier
- Keep similar types of data together.
- Every project should have a minimal README explaining to *you* what
  the project is about.



## Naming projects and keeping them organized

- Decide what goes together, give it a unique name
- Each directory with that name should fundamentally be the same.
- Each directory is synced as a whole (git, rsync, etc), excluding
  ignored files - we'll take about this later. This prevents some
  difficult sync things.

Examples:

- `project`  (main project directory)
- `project-data` (this project has too much data or the data is reused
  among multiple projects, so it's split out)
- `project-paper` (the articles are tracked separately)

My recommendation is that these directories are as flat as possible,
not sub-directories of each other.  This makes it easy to manage.

How do people work together:
- Everyone has their own local clone of the code (good for the code
  directories and other workspaces)
- One shared folder that everyone edits (good for data, etc.)



## Directories within projects

- Keep different types of files organized, probably within directories
  for example:
  - source code, original data, parameters, temporary data, final data, etc.

Example:

- `PROJECT/code/` - backed up and tracked in a version control system.
- `PROJECT/original/` - original and irreplaceable data. Backed up at
  the same time it is placed here.
- `PROJECT/scratch/` - bulk data, can be regenerated from
  code+original
- `PROJECT/doc/` - final outputs, which should be kept for a very long
  term.
- `PROJECT/doc/paper1/` - different papers/reports, if not stored in a
  different project directory.
- `PROJECT/doc/paper2/`
- `PROJECT/doc/opendata/`



## Directories for teams

- Usually you get *one* directory per project allocated on the cluster.
- Then often subfolders for each user, for their own workspaces.
  Stuff within here is synced with git
- Common data folders referred to by each individual's workspaces.

Example:

- `SCRATCH/project-2569456905/` - the main directory allocated by the
  cluster admins
- `SCRATCH/project-2569456905/user1/`
- `SCRATCH/project-2569456905/user1/code-projA/` - tracked with git
- `SCRATCH/project-2569456905/user1/code-projB/`
- `SCRATCH/project-2569456905/user2/`
- `SCRATCH/project-2569456905/user2/code-projA/`
- `SCRATCH/project-2569456905/user3/`
- `SCRATCH/project-2569456905/user4/`
- `SCRATCH/project-2569456905/projA-common-data/` - manually managed / git-annex




## Code arrangement
- An installable software package can be reused easily in different
  projects, via the project requirements.
- This can be a good alternative to keeping mature code/algorithms
  reusable and not copied from project to project.
- For development, you can (python) `pip install -e
  ../path/to/project/` to make an "editable" version
- Python for SciComp contains an example of [Python
  packaging](https://aaltoscicomp.github.io/python-for-scicomp/packaging/).


## Group work

:::{type-along} Together: share project arrangements
- In the notes, share the best-organized project you have (it's ok if
  it's not perfect: most aren't!).
- Comment on what makes it good and what could be improved.
:::

## See also

- CodeRefinery reproducibility lesson


## Summary

- Don't be too fancy in your arrangements.
- Plan for a future where you don't remember how it was arranged.
- Keep different types of data together (either in project
  directories, or sub-directories)
