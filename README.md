# NE2001
This is a version of the NE2001 galactic electron density model (interactive version at https://www.nrl.navy.mil/rsd/RORF/ne2001/,
original Fortran code at http://hosting.astro.cornell.edu/~cordes/NE2001/).
It has been modified in a couple of small ways to get it to compile with modern versions of gfortran (tested with gfortran 7.4.0 on Ubuntu 18.04).

To build the executable, run `make pgm` from the `src.NE2001` subdirectory.
If all goes well, the `NE2001` binary should show up in the `bin.NE2001` subdirectory.
You can also build NE2001 as a library by running `make lib` or build both the library and executable by running `make all`. 

The original 2002 documentation can be found in `src.NE2001/code.pdf`.
