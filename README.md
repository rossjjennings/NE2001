# NE2001
This is a version of the NE2001 galactic electron density model (interactive version at https://www.nrl.navy.mil/rsd/RORF/ne2001/,
original Fortran code at http://hosting.astro.cornell.edu/~cordes/NE2001/).
It has been modified in a couple of small ways to get it to compile with modern versions of gfortran (tested with gfortran 11.3.0 on Ubuntu 22.04).

To build the executable, run `make pgm` from the `src.NE2001` subdirectory.
If all goes well, the `NE2001` binary should show up in the `bin.NE2001` subdirectory.
You can also build NE2001 as a library by running `make lib` or build both the library and executable by running `make all`. 

The original 2002 documentation can be found in `src.NE2001/code.pdf`. For convenience, it is also reproduced below.

## Compiling instructions (Unix/Linux)
1. Unpack the tar file through `tar xvf NE2001.tar` in the directory where you would like the code. The unpacked files should include:
```
src.NE2001/00README (plain ascii version of this description)
src.NE2001/Makefile
src.NE2001/NE2001.f
src.NE2001/code.ps (postscript version of this description)  
src.NE2001/code.dvi (dvi file of this description)  
src.NE2001/density.NE2001.f
src.NE2001/dmdsm.NE2001.f
src.NE2001/neLISM.NE2001.f
src.NE2001/neclumpN.f
src.NE2001/run_NE2001.pl
src.NE2001/scattering98.f
input.NE2001/gal01.inp
input.NE2001/ne_arms_log_mod.inp
input.NE2001/ne_gc.inp
input.NE2001/neclumpN.dat
input.NE2001/nelism.inp
input.NE2001/nevoidN.dat
```
2. Edit `Makefile` and set the value of `BINDIR` to the destination where you would like the executable to reside.  
3. Compile the code by typing `make`.  
4. Copy the files in `input.NE2001` to the directory where you will execute the program NE2001. The perl script `run NE2001.pl`, if used, also needs to be executed in the same directory containing the input files. The perl script needs to be edited to designate the location of the executable for NE2001.
