# Project arrangement

:::{objectives}
- Project arrangement is the way you organize files into directories
  and projects.
- Project directories: Choose good names and divide up files well
- Keep types of files organized within directories
:::

Project arrangement not usually discussed (beyond "make directories"),
but setting this up will can be a good start to everything that comes
later. A good arrangement makes syncing, backup, and reproducibility
much easier.



## Why project arrangement matters

:::{admonition} Story time: the missing file

One of the authors has some file they would like to find, from
around 2007.  They probably have it backed up somewhere, but it was in
a directory, that got compressed, and then the directory containing
that got compressed/backed up again.  Maybe one other round?

It probably *could* be found but it's not worth the instructor's time anymore.
:::

- If arrangement isn't good, then you will eventually forget how
  things are organized.
- If you arrange projects well, then moving them around is easier
- Keeping similar types of data together makes it easier to manage.



## Group work

:::{type-along} Together: share project arrangements
- In the notes, share the best-organized (or worst-organized) project
  you have (it's ok if it's not perfect: most aren't!).
- Comment on what makes it good and what could be improved.

We'll discuss at the end.
:::




## Naming projects and keeping them organized

- **Decide what goes together**, call it a project
- Give a **unique name** for each project
- That **name always refers to the same thing**, on every computer.
- **Each directory should be managed the same way** - git, rsync,
  etc.  We'll take about this later.
- Every project should have at least a **minimal README** explaining
  to *you* what the project is about.

Examples:

- `projA`  (main project directory)
- `projA-data` (this project has too much data or the data is reused
  among multiple projects, so it's split out)
- `projA-paper1` (the articles are tracked separately)

My recommendation is that these directories are as flat as possible,
not sub-directories of each other.  This makes it easy to manage.

Ways of working together:

- Everyone has their own **local clone** of the code (good for the code
  directories and other workspaces)
- **One shared folder** that everyone edits (good for data, etc.)



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
- `PROJECT/paper1/` - different papers/reports, if not stored in a
  different project directory.
- `PROJECT/paper2/`
- `PROJECT/opendata/` - things you will share



## Directories for teams

- Usually you get *one* directory per project allocated on the
  cluster.
- This has to be divided further
- Then often subdirectories for each user, for their own workspaces.
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
- We'll look at the notes and for questions and stories from the group
  work above.
:::



## See also

- [CodeRefinery reproducibility
  lesson](https://coderefinery.github.io/reproducible-research/),
  [Organizing your projects episode](https://coderefinery.github.io/reproducible-research/organizing-projects/)



## Summary

- Don't be too fancy in your project arrangements.
- Plan for a future where you don't remember how it was arranged.
- Keep different types of data together (either in project
  directories, or sub-directories).
