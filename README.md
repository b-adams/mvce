# Conda-build MVCE

I am trying to build a package to upload to [my Anaconda channel](https://anaconda.org/adambr/repo). Following along with ["Packaging Python Basics with Continuum Analytics Conda."](http://kylepurdon.com/blog/packaging-python-basics-with-continuum-analytics-conda.html) I was successful in building and uploading [a small example](https://anaconda.org/adambr/mvce). The working code is at [this commit](https://github.com/b-adams/mvce/commit/690f5320d746b12fddd239b85d82c0add5427766).

However, I ran into a problem when adding [cx_Oracle](https://oracle.github.io/python-cx_Oracle/) as a dependency. I usually install it via pip, but I understand it needs to be available as a conda package for use as a dependency of another conda package. So, mirroring the "Converting an Existing Package/Upload Invoke to Your Channel" steps, I successfully built [and uploaded](https://anaconda.org/adambr/cx_oracle) using `conda skeleton pypi cx_Oracle` and `conda build cx_Oracle`.

After adding my channel to my .condarc (`conda config --add channels adambr`), I was able to create a new environment with cx_Oracle (`conda create -n test_my_upload python=3.6 cx_Oracle`) and it pulled from my channel and let me create a database connection as desired.

However, when I added cx_Oracle as a dependency in my test project, I ended up with the following error ([full log here](https://github.com/b-adams/mvce/blob/master/mvce/build.log)):

> Can't build C:\Users\adambr\Projects\Python\mvce due to environment creation error:
> Package missing in current win-64 channels:
>   - cx_Oracle

It's unclear to me why the package would be reported missing, or how to fix it. I've made sure that the `adambr` channel is in my .condarc file (per [this 2014 post](http://stackoverflow.com/questions/25610843/conda-build-requirement-add-package-from-specific-channel#25632265)), I invoked conda-build using `conda build -c adambr .` to further ensure the channel was specified, and it's picked up just fine for my system by `conda install`.
