project('jgfiby', 'c',
  version : '0.1',
  license: 'BSD-3',
  meson_version: '>=0.64.0',
  default_options : ['warning_level=2', 'python.install_env=auto'],
)

add_languages('fortran')

# make some environment variables
proj_name = meson.project_name()
py_mod = import('python')
py = py_mod.find_installation(pure: false)
py_dep = py.dependency()

incdir_numpy = run_command(py,
  ['-c', 'import os; os.chdir(".."); import numpy; print(numpy.get_include())'],
  check : true
).stdout().strip()

incdir_f2py = run_command(py,
    ['-c', 'import os; os.chdir(".."); import numpy.f2py; print(numpy.f2py.get_include())'],
    check : true
).stdout().strip()

inc_np = include_directories(incdir_numpy, incdir_f2py)

# build the fortran wrapper
src_file = files(meson.source_root() / 'src' / 'fortran' / 'fib1.f')
fibby_source = custom_target('fibbymodule.c',
  input : src_file,  # .f so no F90 wrappers
  output : ['fibbymodule.c', 'fibby-f2pywrappers.f'],
  command : [py, '-m', 'numpy.f2py', '@INPUT@', '-m', 'fibby', '--lower']
)

# compile wrapper to shared object and install to python site-packages
py.extension_module('fibby',
  [src_file, fibby_source],
  incdir_f2py / 'fortranobject.c',
  include_directories: inc_np,
  dependencies : py_dep,
  subdir: proj_name,
  install : true
)
