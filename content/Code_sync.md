# Code sync

```{objectives}
- Keep a backup copy of your code
- A workflow for coding on a cluster
```

## Introductions

In the previous section we talked about syncing data.
Most of the lessong there apply to this code as well, but
there are some differences. Mainly, code changes more
quickly than data and needs to be synced more often.

You might also want to do most of your coding work locally,
instead of coding on the cluster. There you will have all
your favorite tools available and can be more productive.

We will demonstrate a workflow for keeping your code in
sync between a laptop and a cluster using git. On both
systems, we will push the code to a cloud server and pull
updates from there. This
workflow comes with a great benefit, it keeps an online
backup of the code up to date.

You can use any cloud service that supports git, or even
set up your own repository on a server you have ssh acces
to.

## Setup steps

1. **Set up a cloud repository**: Create a repository on
   a cloud service like GitHub, GitLab, or Bitbucket. 
   Clone it and add your code.

   If you are not familiar with this part, Coderefinery has
   [excellent lessons](https://coderefinery.github.io/git-intro/)
   on git and related workflows.

2. **Clone to the repository**: Clone the repository to on the
   cluster


## Actual workflow

1. **Pull from the cloud**: Before you start coding, pull
   the latest version of the code from the cloud.

```bash
git pull
```

2. **Make a change**: work on your code. Run any tests
    you can run locally.

2. **Commit and push**: While working, create periodic
    commits and push them to the cloud.

```bash
git commit -m "message"
git push
```

3. **Pull on the cluster**: Pull the changes to the cluster
    to run tests of benchmarks.

```bash
git pull
```

- The main downside of the this workflow is that you cannot
  test or benchmark your code where you edit it. You can 
  still edit on the cluster side, just commit and push
  before moving back to the local repository.

## Summary

- A simple workflow that using git
- Keeps an online backup up to date
- Extra steps when testing on a cluster

