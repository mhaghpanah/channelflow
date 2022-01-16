# Compiling Channelflow on Ganymede

## Loading the Modules

Several of the modules needed are in a non-standard location. To access these modules, set your `MODULEPATH` environment variable:

```bash
$ export MODULEPATH="/opt/ohpc/pub/circ/moduledeps/gnu8-mvapich2:/opt/ohpc/pub/circ/modulefiles:$MODULEPATH"
```

If you want these paths to be loaded automatically, add this line to your `.bashrc`. 

Once the correct `MODULEPATH` is set up, load the following modules:

```bash
$ module swap intel gnu8
$ module load cmake
$ module load eigen/3.3.9
$ module load netcdf4
$ module load fftw
```

You can save this module collection for easy loading later with

```bash
$ module save channelflow
```

This collection can be loaded again later with

```bash
$ module restore channelflow
```

## Compiling Channelflow

The flow is:
1. Create and enter a build directory
2. Set up your project with cmake
3. Make your project
4. Install your project

In the commands below, `-DCMAKE_INSTALL_PREFIX=$CF_INSTALL_DIR` is optional. If you want `channelflow` installed elsewere, create a directory and set `CF_INSTALL_DIR` to that directory using `export CF_INSTALL_DIR="/path/to/directory"`. 

```bash
$ mkdir build 
$ cd build
$ cmake .. -DCMAKE_C_COMPILER=mpicc -DCMAKE_CXX_COMPILER=mpicxx -DFFTW_LIBDIR=$FFTW_LIB -DNETCDF_DIR=$NETCDF_DIR -DWITH_NETCDF=Parallel -DCMAKE_INSTALL_PREFIX=$CF_INSTALL_DIR
```

After the `cmake` step, you should have a printout that says:

```
   ###############################################
   ###########  Configuration summary  ###########
   ###############################################
   Compiler:                       GNU 8.3.0
   Build type:                     Release
   Install prefix:                 <Your channelflow install dir>
   Building shared library:        yes
   Linking programs to:            shared library

   Libraries
   MPI:                            enabled
   netcdf (default file format):   enabled
   Parallel netcdf:                enabled
   HDF5 C++ (legacy file format):  disabled

   Channelflow components
   nsolver:                        enabled
   Python wrapper:                 disabled
   GTest unit testing:             enabled

```

Then, you build, test, and install:

```bash
$ make
$ make test
$ make install
```
