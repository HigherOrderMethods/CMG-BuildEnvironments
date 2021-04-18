# CMG Build Environments
Copyright 2021 [Fluid Numerics LLC](https://fluidnumerics.com)

## About {Compiler}-{MPI}-{GPU} Build Environments
This repository contains Dockerfile definitions for images that are used as the base build environments for modern HPC applications that need to target traditional CPU only platforms and GPU accelerated platforms from multiple vendors.

Container recipes included in this repository leverage [spack](https://github.com/spack/spack) environments to help keep recipes as simple as possible.

This repository is motivated by the view that modern HPC applications can be built for cpu-only (serial and parallel), gpu, and multi-gpu platforms. 

For GPU platforms, [hip](https://github.com/ROCm-Developer-Tools/hip) and [hipfort](https://github.com/ROCmSoftwarePlatform/hipfort) enable builds for AMD and Nvidia hardware. Because of this, there are a number of build environments that are needed for building modern HPC application containers that can be deployed on a variety of hardware

* Serial x86 CPU
* Parallel x86 CPU
* Single AMD(hcc) GPU x86 CPU
* Single Nvidia(nvcc) GPU x86 CPU
* Multi AMD(hcc) GPU x86 CPU
* Multi Nvidia(nvcc) GPU x86 CPU

Build environments in this repository are organized by `{compiler}/{mpi}/{platform}`. For serial or single gpu builds, `{mpi}` is replaced with `serial`. `{platform}` always references `{cpu_platform}-{gpu_platform}`; if a `{gpu_platform}` is not targeted in the build environment, `{platform}={cpu_platform}`.

 Currently available build environments are

* [`gcc-10.2.0/serial/x86`](./env/gcc-10.2.0/serial/x86)
* [`gcc-10.2.0/serial/x86-hcc`](./env/gcc-10.2.0/serial/x86-hcc)
* [`gcc-10.2.0/serial/x86-nvcc`](./env/gcc-10.2.0/serial/x86-nvcc)
* [`gcc-10.2.0/openmpi-4.0.2/x86`](./env/gcc-10.2.0/openmpi-4.0.2/x86)
* [`gcc-10.2.0/openmpi-4.0.2/x86-hcc`](./env/gcc-10.2.0/openmpi-4.0.2/x86-hcc)
* [`gcc-10.2.0/openmpi-4.0.2/x86-nvcc`](./env/gcc-10.2.0/openmpi-4.0.2/x86-nvcc)

Planned build environments include


### Repositories using these recipes

* [SELF](https://github.com/FluidNumerics/SELF).

## Usage
You can build Docker images using the Dockerfiles in any of the `env/` subdirectories. 
If you are working on Google Cloud Platform, you can use the provided [cloudbuild.yaml](./cloudbuild.yaml) file to build the build environments and post the resulting images to your container registry, e.g.
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


## Support
To obtain support, simply post a new issue to this repository.

### About the support team
The HigherOrderMethods organization is a (new) community driven organization started by [Fluid Numerics LLC](https://fluidnumerics.com) that aims to provide open-source tools for modern HPC and Scientific Computing applications. As a community organization, support of this repository depends on volunteerism from the community. [Become a member of this community](https://forms.gle/YwBhyCXVEiZPH1pN6)

Contributors to this repository contribute on a volunteer basis. We will do our best to respond to issues as they arise. Keep in mind that the open-source license applied to this repository releases Fluid Numerics LLC and HigherOrderMethods from any liability related to the usage of this code. Additionally, the license, like all open-source licenses, does not provide any guarantee of support.
