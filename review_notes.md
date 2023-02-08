# Installation

Installation failed on my M1 Mac.
```
ld: warning: directory not found for option '-L/opt/R/arm64/gfortran/lib/gcc/aarch64-apple-darwin20.6.0/12.0.1'
ld: warning: directory not found for option '-L/opt/R/arm64/gfortran/lib'
ld: library not found for -lgfortran
clang: error: linker command failed with exit code 1 (use -v to see invocation)
make: *** [dfms.so] Error 1
ERROR: compilation failed for package ‘dfms’
```

Problem: no fortran compiler for M1 Macs in XCode. Problem discussed here: https://mac.r-project.org/tools/

Solution posted here https://github.com/RubD/Giotto_site/issues/11
```
# for R>=4.2.0
curl -O https://mac.r-project.org/tools/gfortran-12.0.1-20220312-is-darwin20-arm64.tar.xz

# unpack
sudo tar fxz gfortran-12.0.1-20220312-is-darwin20-arm64.tar.xz -C /

# /opt/R/arm64/gfortran/SDK has to point to your macOS SDK
# EEH: this didn't work for me but was able to install anyhow and run example
sudo gfortran-update-sdk
```

Notes as I am working

- [ ] Readme and vignettes code examples use "=" but R style uses "<-" for assignment.
- [ ] https://sebkrantz.github.io/dfms/reference/DFM.html says it returns a class dfm which is "list-like". like? I looks like a list not "list-like". If it is a list then set class to c("dfm", "list") so that list methods recognize it as a list.
- [ ] `?dfms` did not pull up the help page. Is there no help page for the package?
- [ ] https://sebkrantz.github.io/dfms/reference/SKFS.html
The covariance equation has a typo. I think you meant F^{smooth}_t and left out the { }


## Kalman filter is not only for stationary data

https://sebkrantz.github.io/dfms/reference/SKF.html
https://sebkrantz.github.io/dfms/reference/SKFS.html

The Kalman filter and smoother are not restricted to stationary data. A can be identity (random walks).

You have to know a lot about state-space models and the Kalman filter and smoother to understand the documentation. However it does cite the references for the user.

## Add mention of the data assumptions

The model is stationary but no stationarity tests for the data or mention of the importance of this. Also the data must have 0 mean due to no mean in the model.

I would make this quite clear for the user and point them towards tests for stationarity.

## no coef() and logLik() functions

Not big deal but I would expect those for a statistics package.

## class for DFA output

The class is shown as "dfm", but it appears to be a list also. In this case, one should prepend "dfm" but keep "list" also so that list methods work.

## The method of imputation for X_imp is not given

https://sebkrantz.github.io/dfms/reference/DFM.html

In the value section, X_imp is one of the values returned, but how it is imputed is not given. Is this the Kalman smoother output from the estimated model? Or something else.

## No info on the residuals

No information on what type of residuals are returned. Link is to the vars residuals() function, but that has no information either. I am assuming that the economics DFA field, there are a standard type of residuals being used, but would be helpful to say which ones.

*I'll add a list of the possible model residuals later*

## No info on fitted.

Same re fitted(). No info in the vars help page either. For conditional state-space models, the meaning of "fitted" can have multiple definitions. I can make a good guess, but it'd be better to be explicit by saying.

*I'll add an example later*

## Nothing mentioned about the F_0 and P_0 assumptions

Different algorithms and authors take different approaches. Which one is used here?










