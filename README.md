**Instructions and hints on how to run for the MPI course**

# Where to run

The exercises will be run on PDC's CRAY XC-40 system [Beskow](https://www.pdc.kth.se/hpc-services/computing-systems):

```
beskow.pdc.kth.se
```

# How to login

To access PDC's cluster you should use your laptop and the Eduroam or KTH Open wireless networks.

[Instructions on how to connect from various operating systems](https://www.pdc.kth.se/support/documents/login/login.html).


# More about the environment on Beskow

The Cray automatically loads several [modules](https://www.pdc.kth.se/support/documents/run_jobs/job_scheduling.html#accessing-software) at login.

- Heimdal - [Kerberos commands](https://www.pdc.kth.se/support/documents/login/login.html#general-information-about-kerberos)
- OpenAFS - [AFS commands](https://www.pdc.kth.se/support/documents/data_management/afs.html)
- SLURM -  [batch jobs](https://www.pdc.kth.se/support/documents/run_jobs/queueing_jobs.html) and [interactive jobs](https://www.pdc.kth.se/support/documents/run_jobs/run_interactively.html)
- Programming environment - [Compilers for software development](https://www.pdc.kth.se/support/documents/software_development/development.html)

# Compiling MPI programs on Beskow

By default the cray compiler is loaded into your environment. In order to use another compiler you have to swap compiler modules:

```
module swap PrgEnv-cray PrgEnv-gnu
```
or
```
module swap PrgEnv-cray PrgEnv-intel
```

On Beskow one should always use the *compiler wrappers* `cc`, `CC` or 
`ftn` (for C, C++ and Fortran codes, respectively), 
which will automatically link to MPI libraries and linear 
algebra libraries like BLAS, LAPACK, etc.

Examples:

```
# Fortran
ftn [flags] source.f90
# C
cc [flags] source.c
# C++
CC [flags] source.cpp
```

Note: if you are using the Intel Programming Environment, and 
if you are compiling C code, you might see error messages containing:

```
error: identifier "_Float128" is undefined
```

A workaround is to add a compiler flag:

```
cc -D_Float128=__float128 source.c
```

# Running MPI programs on Beskow

First it is necessary to book a node for interactive use:

```
salloc -A <allocation-name> -N 1 -t 1:0:0
```

You might also need to specify a reservation by adding the flag 
`--reservation=<name-of-reservation>`.

Then the srun command is used to launch an MPI application:

```
srun -n 32 ./example.x
```

In this example we will start 32 MPI tasks (there are 32 cores per node on the Beskow nodes).

If you do not use srun and try to start your program on the login node then you will get an error similar to

```
Fatal error in MPI_Init: Other MPI error, error stack:
MPIR_Init_thread(408): Initialization failed
MPID_Init(123).......: channel initialization failed
MPID_Init(461).......:  PMI2 init failed: 1
```


# MPI Exercises

- MPI Lab 1: [Program Structure and Point-to-Point Communication in MPI](lab1/README.md)
- MPI Lab 2: [Collective and Non-Blocking Communication](lab2/README.md)
- MPI Lab 3: [Advanced Topics](lab3/README.md)
