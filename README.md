# The Accel-Sim fronted repository for ACPL
Welcome to the **acpl_baseline** branch.

Basically, the **acpl_baseline** branch is based on the branch **dev** which is maintained by [https://github.com/accel-sim](https://github.com/accel-sim).

For convenience, the **acpl_baseline** branch does not contain following files (While these files exist in the [release](https://github.com/parcolab/accelsim_gpgpusim/tree/master) and [dev](https://github.com/parcolab/accelsim_gpgpusim/tree/master) branch, we removed them here because they do not contribute to run simulation):
- util/
- travis.sh
- get-accel-sim-traces.py
> Note that our lab manages the [tracer repository](https://github.com/parcolab/accelsim_trace_generator) separately.

# How to install
The guide assumes that you will run the simulation on the ACPL's cluster (CentOS 7).

1. Install GCC 7.3.0. To see how to install GCC, refer to the installation guide on our Notion.
2. Set the environment variable, CUDA_INSTALL_PATH to the root directory of CUDA. Note that our lab uses CUDA 10.1.
    ```
    export CUDA_INSTALL_PATH=/opt/ohpc/pub/apps/cuda-10.1
    ```
  > If you do not set CUDA_INSTALL_PATH, you'll see the compilation error related to `vector_types.h` later.
  
3. Source the `setup_environment.sh` located in the directory, gpu-simulator. 
    ```
    source gpu-simulator/setup_environment.sh
    ```
  > This will automatically clone the `accelsim_gpgpusim` repository from our git and switch its branch to **acpl_baseline**. 
  **Note** that the branches are separately managed between `accelsim_gpgpusim` and `accelsim_frontend` (current repo) because they are different repositories.
  
4. Compile the source code
    ```
    cd gpu-simulator
    make clean && make -j31 2> error.txt
    ```
  >Because a lot of outputs are generated when the compilation is in progress, redirect the standard error to the file. Later, use keyword `error` to find the compilation errors.

5. The binary file of accel-sim is generated (see gpu-simulator/bin/release/accel-sim.out).

# How to run

>At this point, please read the README file in the **acpl_baseline** branch of the tracer repository  before you dive into following steps.

The simulator receives 3 files to run the simulation:

1. Application trace file: `kernelslist.g`
2. GPU simulator configuration file: `gpgpusim.config` 
(e.g., located in gpu-simulator/gpgpu-sim/configs/tested-cfgs/SM75_RTX2060)
3. GPU simulator configuration file managed by accel-sim: `trace.config`
(e.g., located in gpu-simulator/configs/tested-cfgs/SM75_RTX2060)
 > Note that you need to use `gpgpusim.config` and `trace.config` for the same device (here, RTX2060).

Type the command:
```
gpu-simulator/gpgpu-sim/bin/release/accel-sim.out \
-trace path of kernelslist.g \
-config path of gpgpusim.config \
-config path of trace.config
```

