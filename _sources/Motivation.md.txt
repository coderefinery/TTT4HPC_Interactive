# Motivation

This day is about the daily struggle with HPC: how do people
actually interact with the cluster?  What are the nicer tools than
SSH, but can' be taught in normal courses since they don't work for
everywhere/everyone?


You will see these ways of interaction today:

- Transferring data
- Transferring code
- Running in testing or interactive mode on the cluster.
- Using graphical interfaces on the cluster.
- Using an IDE (VS Code)


We will try to explain the *geography* of the cluster: what is
everything?  How does it connect?  How do you move from place to place?
Which places are the same?  And so on.  For example,

- Different directories in different places (or the same ones, mounted)
- Different speed of access to data from different places.

```{figure} img/cluster-schematic-cluster.png
:width: 100 %
:alt: The physical map of a typical HPC cluster

The geography of an HPC cluster -- From home or from your office (left hand side) you can connect to the cluster (right hand side) via a **login node**. This is just an entry point for the cluster and no big processes should ever be run here. From the login node you can submit your non-interactive jobs, request an interactive terminal session, transfer data, check the status of your jobs. It is important to notice that the storage you have on the cluster is a special storage connected to all cluster nodes that allows fast I/O (usually implemented with Infiniband technology). Fast I/O comes with a few compromises: i) access to storages that are not inside the cluster (e.g. your university cloud storage) is going to be slow; ii) to ensure fast I/O and large disk space, the cluster high-speed /scratch storage is not backed up; iii) the Internet bandwidth from the computing nodes might be limited.
```

:::{admonition} Cluster etiquette

Not only you are using a remote system, it is a system that is used by many other users. This means that respecting the **cluster etiquette** is extremely important! We do not cover cluster etiquette in this episode, but there is an entertaining [Research Software Hour episode on Cluster Etiquette](https://researchsoftwarehour.github.io/sessions/rsh-013/). 
:::

## About the homework

This day has various exercises/homeworks embedded within it.  Unlike
day 1, these are mostly doing implementing the demos and trying them
out.  We can adjust the exercises to suit people's interests.

Many of them require something to be installed on your own computer,
since they are about copying data to or from the cluster, and you need
the corresponding program installed.  This may make some of the
exercises harder to do for some people.  We'll discuss that in the
exercise session.
