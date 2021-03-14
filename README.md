# SELF Build Environments
Copyright 2021 Fluid Numerics LLC

This repository contains Dockerfile definitions for images that are used as the base build environments for [SELF](https://github.com/FluidNumerics/SELF). Intentionally, this repository may be useful for other software projects that are looking to build modern HPC applications that need to target traditional CPU only platforms and GPU accelerated platforms from multiple vendors.

SELF can be built for cpu-only (serial and parallel), gpu, and multi-gpu platforms. For GPU platforms, [hipfort](https://github.com/ROCmSoftwarePlatform/hipfort) enables builds for AMD and Nvidia hardware. Because of this, there are a number of build environments that are needed for building SELF artifacts that can be deployed on a variety of hardware

* Serial x86 CPU
* Parallel x86 CPU
* Single AMD(hcc) GPU x86 CPU
* Single Nvidia(nvcc) GPU x86 CPU
* Multi AMD(hcc) GPU x86 CPU
* Multi Nvidia(nvcc) GPU x86 CPU

Build environments in this repository are organized by `{compiler}/{mpi}/{platform}`. For serial or single gpu builds, `{mpi}` is replaced with `serial`. `{platform}` always references `{cpu_platform}-{gpu_platform}`; if a `{gpu_platform}` is not targeted in the build environment, `{platform}={cpu_platform}`.

 Currently available build environments are

* [`gcc-10.2.0/serial/x86`][./env/gcc-10.2.0/serial/x86]
* [`gcc-10.2.0/serial/x86-hcc`][./env/gcc-10.2.0/serial/x86-hcc]
* [`gcc-10.2.0/serial/x86-nvcc`][./env/gcc-10.2.0/serial/x86-nvcc]


## Usage
If you are a SELF developer and you would like to have these build environments, you can build Docker images using the Dockerfiles in any of the `env/` subdirectories. If you are working on Google Cloud Platform, you can use the provided [cloudbuild.yaml](./cloudbuild.yaml) file to build the build environments and post the resulting images to your container registry, e.g.
```
gcloud builds submit . --substitutions=_COMPILER=gcc-10.2.0,_MPI=serial,_PLATFORM=x86-hcc --async
```

## Contributing
We welcome contributions from you on this repository! 

If you have another compiler, MPI flavor, or target platform you would like to contribute:
1. Open a new issue (if there's not one already) for your proposed build environment.
2. Fork this repository.
3. Make and commit your changes to your fork.
4. Open a pull request back to this repository.
5. Work with the maintainers to get your changes merged in!


