# VSCode

```{objectives}
- Learning about remote development methods.
- Using local VSCode for remote development with ssh-remote.
- Debugging project on computational nodes.
```

In this session, we are discussing about remote developement, and it's pros and cons.
We will also demonstarte how to use VSCode (as an example) to do remote development,
and how VScode could be used as an "HPC Interface".

In the end, we will look into one example of debugging code using `debugpy` on one of the computational nodes.

## Setup steps
1. **How to install**: VSCode is an open-source program which can be downloaded directly from [Github](https://github.com/microsoft/vscode).

2. **Setup Remote SSH**: Discussing about the plugin, and `.ssh/config` file. Read more about the configuration [here](https://scicomp.aalto.fi/scicomp/ssh/#ssh-config).

3. **What are the interactive/test/debug slurm partitions**: Every HPC cluster has a dedicated partition for testing/debuging/interacting with small programs. We will discuss how to use them for debugging.

4. **Remote Debugging**: We will demonstrate how to use `debugpy` for remote-debugging python programs.

