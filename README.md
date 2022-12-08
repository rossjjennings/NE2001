# NE2001
This is a version of the NE2001 galactic electron density model, developed in 2001 by James M. Cordes and T. Joseph W. Lazio and released on the arXiv at https://arxiv.org/abs/astro-ph/0207156 (original Fortran code at http://hosting.astro.cornell.edu/~cordes/NE2001/).
It has been modified in a couple of small ways to get it to compile with modern versions of gfortran (tested with gfortran 11.3.0 on Ubuntu 22.04).

## Compilation instructions (Unix/Linux)
1. Clone this repository:
```bash
git clone https://github.com/rossjjennings/NE2001.git
```
2. Change directories into the `src.NE2001` subdirectory of the resulting `NE2001` directory.
3. If you wish, edit `Makefile` and set the value of `BINDIR` to the destination where you would like the executable to reside. The default location is in the `bin.NE2001` subdirectory. 
4. To compile the code, make sure you are in the `src.NE2001` subdirectory and run `make all`. You can alternatively run `make pgm` to build the executable binary only, or `make lib` to build the library only. 
5. The NE2001 executable must be run from a directory containing the files in `input.NE2001`. This can be accomplished by copying the input files into the directory where you wish to run NE2001, or simply by running NE2001 from the `input.NE2001` directory. The Perl script `run_NE2001.pl`, if used, also needs to be executed in the directory containing the input files. Additionally, the Perl script needs to be edited to designate the location of the NE2001 executable.

Additional information (taken from the original 2002 documentation) can be found in DESCRIPTION.md or in `src.NE2001/code.pdf`.

