missing .env file in .vscode may has well have an empty one. Fixed the issue preventing startup

`R CMD check`

* checking for file ‘vscode-rcpp-demo/DESCRIPTION’ ... ERROR
Required fields missing or empty:
  ‘Author’ ‘Maintainer’


`R CMD INSTALL`

```
** R
** byte-compile and prepare package for lazy loading
Error in rbind(info, getNamespaceInfo(env, "S3methods")) : 
  number of columns of matrices must match (see arg 2)
ERROR: lazy loading failed for package ‘VSCodeRcppDemo’
* removing ‘/home/pblah/.local/lib/R/site-library/VSCodeRcppDemo’
```

trying the `F5` func key has the same result.

Trying to install R 3.6 from source on debian

Note: after `./configure` I notice I probably need to first do `conda deactivate` as libcurl is screwed up visibly because of conda.

Trying now after R 3.6 and Rcpp reinstall, but `/usr/local/lib/R/bin/exec/R: error while loading shared libraries: libRblas.so: cannot open shared object file: No such file or directory`. Suspect need a restart of the process.

```
Warning: Debuggee TargetArchitecture not detected, assuming x86_64.
=cmd-param-changed,param="pagination",value="off"
Stopped due to shared library event (no libraries added or removed)
Loaded '/lib64/ld-linux-x86-64.so.2'. Symbols loaded.
[Inferior 1 (process 19550) exited with code 0177]
The program '/usr/local/lib/R/bin/exec/R' has exited with code 177 (0x000000b1).
```

and nope, still `/usr/local/lib/R/bin/exec/R: error while loading shared libraries: libRblas.so: cannot open shared object file: No such file or directory`

`/usr/local/lib/R/lib/libRblas.so`
`sudo ldconfig`
but still `ldd /usr/local/lib/R/bin/exec/R`
```
      linux-vdso.so.1 (0x00007ffee5d87000)
      libRblas.so => not found
```

Hunch I had from rClr work,the R script updates the env var `LD_LIBRARY_PATH` 

`export LD_LIBRARY_PATH=/usr/local/lib/R/lib:/usr/local/lib`
