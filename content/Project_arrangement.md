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



## Code arrangement
- An installable software package can be reused easily in different
  projects, via the project requirements.
- This can be a good alternative to keeping mature code/algorithms
  reusable and not copied from project to project.
- For development, you can (python) `pip install -e
  ../path/to/project/` to make an "editable" version
- Python for SciComp contains an example of [Python
  packaging](https://aaltoscicomp.github.io/python-for-scicomp/packaging/).



## Summary

- Don't be too fancy in your arrangements.
- Plan for a future where you don't remember how it was arranged.
- Keep different types of data together (either in project
  directories, or sub-directories)
