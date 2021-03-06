# larcv2_to_larcv3
Conversion tools to turn larcv2 (ROOT) files into larcv3 (hdf5) files

This tool is meant to be built and run as an executable.  You need both larcv2 and larcv3 installed side by side to run this software, which can be a challenge.  

This tool is not without a little intervention, because it is not as user friendly as larcv2 or larcv3 themselves.  In particular, take a look at the `Makefile`: at the top are several variables you will need to change to compile.  Use the compiler that you used for larcv2 and for larcv3 as `CC`, and make sure to explicitly get the H5 include and lib directories.  If you aren't sure what they are, you can find out on the command line:

```h5c++ -show```

```h5c++ -showconfig```

Once the makefile is updated, you should run `make` and it will compile the converter and link it to larcv2 and larcv3 libraries.

## Converting a file

After you build the software, you should have `larcv2_to_larcv3.so` in the directory.  When you try to run, however, it may crash with an `image not found` error since this library isn't on any sort of path.  You might also get this problem if it can't find the larcv3 library.  The way to fix this is straightfoward:

Open python with PYTHONPATH unset:  `PYTHONPATH="" python`
Then, import larcv and call a function: 
```
import larcv
print(larcv.get_lib_dir())
```

This will output a long path that goes through python libraries, and should end with ...egg/larcv/lib/
Add this whole path (ending in .../larvc/lib) to the LD_LIBRARY_PATH variable and it should be found at run time.

You can use the python wrapper to run the converter - all of the conversion will be driven in C++ for speed, but the python wrapper let's you configure input/output and number events.

You can control the arguments with python command line args:
 - `-il` or `--input-larcv` points to the larcv2 file you want to convert.
 - `-ol` or `--output-larcv` is the output larcv3 file.  If not provided, it will be the same file with `.root` replaced with `.h5`.
 - `-nevents` controls how many events to convert.
 - `-nskip` controls how many events to skip before starting to convert.

It's recommended you use the viewers to analyze the output of the conversion and make sure things are good: https://github.com/DeepLearnPhysics/larcv-viewer.  By default, the viewer works with larcv3, but there is a larcv2 tag (no longer being developed) that will show larcv2 files.


# Problems
If you have problems building, running, or think there is a conversion bug, please open an issue!
