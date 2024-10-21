A demonstration of building a python extension from fortran code using meson and f2py

As all tools used here are in constant development, care must be taken when looking up the documentation.

It install package

>>> pip install .

To test

>>> python -c "import numpy as np; from jgfiby import fibby; a = np.zeros(9); fibby.fib(a); print (a); print(fibby.__file__)"

-------------------------
Previous build steps for non pip use

meson setup builddir
meson compile -C builddir
meson install -C builddir

NB: This is without pip package support
