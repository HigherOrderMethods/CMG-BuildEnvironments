# CMG-Build Environments
Copyright 2021 Fluid Numerics LLC, HigherOrderMethods

The CMG Build Environments repository is meant to provide good starting point environments for developers that are looking to create portable GPU accelerated applications.
This is achieved through Dockerfile recipes that incorporate 
1. A C/C++/Fortran compiler suite
2. An MPI implementation (e.g. OpenMPI, MPICH, MVAPICH)
3. Development libraries that support GPU offloading to multiple GPU architectures (e.g. ROCm)

Dockerfile recipes leverage [spack](https://spack.io) for concise environment installation

## How do I make a contribution?

### Contribute a new environment
Before making a contribution, take a look through the [`env/`](https://github.com/HigherOrderMethods/CMG-BuildEnvironments/tree/main/env) subdirectory to see which environments are currently available and review the [issue tracker](https://github.com/HigherOrderMethods/CMG-BuildEnvironments/issues). This will help you make sure that your efforts are not duplicating others.

1. Create an issue (feature request) if one does not exist, or find an issue related to the environment you want to see made available.
2. Fork the repository associated with the issue to your local GitHub organization. This means that you will have a copy of the repository under your-GitHub-username/SELF.
3. Clone the repository to your local machine using git clone https://github.com/github-username/CMG-BuildEnvironments.git.
4. Develop your environment!

Your environment must be placed in the `env/` subdirectory and directory tree pattern `{compiler}/{mpi}/{platform}`. 
* `{compiler}` is always `{vendor}-{version}`.
* For serial or single gpu builds, `{mpi}` is replaced with `serial`. 
* `{platform} always references {`cpu_platform}-{gpu_platform}`; if a `{gpu_platform}` is not targeted in the build environment, `{platform}={cpu_platform}`.

We recommend that you use an existing environment as a starting point.

### Acceptance criteria
* At a bare minimum, your container must build without failure.
* If you have another project that is using that environment, [add it to the README](https://github.com/HigherOrderMethods/CMG-BuildEnvironments#repositories-using-these-recipes)


## What does the Code of Conduct mean for me?
Our Code of Conduct means that you are responsible for treating everyone on the project with respect and courtesy regardless of their identity. If you are the victim of any inappropriate behavior or comments as described in our Code of Conduct, we are here for you and will do the best to ensure that the abuser is reprimanded appropriately, per our code.
