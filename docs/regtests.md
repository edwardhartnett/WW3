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


## What Next?

## References

https://github.com/NOAA-EMC/WW3/wiki/Developer-Guide#regression-testing-in-wavewatch-iii