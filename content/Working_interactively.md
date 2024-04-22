# Working interactively

Clusters are about batch processing: they provide tons of resources
and there are lots of users, and all those scheduled as efficiently as
possible. But sometimes, being able to see the computations as they
come out with your own eyes helps (mainly in either development or
debugging).  You can, in fact, do this.

:::{admonition} Initial notes

## GUI

- Ways this is possible on HPC too
- RStudio/Jupyter/VSCode
- GUI software via OOD or -X

## CLI

- Interactive working from command line - possibilities
- When do and when is it time to move to batchjobs?
:::

## X Forwarding

:::{demo} X Forwarding
- `ssh -X` and `ssh -Y`
- `xeyes`
- Navigate to the R example directory
- `gnuplot -c gnuplot.plt`
::::

- Sends the image of the GUI to your local machine and
  the input back to the remote machine.
- Needs a fast network, otherwise get's very laggy


## Open On Demand

- A more efficient way of running graphical applications
  on the cluster
- The interface depends on the cluster
- Not available on all clusters

:::{demo} Open On Demand
- Open OOD
- Look at the file system, verify it is the cluster
- Open the Rstudio app
- Open and edit the R script
:::


:::{exercise} Graphical User Interfaces
1. **Graphical User Interfaces**
   - Find out what GUIs are available on the cluster
   - Is your favorite text editor available?
   - What other graphical applications are available? What would you use if they were?
   - Try uploading or downloading afile
:::
:::{exercise} X Forwarding
1. **X Forwarding**
   - What is the difference between `ssh -X` and `ssh -Y`? (Do a web search and see what you can find.)
   - Try running a graphical application on the cluster and displaying it on your local machine
:::

## Interactive jobs

:::{demo} Interactive jobs
- demo `srun --pty bash`
- Explain the use cases
:::

## Sessions that persist across logins: screen and tmux

:::{demo} tmux
Demo of tmux
:::

## Debugging

Many people ask how to connect their graphical IDE to a cluster in
order to do debugging.  Did you know you can debug things without a
graphical environment - thus easily on the cluster?

:::{demo} Debugging with PDB (python debugger)
Demonstration of Python debugging using command line and pdb.
:::

## Shell tips and tricks

:::{demo} bash tips for interactive work
- efficient history searching
- aliases
- functions
- conditionals in .bashrc
:::
