# Assorted small utilities for MC simulations with Cython.

----------

### How to install (meson + PEP517)

1. ```
    git remote add noDGodyaev https://github.com/noDGodyaev/mc_lib
    git fetch noDGodyaev
    git checkout noDGodyaev/meson_build
    ```
2. ```pip install .```

Err: Tests from `mc_lib\tests` don't pass, while package is built (in virtual env) and tests files are in catalog near source (inside or one level up).


----------
### How to build (meson)
Thx to Ralf Gommers
1. ```pip install git+https://github.com/mesonbuild/meson.git@master```
2. ```
   git remote add noDGodyaev https://github.com/noDGodyaev/mc_lib
   git fetch noDGodyaev
   git checkout noDGodyaev/meson_build
   ```
3. ```pip install -r requirements.txt```
   
   Meson uses a configure and a build stage.
   To configure it for putting the build artifacts in `build/` 
   and a local install under `installdir/` and then build:

4. ```
    meson setup build --prefix=$PWD/installdir
    meson install -C build
   ``` 
   If you want to rebuild the package for some developer needs.

   Use `setup` with flag `--wipe` to fully rebuild package or
   `--reconfigure` to update current build
7. ```export PYTHONPATH=$PWD/installdir/lib/python3.8/site-packages/```
   
7.  Pure python :```python -c "from mc_lib.tests.test_cubic_lattice import test_roundtrip; test_roundtrip"```

----



### Installation

```
  pip install git+https://github.com/ev-br/mc_lib.git
```

on Windows `msvcompiler` need to be installed 
  
### Usage 

```
>>> from mc_lib.lattice import get_neighbors    # for sc and other lattices
```

and

```
%% cython --cplus
from mc_lib cimport RealObservable          # for statistics
from mc_lib.rndm cimport RndmWrapper        # For rndm.uniform in Cython
```

See `examples/cy_ising.pyx` for a usage example.


Note that `RealObservable` uses C++, so you'll need to compile your
client Cython code to C++ not C. E.g. in a Jupyter notebook, use
```
%%cython --cplus
from mc_lib cimport RealObservable
```

Dependencies are `cython`, `numpy >= 1.19.0` (needs `np.random.Generator`)
and `pytest` for testing.

To build on Apple M1, need `numpy >= 1.21.0` (which is the first numpy version
to support this hardware).

![linux tests](https://github.com/ev-br/mc_lib/actions/workflows/python-package.yml/badge.svg)
![macOS tests](https://github.com/ev-br/mc_lib/actions/workflows/macos.yml/badge.svg)


Hat tip to Jake VanderPlas for his cython_template package. 


# Package Template for a Project Using Cython

[![build status](http://img.shields.io/travis/jakevdp/cython_template/master.svg?style=flat)](https://travis-ci.org/jakevdp/cython_template)
[![license](http://img.shields.io/badge/license-BSD-blue.svg?style=flat)](https://github.com/jakevdp/cython_template/blob/master/LICENSE)

This is a package template for a Python project using Cython. There are many
different ways to build cython extensions within a project, as seen in the
[Cython documentation](http://docs.cython.org/src/quickstart/build.html), but
the best way to integrate Cython into a distributed package is not always clear.

This template borrows utility scripts used in the [scipy](http://scipy.org)
and [scikit-learn](http://scikit-learn.org) projects, which give the template
a few nice features:

- Cython is required when the package is built, but not when the package is
  installed from a distributed source. That means that cython-generated C
  code is not included in the git repository, but *is* automatically included
  in any distribution. Further, it means that Cython is a requirement for
  package developers, but not for package users.
- The ``__check_build`` submodule includes scripts which detect whether the
  package has been properly built, and raise useful errors if this is not the
  case. This is especially important for when users try to import the package
  from the source directory without first building/compiling the cython
  sources.

Note that any "*.pyx" files added to the repository will have to be explicitly
added to the ``setup.py`` file within the same directory: in most cases you
should be able to simply copy the example [``setup.py``](https://github.com/jakevdp/cython_template/blob/master/cython_template/setup.py) within this repository.

This repository has a very permissive BSD license: please feel free to
use the contents to help set up your own cython-driven packages!
