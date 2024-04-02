# How to Run the Regtests

## Set Up Model Environment

Copy and rename the switch file:
cp /home/ed/WW3/regtests/unittests/data/switch.io model/switch_IO

Run this command:

./model/bin/w3_setup /home/ed/WW3/model -c tmpl -s IO
 
                *****************************
              ***   WAVEWATCH III setup     ***
                *****************************
 
 
[INFO] local env file wwatch3.env found in /home/ed/WW3/model/bin/wwatch3.env
   Setup file /home/ed/WW3/model/bin/wwatch3.env found
      Source directory            : /home/ed/WW3/model
      Scratch directory           : /home/ed/WW3/model/tmp
      Save source code            : yes
      Save listings               : yes
   Update settings ? [y/n] y
 
   Creating new set-up :
 
      Scratch space [/home/ed/WW3/model/tmp] : 
      Save source code files (*.f)  [yes] : 
      Save listing files  [yes] : 
 
   Modified set up :
      Scratch directory        : /home/ed/WW3/model/tmp
      Save sources             : yes
      Save listings            : yes
   New settings OK ? [y/n] y
 
   Setup comp & link files
      copy /home/ed/WW3/model/bin/comp.tmpl => /home/ed/WW3/model/bin/comp
      copy /home/ed/WW3/model/bin/link.tmpl => /home/ed/WW3/model/bin/link
      copy /home/ed/WW3/model/bin/ad3.tmpl => /home/ed/WW3/model/bin/ad3
 
   Setup switch file
      /home/ed/WW3/model/bin/switch_IO => /home/ed/WW3/model/bin/switch
 
   Create required model subdirectories
 
Finished setting up WAVEWATCH III
 

## Compile WW3

Install dependencies with spack as shown in .github/workflow/io_gnu.yml.

Build WW3 binaries like this:

cmake -DSWITCH=/home/ed/ww3/regtests/unittests/data/switch.io  -DCMAKE_BUILD_TYPE=Debug -DCMAKE_Fortran_FLAGS="-g" -DCMAKE_C_FLAGS="-g" ..
make VERBOSE=1 -j2
make test

## Download Test Data

Download the test data by running the shell script: model/bin/ww3_from_ftp.sh.

## Run Some Stuff

In the build directory there is a file ww3_grid.inp. In the build directory execute:
<pre>
./bin/ww3_grid
</pre>

Expect the following output:

<pre>

                   *** WAVEWATCH III Grid preprocessor ***    
               ===============================================

  Comment character is '$'

  Grid name : 1-D REFRACTION X              


  Spectral discretization : 
 --------------------------------------------------
       Number of directions        :  24
       Directional increment (deg.):  15.0
       First direction       (deg.):   0.0
       Number of frequencies       :   3
       Frequency range        (Hz) :   0.0800-0.1250
       Increment factor            :   1.250


  Model definition :
 --------------------------------------------------
       Dry run (no calculations)   :  ---/NO
       Propagation in X-direction  :  YES/--
       Propagation in Y-direction  :  ---/NO
       Refraction                  :  YES/--
       Current-induced k-shift     :  ---/NO
       Source term calc. and int.  :  ---/NO


  Time steps : 
 --------------------------------------------------
       Maximum global time step      (s) :  300.00
       Maximum CFL time step X-Y     (s) :  300.00
       Maximum CFL time step k-theta (s) :  150.00
       Minimum source term time step (s) :  300.00

  Preprocessing namelists ...
  Preprocessing namelists finished.


  Stresses (T&C 96)
 --------------------------------------------------


  Linear input not defined.


  Wind input not defined.


  Nonlinear interactions not defined.


  Dissipation not defined.


  Bottom friction not defined.


  Surf breaking not defined.


  Triad interactions not defined.


  Bottom scattering not defined.


  Propagation scheme : 
 --------------------------------------------------
       Type of scheme (structured) : First order upstream          
                                     (user def. values)
       CFLmax depth refraction     :    0.750


  Ice scattering not defined.


  Spectral output on full grid (default values) :  
 --------------------------------------------------
       Second order pressure at K=0:   0   1   3
       Spectrum of Uss             :   0   1   3
       Frequency spectrum          :   0   1   3
       Partions of Uss             :   0   1
       Partition wavenumber # 1   :  0.063

  Miscellaneous (default values) :  
 --------------------------------------------------
       Ice concentration cut-offs  :    0.50  0.50
       Wind input reduction factor in presence of 
         ice :  1.00
         (0.0==> no reduction and 1.0==> no wind
         input with 100% ice cover)
       Space-time extremes DX-Y set to default 1000 m
       Space-time extremes Dt set to default 1200 s
       Compression of track output  :   T

    Dynamic source term integration scheme :
       Xp                      (-) :    0.150
       Xr                      (-) :    0.100
       Xfilt                   (-) :    0.050

    Wave field partitioning :
       Levels                  (-) :  100
       Minimum wave height     (m) :    0.050
       Wind area multiplier    (-) :    1.700
       Cut-off wind sea fract. (-) :    0.333
       Combine wind seas           :  YES/--
       Number of swells in fld out :    5
       Partitioning method         :  WW3 default                                  

    Miche-style limiting wave height :
       Hs,max/d factor         (-) :    1.600
       Hrms,max/d factor       (-) :    1.131
       Limiter activated           :  ---/NO

    Calendar type                  :  standard


  Equivalent namelists ...

  &PRO1 CFLTM = 0.75 /
  &UNST UGBCCFL =  T, UGOBCAUTO =  T, UGOBCDEPTH = -10.000, UGOBCFILE=unset,
,  EXPFSN =  T,EXPFSPSI =  F,  EXPFSFCT =  F,IMPFSN =  F,EXPTOTAL=  F,  IMPTOTAL=  F,IMPREFRACTION=  F,  IMPFREQSHIFT=  F, IMPSOURCE=  F,  SETUP_APPLY_WLV=  T,  JGS_TERMINATE_MAXITER=  T,  JGS_TERMINATE_DIFFERENCE=  T,  JGS_TERMINATE_NORM=  F,  JGS_LIMITER=  F,  JGS_LIMITER_FUNC=  1,  JGS_USE_JACOBI=  T,  JGS_BLOCK_GAUSS_SEIDEL=  T,  JGS_MAXITER=  100,  JGS_PMIN=   1.000,  JGS_DIFF_THR=   0.000,  JGS_NORM_THR=   0.000,  JGS_NLEVEL=  0,  JGS_SOURCE_NONLINEAR=  F

  &OUTS P2SF  = 0, I1P2SF = 1, I2P2SF = 15,
        US3D  = 0, I1US3D =  1, I2US3D =  3,
        USSP  = 0, IUSSP  =  1,
        E3D   = 0, I1E3D  =  1, I2E3D  =  3,
        TH1MF = 0, I1TH1M =  1, I2TH1M =  3,
        STH1MF= 0, I1STH1M=  1, I2STH1M=  3,
        TH2MF = 0, I1TH2M =  1, I2TH2M =  3,
        STH2MF= 0, I1STH2M=  1, I2STH2M=  3 /
  &MISC CICE0 = 0.500, CICEN = 0.500, LICE =      0.0, PMOVE = 0.500,
        XSEED = 1.000, FLAGTR = 0, XP = 0.150, XR = 0.100, XFILT = 0.050
        IHM =  100, HSPM = 0.050, WSM = 1.700, WSC = 0.333, FLC = .TRUE.
        NOSW =  5, FMICHE = 1.600, RWNDC = 1.000, WCOR1 = 99.00, WCOR2 =  0.00,
        FACBERG = 1.0, GSHIFT =   0.000E+00, STDX =   -1.00, STDY =  -1.00,
        STDT =   -1.00, ICEHMIN = 0.20, ICEHFAC = 1.00,
        ICEHINIT = 0.50, ICEDISP =  F, ICEHDISP = 0.60,
        ICESLN =   1.00, ICEWIND =   1.00, ICESNL =   1.00, ICESDS =  1.00,
        ICEDDISP = 80.00, ICEFDISP =  2.00, CALTYPE = standard , TRCKCMPR =   T,
        BTBET  =   1.20 /

  Equivalent namelists finished.


  The spatial grid: 
 --------------------------------------------------

       Grid type                   : rectilinear
       Coordinate system           : Cartesian
       Index closure type          : none
       Dimensions                  :     13       3
       Increments             (km) :    5.00    5.00
       X range                (km) :   -5.00   55.00
       Y range                (km) :   -5.00    5.00

       Bottom level unit           :    10
       Limiting depth          (m) :   -1.00
       Minimum depth           (m) :    1.00
       Scale factor                :   -1.00
       Layout indicator            :     2
       Format indicator            :     1

       Sub-grid information        : Not available.
  Processing boundary points
  Processing excluded points

  Input boundary points : 
 --------------------------------------------------
       Number of boundary points   :     1

         Nr.|   IX  |   IY  |     X     |     Y     
       -----|-------|-------|-----------|-----------
          1 |     2 |     2 |     0.0E3 |     0.0E3

  Excluded points : 
 --------------------------------------------------
       Number of excluded points   :    25


  Status map, printed in     1 part(s) 
 -----------------------------------

   3 3 3 3 3 3 3 3 3 3 3 3 0
   3 2 1 1 1 1 1 1 1 1 1 1 0
   3 3 3 3 3 3 3 3 3 3 3 3 0
  
  Legend : 
 -----------------------------
    0 : Land point            
    1 : Sea point             
    2 : Active boundary point 
    3 : Excluded point        


  Output boundary points : 
 --------------------------------------------------
       No boundary points.


  Writing model definition file ...


  Summary grid statistics : 
 --------------------------------------------------
       Number of longitudes      :        13
       Number of latitudes       :         3
       Number of grid points     :        39
       Number of sea points      :        11 (28.2%)
       Number of input b. points :         1
       Number of land points     :         3
       Number of excluded points :        25


  End of program 
 ========================================
         WAVEWATCH III Grid preprocessor 
</pre>


## Running a Regtest

<pre>

ed@Pooh-Bah:~/ww3/regtests/ww3_tp1.1/input$ ../../../build/bin/ww3_grid 

                   *** WAVEWATCH III Grid preprocessor ***    
               ===============================================

  Grid name : 1-D PROPAGATION EQUATOR       


  Spectral discretization : 
 --------------------------------------------------
       Number of directions        :   4
       Directional increment (deg.):  90.0
       First direction       (deg.):   0.0
       Number of frequencies       :   3
       Frequency range        (Hz) :   0.0368-0.0445
       Increment factor            :   1.100


  Model definition :
 --------------------------------------------------
       Dry run (no calculations)   :  ---/NO
       Propagation in X-direction  :  YES/--
       Propagation in Y-direction  :  ---/NO
       Refraction                  :  ---/NO
       Current-induced k-shift     :  ---/NO
       Source term calc. and int.  :  ---/NO


  Time steps : 
 --------------------------------------------------
       Maximum global time step      (s) : 3600.00
       Maximum CFL time step X-Y     (s) : 3600.00
       Maximum CFL time step k-theta (s) : 3600.00
       Minimum source term time step (s) : 3600.00

  Preprocessing namelists ...
  Preprocessing namelists finished.


  Stresses (T&C 96)
 --------------------------------------------------


  Linear input not defined.


  Wind input not defined.


  Nonlinear interactions not defined.


  Dissipation not defined.


  Bottom friction not defined.


  Surf breaking not defined.


  Triad interactions not defined.


  Bottom scattering not defined.


  Propagation scheme : 
 --------------------------------------------------
       Type of scheme (structured) : First order upstream          
                                     (default values)  
       CFLmax depth refraction     :    0.700


  Ice scattering not defined.


  Spectral output on full grid (default values) :  
 --------------------------------------------------
       Second order pressure at K=0:   0   1   3
       Spectrum of Uss             :   0   1   3
       Frequency spectrum          :   0   1   3
       Partions of Uss             :   0   1
       Partition wavenumber # 1   :  0.063

  Miscellaneous (default values) :  
 --------------------------------------------------
       Ice concentration cut-offs  :    0.50  0.50
       Wind input reduction factor in presence of 
         ice :  1.00
         (0.0==> no reduction and 1.0==> no wind
         input with 100% ice cover)
       Space-time extremes DX-Y set to default 1000 m
       Space-time extremes Dt set to default 1200 s
       Compression of track output  :   T

    Dynamic source term integration scheme :
       Xp                      (-) :    0.150
       Xr                      (-) :    0.100
       Xfilt                   (-) :    0.050

    Wave field partitioning :
       Levels                  (-) :  100
       Minimum wave height     (m) :    0.050
       Wind area multiplier    (-) :    1.700
       Cut-off wind sea fract. (-) :    0.333
       Combine wind seas           :  YES/--
       Number of swells in fld out :    5
       Partitioning method         :  WW3 default                                  

    Miche-style limiting wave height :
       Hs,max/d factor         (-) :    1.600
       Hrms,max/d factor       (-) :    1.131
       Limiter activated           :  ---/NO

    Calendar type                  :  standard


  Equivalent namelists ...

  &PRO1 CFLTM = 0.70 /
  &UNST UGBCCFL =  T, UGOBCAUTO =  T, UGOBCDEPTH = -10.000, UGOBCFILE=unset,
,  EXPFSN =  T,EXPFSPSI =  F,  EXPFSFCT =  F,IMPFSN =  F,EXPTOTAL=  F,  IMPTOTAL=  F,IMPREFRACTION=  F,  IMPFREQSHIFT=  F, IMPSOURCE=  F,  SETUP_APPLY_WLV=  T,  JGS_TERMINATE_MAXITER=  T,  JGS_TERMINATE_DIFFERENCE=  T,  JGS_TERMINATE_NORM=  F,  JGS_LIMITER=  F,  JGS_LIMITER_FUNC=  1,  JGS_USE_JACOBI=  T,  JGS_BLOCK_GAUSS_SEIDEL=  T,  JGS_MAXITER=  100,  JGS_PMIN=   1.000,  JGS_DIFF_THR=   0.000,  JGS_NORM_THR=   0.000,  JGS_NLEVEL=  0,  JGS_SOURCE_NONLINEAR=  F

  &OUTS P2SF  = 0, I1P2SF = 1, I2P2SF = 15,
        US3D  = 0, I1US3D =  1, I2US3D =  3,
        USSP  = 0, IUSSP  =  1,
        E3D   = 0, I1E3D  =  1, I2E3D  =  3,
        TH1MF = 0, I1TH1M =  1, I2TH1M =  3,
        STH1MF= 0, I1STH1M=  1, I2STH1M=  3,
        TH2MF = 0, I1TH2M =  1, I2TH2M =  3,
        STH2MF= 0, I1STH2M=  1, I2STH2M=  3 /
  &MISC CICE0 = 0.500, CICEN = 0.500, LICE =      0.0, PMOVE = 0.500,
        XSEED = 1.000, FLAGTR = 0, XP = 0.150, XR = 0.100, XFILT = 0.050
        IHM =  100, HSPM = 0.050, WSM = 1.700, WSC = 0.333, FLC = .TRUE.
        NOSW =  5, FMICHE = 1.600, RWNDC = 1.000, WCOR1 = 99.00, WCOR2 =  0.00,
        FACBERG = 1.0, GSHIFT =   0.000E+00, STDX =   -1.00, STDY =  -1.00,
        STDT =   -1.00, ICEHMIN = 0.20, ICEHFAC = 1.00,
        ICEHINIT = 0.50, ICEDISP =  F, ICEHDISP = 0.60,
        ICESLN =   1.00, ICEWIND =   1.00, ICESNL =   1.00, ICESDS =  1.00,
        ICEDDISP = 80.00, ICEFDISP =  2.00, CALTYPE = standard , TRCKCMPR =   T,
        BTBET  =   1.20 /

  Equivalent namelists finished.


  The spatial grid: 
 --------------------------------------------------

       Grid type                   : rectilinear
       Coordinate system           : spherical
       Index closure type          : simple
       Dimensions                  :    360       3

       Increments           (deg.) :    1.0000    1.0000
       Longitude range      (deg.) : -180.0000  179.0000
       Latitude range       (deg.) :   -1.0000    1.0000

       Bottom level unit           :    50
       Limiting depth          (m) :   -5.00
       Minimum depth           (m) :    5.75
       Scale factor                :-2500.00
       Layout indicator            :     2
       Format indicator            :     1
       File name                   : ../input/1-D.depth

       Sub-grid information        : Not available.
  Processing boundary points
  Processing excluded points

  Input boundary points : 
 --------------------------------------------------
       No boundary points.


  Excluded points : 
 --------------------------------------------------
       Number of excluded points   :   720


  Status map, printed in     5 part(s) 
 -----------------------------------

   3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3
   1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1
   3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3
  
   3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3
   1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1
   3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3
  
   3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3
   1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1
   3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3
  
   3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3
   1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1
   3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3
  
   3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3
   1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1
   3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3
  
  Legend : 
 -----------------------------
    0 : Land point            
    1 : Sea point             
    2 : Active boundary point 
    3 : Excluded point        


  Output boundary points : 
 --------------------------------------------------
       No boundary points.


  Writing model definition file ...


  Summary grid statistics : 
 --------------------------------------------------
       Number of longitudes      :       360
       Number of latitudes       :         3
       Number of grid points     :      1080
       Number of sea points      :       360 (33.3%)
       Number of input b. points :         0
       Number of land points     :         0
       Number of excluded points :       720


  End of program 
 ========================================
         WAVEWATCH III Grid preprocessor 

ed@Pooh-Bah:~/ww3/regtests/ww3_tp1.1/input$ ../../../build/bin/ww3_strt 

                  *** WAVEWATCH III  Initial conditions ***   
               ===============================================

  Comment character is '$'

  Grid name : 1-D PROPAGATION EQUATOR       


  Initial field ITYPE = 1
 --------------------------------------------------
  Negative SIX was provided by user.         
  WW3 will create a gaussian distribution    
  that is circular in real space. 
       Gaussian / cosine power spectrum 

       Peak frequency and spread (Hz)    :    0.0405  0.0001
       Mean direction (Naut., degr.)     :  270.0
       Cosine power of dir. distribution :  200
       Mean longitude and spread (degr.) :    0.00    0.01
       Mean latitude and spread (degr.)  :    0.00    2.00
       Maximum wave height               :    2.50


  Location : TEST E(f)
  Spectrum : Unscaled 1-D  Extreme value :  0.100E+01  

           +-------+
 0.990E+00 +   *   |
           |       |
 0.770E+00 +       |
           |       |
 0.550E+00 +       |
           |       |
 0.330E+00 +       |
           |       |
 0.110E+00 +       |
           +-*-|-*-+
             0.040
 

  Location : TEST 2-D
  Spectrum : Energy (Normalized)   Maximum value :  0.706E+01 m2s

       ang.|  frequencies (Hz) 
       deg.| 0.040
       ----+---|---+
         N |       |
         E |       |
         S |       |
         W |   *   |
       ----+-------+
 

       Converting energy to action ... 

 Variable: Hs Max.:  0.250E+01 m
 
             1      23      45      67      89     111     133     155     177     199     221     243     265     287     309     331     353
       +-------------------------------------------------------------------+
     3 |                                                                   |
       | 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 3 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 |
       |                                                                   |
       +-------------------------------------------------------------------+
             1      23      45      67      89     111     133     155     177     199     221     243     265     287     309     331     353
 

       Writing restart file  ... 


  End of program 
 =========================================
         WAVEWATCH III Initial conditions 

ed@Pooh-Bah:~/ww3/regtests/ww3_tp1.1/input$ ../../../build/bin/ww3_prep 

                 *** WAVEWATCH III  Input pre-processing ***  
               ===============================================


 *** WAVEWATCH III ERROR IN W3PREP : 
     ERROR IN OPENING INPUT FILE
     IOSTAT =    2
</pre>

## Running WW3

These are the steps:
* ww3_grid (generate mod_def.ww3 file).
* ww3_strt (generate initial conditions).
* ww3_prep (generate ice.ww3 file).
* ww3_shel (run the model).
* ww3_outf (print out wave heights).
* ww3_outp (print out some spectra).

## run_cmake_test

There is a script called run_cmake_test. It seems relevant:

<pre>
ed@Pooh-Bah:~/ww3/regtests$ ./bin/run_cmake_test ~/ww3 ww3_tp1.1
 
 Running now options: run_cmake_test /home/ed/ww3 ww3_tp1.1
 

ERROR: /home/ed/ww3/tools not found
</pre>

## References

https://github.com/NOAA-EMC/WW3/wiki/Developer-Guide#regression-testing-in-wavewatch-iii

WaveWatch III Installation, retrieved from https://www.youtube.com/watch?v=cyyIKqm9R2s&t=1s on Apr 2, 2024.

file:///home/ed/Downloads/10.WAVEWATCHIII_install.Tolman.pdf