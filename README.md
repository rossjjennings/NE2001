# NE2001
This is a version of the NE2001 galactic electron density model (interactive version at https://www.nrl.navy.mil/rsd/RORF/ne2001/,
original Fortran code at http://hosting.astro.cornell.edu/~cordes/NE2001/).
It has been modified in a couple of small ways to get it to compile with modern versions of gfortran (tested with gfortran 11.3.0 on Ubuntu 22.04).

To build the executable, run `make pgm` from the `src.NE2001` subdirectory.
If all goes well, the `NE2001` binary should show up in the `bin.NE2001` subdirectory.
You can also build NE2001 as a library by running `make lib` or build both the library and executable by running `make all`. 

## Compilation instructions (Unix/Linux)
1. Clone this repository by executing the command
```bash
git clone https://github.com/rossjjennings/NE2001.git
```
2. If you wish, edit `Makefile` and set the value of `BINDIR` to the destination where you would like the executable to reside. The default location is in the `bin.NE2001` subdirectory. 
3. To compile the code, `cd` into the `src.NE2001` subdirectory and type `make`.  
4. Copy the files in `input.NE2001` to the directory where you will execute the program NE2001. The perl script `run_NE2001.pl`, if used, also needs to be executed in the same directory containing the input files. The perl script needs to be edited to designate the location of the executable for NE2001.

Additional information (taken from the original 2002 documentation) can be found in DESCRIPTION.md or in `src.NE2001/code.pdf`.

