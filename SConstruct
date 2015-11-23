import os

# assume check was installed into /usr/local/
env_with_err = Environment(
	ENV = os.environ,
	CPPPATH = ['#/src', '/usr/local/include'])
if "CC" in os.environ:
	env_with_err["CC"] = os.environ["CC"]
if "CCFLAGS" not in os.environ:
	env_with_err["CCFLAGS"] = '-g -std=c99 -D_GNU_SOURCE -Wall -Werror -O3'
#print "CCCOM is:", env_with_err.subst('$CCCOM')

prefix = '/usr/local'
install_libdir = os.path.join(prefix, 'lib')
install_incdir = os.path.join(prefix, 'include')

# shared
shared_lib = env_with_err.SharedLibrary('src/libart.so', 'src/art.c')
env_with_err.Install(install_libdir, shared_lib)

# static
static_lib = env_with_err.StaticLibrary('src/libart.a', 'src/art.c')
env_with_err.Install(install_libdir, static_lib)

# build
build = env_with_err.Alias('build', ['src/libart.a', 'src/libart.so'])

# header
env_with_err.Install(install_incdir, 'src/art.h')

# install
env_with_err.Alias('install', [install_libdir, install_incdir])

# test
env2 = env_with_err.Clone()
env2.ParseConfig('pkg-config --cflags --libs check')
test_runner = env2.Program('test_runner', ['tests/runner.c', 'src/libart.a'])
test_alias = Alias('test', [test_runner], os.path.join('.', test_runner[0].path))
AlwaysBuild(test_alias)

Default(build)
