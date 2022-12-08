## Description of Fortran code
Our model is implemented in a set of Fortran routines that are similar in functionality to those presented in TC93, but represent a complete revision according to the new features presented in the main text. A master program `NE2001` evaluates the model by returning the integrated measures (DM, SM, etc.) and/or distance given an input direction and DM or distance. Integrations are performed in the the subroutine `dmdsm`, which evaluates the model by making calls to subroutine `density_2001`. Copies of all code and necessary input files are available over the Internet at www.astro.cornell.edu/cordes/NE2001 and NRL URL for mirror site here. The code is packaged as a 'tar' file, `NE2001.tar`, that includes a make file for compiling the code in a Unix/Linux environment.

A description of the functionality of the code is as follows: The call to `dmdsm` is of the form
```fortran
call dmdsm(l,b,ndir,dm,dist,limit,sm,smtau,smtheta,smiso) .
```
Here the input data include Galactic longitude and latitude `l` and `b` (in radians) and a flag `ndir` indicating whether distance is to be calculated from dispersion measure (`ndir` $\ge$ 0), or *vice-versa* (ndir < 0). In either case, `dm` and `dist` have units of $\mathrm{pc}\thinspace\mathrm{cm}^{-3}$ and $\mathrm{kpc}$, respectively. A flag `limit` is set if `ndir` $\ge$ 0 and the model distance is a lower limit; this will occur, for example, if a large `dm` is specified at high galactic latitude. The subroutine also returns four estimates of scattering measure, all having units $\mathrm{kpc}\thinspace\mathrm m^{-20/3}$. The first, `sm`, conforms to the definition $\mathrm{SM} = \int_0^D ds\ C^2_n(s)$ with uniform weighting along the line of sight. The next two estimates, `smtau` and `smtheta`, correspond to line-of-sight weightings appropriate for temporal and angular broadening of galactic sources, respectively. Temporal broadening emphasizes scattering material midway between source and observer, while angular broadening favors material closest to the observer; see Eqs. (A14, B2) of Cordes, Weisberg, & Boriakoff (1985). The last estimate, `smiso` uses the weighting appropriate for calculating the isoplanatic angle of scattering.

Integrations in `dmdsm` involve evaluations of the model at a given Galactic location $(x, y, z)$ through a call to subroutine `density_2001`, where `x`, `y`, `z` are Galactocentric Cartesian coordinates, measured in kiloparsecs, with the axes parallel to $(l, b) = (0\degree, 90\degree)$, $(180\degree, 0\degree)$, and $(0\degree, 90\degree)$;
```
call density_2001(x,y,z,
. ne1,ne2,nea,negc,nelism,necN,nevN,
. F1, F2, Fa, Fgc, Flism, FcN, FvN,
. whicharm, wlism, wldr, wlhb, wlsb, wloopI,  
. hitclump, hitvoid, wvoid).  
```
The routine returns values for the electron density in seven components (`ne1, ···, nevN`), the corresponding 'F' parameters (`F1, ···, FvN`), followed by a series of integer-valued flags. The meanings of these flags are as follows:
1. `whicharm = 0, ···, 5` indicates which spiral arm contributes to the density, with numbering as in the text and where a zero value denotes an interarm region.
2. `wlism`, `wldr`, `wlhb`, `wlsb`, and `wloopI` take on values of 0 or 1 as described in the Appendix of the paper describing NE2001.
3. `hitclump` denotes whether a clump has been hit; if so, then `hitclump` denotes the clump number in the table of clumps; if not, `hitclump = 0`.
4. `hitvoid` works in the same fashion for voids; additionally, `wvoid = 0,1` is used in evaluating the total density and indicates if a void has been hit (`wvoid = 1`).

The calling program is executed using command-line arguments that specify the galactic longitude and latitude, an input DM or distance value, and a flag (`ndir`) that specifies whether a distance is calculated from DM (`ndir` $\ge$ 0) or a DM calculated from an input distance (`ndir` < 0):
```
Usage: NE2001 l b DM/D ndir  
       l (deg)  
       b (deg)  
       DM/D (pc cm^{-3} or kpc)  
       ndir = 1 (DM->D)    -1 (D->DM)  
```

Program `NE2001` uses output from `dmdsm` to calculate scattering and scintillation quantities by making suitable calls to a series of functions. In all cases, input distances, scattering measures, frequencies and velocities are in standard units ( $\mathrm{kpc}$, $\mathrm{kpc}\thinspace\mathrm{m}^{-20/3}$, $\mathrm{GHz}$ and $\mathrm{km}\thinspace\mathrm{s}^{-1}$ ):
1. `function tauiss(d,sm,nu)`: calculates the pulse broadening time, $\tau_d$ (ms).
2. `function scintbw(d,sm,nu)`: calculates the scintillation bandwidth, $\Delta\nu_{\mathrm{d}}$ (MHz).
3. `function scintime(sm,nu,vperp)`: calculates the scintillation time, $\Delta t_{\mathrm{ISS}}$ (sec) (Cordes & Lazio 1991; Cordes & Rickett 1998).
4. `function specbroad(sm,nu,vperp)`: calculates the spectral broadening, $\Delta\nu_{\mathrm{b}}$ (Hz), that is proportional to the reciprocal of the scintillation time (Cordes & Lazio 1991).
5. `function theta_xgal(sm,nu)`: calculates the angular broadening, $\theta_d$ (mas), appropriate for the scattering geometry for an extragalactic source (mas).
6. `function theta_gal(sm,nu)`: calculates the angular broadening, $\theta_d$ (mas), of a Galactic source (mas).
7. `function em(sm)`: calculates the emission measure, EM ( $\mathrm{pc}\thinspace\mathrm{cm}^{-6}$ ), associated with the scattering measure; note that the value calculated assumes a particular outer scale for a Kolmogorov wavenumber spectrum and represents a lower bound on EM (see text).
8. `function theta_iso(smiso,nu)`: calculates the isoplanatic angle, $\theta_{\mathrm{iso}}$ (mas), the region on the sky over which scintillations are correlated.  
9. `function transition_frequency(sm,smtau,smtheta,dintegrate)`: calculates the frequency of transition,  $\nu_{\mathrm{trans}}$ (GHz), between the weak and strong scattering regimes.

Sample output for $\ell=45\degree$, $b=5\degree$, and $\mathrm{DM}=50\ \mathrm{pc}\thinspace\mathrm{cm}^{-3}$ is:
```
NE2001.new 45 5 50 1
#NE2001 input: 4 parameters  
  45.0000         l         (deg)                    GalacticLongitude  
   5.0000         b         (deg)                    GalacticLatitude  
  50.0000         DM/D      (pc-cm^{-3}_or_kpc)      Input_DM_or_Distance  
        1         ndir      1:DM->D;-1:D->DM         Which?(DM_or_D)  
#NE2001 output: 14 values  
   2.6365         DIST      (kpc)                    ModelDistance  
  50.0000         DM        (pc-cm^{-3})             DispersionMeasure  
   4.3578         DMz       (pc-cm^{-3})             DM_Zcomponent  
0.3528E-03        SM        (kpc-m^{-20/3})          ScatteringMeasure  
0.2367E-03        SMtau     (kpc-m^{-20/3})          SM_PulseBroadening  
0.7719E-04        SMtheta   (kpc-m^{-20/3})          SM_GalAngularBroadening  
0.1307E-02        SMiso     (kpc-m^{-20/3})          SM_IsoplanaticAngle  
0.1921E+00        EM        (pc-cm^{-6})             EmissionMeasure_from_SM  
0.1293E-03        TAU       (ms)                     PulseBroadening @1GHz  
0.1428E+01        SBW       (MHz)                    ScintBW @1GHz  
0.4943E+03        SCINTIME  (s)                      ScintTime @1GHz @100 km/s  
0.2420E+00        THETA_G   (mas)                    AngBroadeningGal @1GHz  
0.1086E+01        THETA_X   (mas)                    AngBroadeningXgal @1GHz  
    14.02         NU_T      (GHz)                    TransitionFrequency
```
The Fortran program can be run using a perl script and where an individual field can be selected for output:
```
run_NE2001.pl
Usage:  
   run_NE2001      l    b       DM/D         ndir    field    
                  deg  deg    pc-cm^{-3}     1,-1    D etc    
                              or kpc    
   Possible Fields (case insensitive):  
   Dist, DM, SM, EM, TAU, SBW, SCINTIME, THETA_G, THETA_X, NU_T, ALL
```
