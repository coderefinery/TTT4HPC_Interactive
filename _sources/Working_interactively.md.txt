# Working interactively (CLI)

:::{objectives}
- Interactive jobs allow better debugging
- screen and tmux allow you to keep things running when you log out.
- Command line debuggers exist and work
- Some common shell config can make your life easier.
:::


Clusters are about batch processing: they provide tons of resources
and there are lots of users, and all those scheduled as efficiently as
possible. But sometimes, being able to see the computations as they
come out with your own eyes helps (mainly in either development or
debugging).  You can, in fact, do this.



## Interactive jobs

**Submit, wait, review, repeat.**  That's too slow for development.
What can you do?  Interactive jobs.

You can request resources, but instead of running the program
directly, you get a shell.  Use the shell to run your tests, over and
over again.

:::{demo} Demo: Interactive jobs
- Demo `srun --pty bash`
  - This runs a job and gets a shell
  - You can run your jobs over and over as a test
  - There may be other options to use, like `-p interactive`, or a
    different command like `sinteractive`.  Read your instructions
- Other clusters may have other
:::

When is this useful?  When is this a problem?


:::{exercise} Homework: Interactive on your cluster
- Figure out how to run interactively jobs on your cluster
- Run interactively and run your own code.
:::



## Sessions that persist across logins: screen and tmux

When you log out, everything (except batch jobs) gets lost.  Or does
it?

[screen](https://en.wikipedia.org/wiki/GNU_Screen) and
[tmux](https://en.wikipedia.org/wiki/Tmux) allow you to start a
session, log out, and resume that session.  Sessions have to be
started within screen/tmux.

Screen is slightly older, tmux is newer.  Otherwise they are pretty
similar.

:::{demo} Demo: tmux
- Connect to the cluster
- Navigate to a directory
- Create a tmux session: `tmux new`
- Start a program: `htop` or similar
- Detach tmux: `C-b d` (Control-b is the standard tmux prefix key, `d`
  for detach)
- Log out
- Log back in
- `tmux list-sessions`
- `tmux attach -t ID`
- Create a new window: `C-b c`
- Switch windows forward and back: `C-b p` and `C-b n` (and `C-b`+a number)
:::

When is this useful?  When is this a problem?  Should you combine
interactive jobs and screen/tumx?

:::{exercise}
Experiment with tmux or screen.
:::



## Debugging

Many people ask how to connect their graphical IDE to a cluster in
order to do debugging.  Did you know you can debug things without a
graphical environment - thus easily on the cluster?

:::::{demo} Debugging with PDB (python debugger)
Demonstration of Python debugging using command line and pdb.

```console
$ python3 -m pdb pi-broken.py 1000
```
```
> /home/darstr1/pi-broken.py(2)<module>()
-> from __future__ import division, print_function

(Pdb) cont
Calculating pi via 1000 stochastic trials
Traceback (most recent call last):
  File "/usr/lib64/python3.6/pdb.py", line 1667, in main
    pdb._runscript(mainpyfile)
  File "/usr/lib64/python3.6/pdb.py", line 1548, in _runscript
    self.run(statement)
  File "/usr/lib64/python3.6/bdb.py", line 434, in run
    exec(cmd, globals, locals)
  File "<string>", line 1, in <module>
  File "/home/darstr1/pi-broken.py", line 2, in <module>
    from __future__ import division, print_function
  File "/home/darstr1/pi-broken.py", line 70, in estimate_pi
    in_circle_points = points_in_circle(iterations)
TypeError: points_in_circle() missing 1 required positional argument: 'seed'
Uncaught exception. Entering post mortem debugging
Running 'cont' or 'step' will restart the program
> /home/darstr1/pi-broken.py(70)estimate_pi()
-> in_circle_points = points_in_circle(iterations)

(Pdb) list
 65             in_circle_points += pic_wrapper((iterations_serial, random_gen.randint(0, 2**32 - 1)))
 66
 67             # Returns pi and in-circle points (successes)
 68             return in_circle_points*4/iters_actual, in_circle_points
 69         else:
 70  ->         in_circle_points = points_in_circle(iterations)
 71             return in_circle_points*4/iterations, in_circle_points
 72
 73
 74     def estimate_pi_vectorized(iterations, seed):
 75         batch_size = int(1e5)

(Pdb) help(points_in_circle)
*** No help for '(points_in_circle)'

(Pdb) points_in_circle(iterations, seed)
782
```
:::::

This works, it can be primitive but good for quickly testing thinsg.

:::{exercise} Homework: command line debuggers
Try out the above, or the IPython debugger `ipdb` which is more
colorful and powerful.
:::


## Shell tips and tricks

Various things in the shell can make your life easier.

:::{demo} Demo: History searching
- You know about the up and down arrow keys, right?  Try `C-r` and
  typing a few characters for an incremental backwards search.
- Try adding `!$` as a command line argument (at least zsh and bash).
  This is replaced with the last argument of the previous line.  There
  are many more of these.
:::

:::{demo} Demo: shell aliases
- In your .bashrc, add
  ```bash
  alias l=`ls`
  alias ll=`ls -l`
  ```
- These are aliases: simple way to save keystrokes.  You can do plenty
  more.
- For other shells, learn how to do this.
:::

:::{demo} Demo: shell functions
- In your `.bashrc`, add:
  ```bash
  function sacct-at-time () ( sacct -N "$1" -S "$2" -E "$2" -s R ; )
  ```
  This runs sacct at with a certain `-S` and `-E` (start/end) and `-N`
  (a node list) - used for finding what was running on a node at a
  certain time.
- This can't be an alias since the arguments get interspersed and
  duplicated within the commond
- For other shells, learn how to do this.
:::


:::{demo} Demo: one .bashrc with conditional sections
- Add the following to your `.bashrc`:
  ```bash
  # hostname -d  returns the domain part (triton.aalto.fi on any
  # Triton cluster node)
  if [ "$(hostname -d)" = "triton.aalto.fi" ] ; then
      # command specific to the Triton login node
  fi
  ```
  Or even this:
  ```bash
  if [ "${HOSTNAME: -15}" = "triton.aalto.fi" ] ; then
      # commands for any Triton node here
  fi
  ```
  You have to verify the contents of `hostname -d` and `$HOSTNAME`
- This allows you to have one .bashrc file that is used on all
  computers, with common stuff and computer-specific things, if you want.
- How do you keep them in sync? (think back to {doc}`Data_sync`)
- For other shells, learn how to do this.
:::



## Summary

The command line can provide plenty of power to compete with
graphical interfaces - sometimes.
