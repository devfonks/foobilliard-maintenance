dnl Process this file with autoconf to produce a configure script.
AC_INIT([foobillardplus],[3.43beta])
AC_CONFIG_SRCDIR([src/billard3d.c])

AC_CONFIG_HEADERS([src/config.h])

# Detect the canonical host and target build environment
AC_CANONICAL_HOST
AC_CANONICAL_TARGET

AM_INIT_AUTOMAKE

dnl Checks for programs.
AC_PROG_CC
AC_PROG_CXX
AC_PROG_INSTALL

dnl Checks for libraries.

PKG_PROG_PKG_CONFIG
AC_CHECK_LIB(png,png_read_image)
AC_CHECK_LIB(SDL,SDL_Init)
AC_CHECK_LIB(SDL,SDL_LockAudio)
AC_CHECK_LIB(SDL_net,SDLNet_Init)
AC_CHECK_LIB(SDL_mixer,Mix_OpenAudio)
PKG_PROG_PKG_CONFIG(1.8.0)
PKG_CHECK_MODULES(FT2,freetype2)
AC_SUBST(FT2_CFLAGS)
AC_SUBST(FT2_LIBS)

### TODO ### win32
AC_ARG_ENABLE([win],
              [AS_HELP_STRING([--enable-win],[enable the compiling of the windows mingw/msys port[no]])],
              [enable_win=$enableval],
              [enable_win=no])
if test x"$enable_win" = "xyes"; then
  AC_CHECK_LIB(gl,main)
  AC_CHECK_LIB(glu,main)
  AC_CHECK_LIB(opengl32,main)
  AC_CHECK_LIB(glu32,main)
  AC_CHECK_LIB(wsock32,main)
  export USE_WIN=yes
else
  AC_MSG_RESULT([no mingw/msys Windows support])
fi
# end win32

AC_CHECK_LIB(m,pow)

dnl Checks for header files.
m4_warn([obsolete],
[The preprocessor macro `STDC_HEADERS' is obsolete.
  Except in unusual embedded environments, you can safely include all
  ISO C90 headers unconditionally.])dnl
# Autoupdate added the next two lines to ensure that your configure
# script's behavior did not change.  They are probably safe to remove.
AC_CHECK_INCLUDES_DEFAULT
AC_PROG_EGREP

AC_CHECK_HEADERS(limits.h unistd.h math.h stdlib.h stdio.h string.h endian.h GL/glu.h GL/gl.h getopt.h png.h ctype.h locale.h)

dnl Checks for typedefs, structures, and compiler characteristics.
AC_C_CONST
AC_C_INLINE

AC_PATH_XTRA

dnl Checks for library functions.
AC_CHECK_FUNCS(socket poll)

### Enable Debian/Linux default locations for compiling 
AC_ARG_ENABLE([debian],
              [AS_HELP_STRING([--enable-debian],[automatic guess for Debian/Linux Filesystem [no]])],
              [enable_debian=$enableval],
              [enable_debian=no])

### set the variables for compiling
if test x"$enable_debian" = "xno"; then
 AC_PREFIX_DEFAULT(/usr/games/foobillardplus)

 if test x"$prefix" = "xNONE"; then
   prefix="/usr/games/foobillardplus"
 else
   prefix=$prefix/foobillardplus
 fi
 if test x"$exec_prefix" = "xNONE"; then
   exec_prefix=$prefix
 fi
 bindir=$exec_prefix/bin
 datarootdir=$prefix/data
 datadir=$prefix/data
 localedir=$prefix/data/locale
 docdir=$prefix/data/locale
 PREFIX=$prefix
 AC_SUBST([PREFIX])
 EXECPREFIX=$exec_prefix
 AC_SUBST([EXECPREFIX])
 BINDIR=$bindir
 AC_SUBST([BINDIR])
 DATAROOTDIR=$datarootdir
 AC_SUBST([DATAROOTDIR])
 DATADIR=$datadir
 AC_SUBST([DATADIR])
 LOCALEDIR=$localedir
 AC_SUBST([LOCALEDIR])
 DOCDIR=$docdir
 AC_SUBST([DOCDIR])
else
 ### set directories for Debian/Linux defaults
 AC_PREFIX_DEFAULT(/usr/games)
 if test x"$prefix" = "xNONE"; then
   prefix="/usr/games"
 fi
 if test x"$exec_prefix" = "xNONE"; then
   exec_prefix=$prefix
 fi
 bindir=$exec_prefix
 datarootdir=/usr/share/games/foobillardplus
 datadir=$datarootdir/data
 localedir=$datadir/locale
 docdir=$datadir/locale
 PREFIX=$prefix
 AC_SUBST([PREFIX])
 EXECPREFIX=$exec_prefix
 AC_SUBST([EXECPREFIX])
 BINDIR=$bindir
 AC_SUBST([BINDIR])
 DATAROOTDIR=$datarootdir
 AC_SUBST([DATAROOTDIR])
 DATADIR=$datadir
 AC_SUBST([DATADIR])
 LOCALEDIR=$localedir
 AC_SUBST([LOCALEDIR])
 DOCDIR=$docdir
 AC_SUBST([DOCDIR])
 DATA_DIRECTORY=\'\"$datadir\"\'
 AC_SUBST([DATA_DIRECTORY])
 export USE_DEBIAN=yes
fi

### does the user want to have automatic guess of suitable optimization options
AC_ARG_ENABLE([optimization],
              [AS_HELP_STRING([--enable-optimization],[automatic guess of suitable optimization options [no]])],
              [enable_optimization=$enableval],
              [enable_optimization=no])

## see if the user wants aggressive optimizations of the code
AC_MSG_CHECKING([whether to enable aggressive optimization flags])
OPTIMIZATION_CFLAGS=""
OPTIMIZATION_CFLAGS=""
if test x"$enable_optimization" = "xyes"; then
  AC_MSG_RESULT([yes])
  
  # cure annoying default arguments for CFLAGS and CXXFLAGS
  # that autoconf automatically sets in GNU environments if
  # no CFLAGS or CXXFLAGS were manually given
  #
  # this is a more or less a hack ...
  if test x"$CFLAGS" = x"-g -O2" ; then
    CFLAGS=""
  fi
  if test x"$CXXFLAGS" = x"-g -O2" ; then
    CXXFLAGS=""
  fi
  if test x"$CFLAGS" = x"-g -O3" ; then
    CFLAGS=""
  fi
  if test x"$CXXFLAGS" = x"-g -O3" ; then
    CXXFLAGS=""
  fi
  
  if test x"$ac_cv_c_compiler_gnu" = "xyes"; then
    
    # special flag for gcc to work around excess precision bug
    #FLOAT_STORE_CFLAGS="-ffloat-store"

    # Due to a misconception, until SVN revision r1713, the case statement
    # used the value of '--target', not of '--host'
    case $host in
    i386-apple-*)
      # all intel macs have at least prescott instruction set
      OPTIMIZATION_CFLAGS="-fast -g"
      ;;
    x86_64-apple-*)
      # all 64bit intel macs have at least nocona instruction set
      OPTIMIZATION_CFLAGS="-fast -g -march=nocona"
      ;;
    i?86-*-*)
      # okay, we use this default together with the specification of the target architecture
      # let us just assume, a Pentium 4 would be the minimum platform for OpenCV,
      # then SSE2 instruction set is available also
      OPTIMIZATION_CFLAGS="-O3 -g -march=pentium4 -ffast-math -fomit-frame-pointer"
      ;;
    amd64-*-* | x86_64-*-*)
      OPTIMIZATION_CFLAGS="-O3 -g -march=x86-64 -ffast-math -fomit-frame-pointer"
      ;;
    powerpc-apple-*)
      # Mac OS X 10.4 runs on CPUs >= G4 only
      OPTIMIZATION_CFLAGS="-O3 -falign-loops-max-skip=15 -falign-jumps-max-skip=15 -falign-loops=16 -falign-jumps=16 -falign-functions=16 -ffast-math -fstrict-aliasing -funroll-loops -ftree-loop-linear -ftree-loop-memset -mcpu=G4 -mpowerpc-gpopt -mtune=G5 -fsched-interblock -fgcse-sm -g"
      ;;
    powerpc64-apple-*)
      # There are no 64bit Macs with a CPU that's not a G5
      OPTIMIZATION_CFLAGS="-O3 -falign-loops-max-skip=15 -falign-jumps-max-skip=15 -falign-loops=16 -falign-jumps=16 -falign-functions=16 -ffast-math -fstrict-aliasing -funroll-loops -ftree-loop-linear -ftree-loop-memset -mcpu=G5 -mpowerpc-gpopt -fsched-interblock -fgcse-sm -m64 -g"
      ;;
    powerpc-*-*)
      # generic PowerPCs don't have Apple's 'fast' or 'fastf' switch
      OPTIMIZATION_CFLAGS="-O3 -g -mcpu=G3 -mtune=G4 -fomit-frame-pointer"
      ;;
    *-*-*)
      OPTIMIZATION_CFLAGS="-O2 -fomit-frame-pointer"
      ;;
    esac
  fi
else
  AC_MSG_RESULT([no])
fi
AC_SUBST(OPTIMIZATION_CFLAGS)
AC_SUBST(OPTIMIZATION_CFLAGS)

### does the user want to have standard optimization options (default = no optimization)
AC_ARG_ENABLE([standard],
              [AS_HELP_STRING([--enable-standard],[standard optimization options [no]])],
              [enable_standard=$enableval],
              [enable_standard=no])

## see if the user wants standard optimizations of the code
AC_MSG_CHECKING([whether to enable standard optimization flags])
if test x"$enable_standard" = "xno"; then
  AC_MSG_RESULT([no])
  # cure annoying default arguments for CFLAGS and CXXFLAGS
  # that autoconf automatically sets in GNU environments if
  # no CFLAGS or CXXFLAGS were manually given
  #
  # this is a more or less a hack ...
  CFLAGS=""
  CXXFLAGS=""
else
  AC_MSG_RESULT([yes])
fi

### does the user want to have special optimization options (default = no)
AC_ARG_ENABLE([special],
              [AS_HELP_STRING([--enable-special],[special optimization options [no]])],
              [enable_special=$enableval],
              [enable_special=no])

## see if the user wants special optimizations of the code (hope this is stable optimization)
AC_MSG_CHECKING([whether to enable special optimization flags])
if test x"$enable_special" = "xno"; then
  AC_MSG_RESULT([no])
else
  AC_MSG_RESULT([yes])
  CFLAGS="\
-falign-functions \
-falign-jumps \
-falign-labels \
-falign-loops \
-ffast-math \
-Wextra \
"
  CXXFLAGS=""
fi

### want single precision math, default = yes no = double precision
AC_ARG_ENABLE(mathsingle,
              [  --enable-mathsingle[=ARG]  compile math single precision [default=yes] ],
              [ if   test "x$enableval" = "xyes" ; then
                   export USE_MATHSINGLE=yes
                elif  test "x$enableval" = "xon" ; then
                   export USE_MATHSINGLE=yes
                elif  test "x$enableval" = "xoff" ; then
                   export USE_MATHSINGLE=no
                elif  test "x$enableval" = "xno" ; then
                   export USE_MATHSINGLE=no
                else
                   export USE_MATHSINGLE=no
                fi
              ],
              [export USE_MATHSINGLE=yes])

### want network compiling code, default = yes 
AC_ARG_ENABLE(network,
              [  --enable-network[=ARG]  compile for IP-network game support [default=yes] ],
              [ if   test "x$enableval" = "xyes" ; then
                   export USE_NETWORK=yes
                elif  test "x$enableval" = "xon" ; then
                   export USE_NETWORK=yes
                elif  test "x$enableval" = "xoff" ; then
                   export USE_NETWORK=no
                elif  test "x$enableval" = "xno" ; then
                   export USE_NETWORK=no
                else
                   export USE_NETWORK=no
                fi
              ],
              [export USE_NETWORK=yes])

### SSE intrinsics compiling code, sets also math precision, default = no
AC_ARG_ENABLE(sse,
              [  --enable-sse          compile for SSE intrinsics],
              [export USE_SSE=yes; export USE_MATHSINGLE=yes], [export USE_SSE=no])

### fast math compiling code, default = no
AC_ARG_ENABLE(fastmath,
              [  --enable-fastmath          compile fastmath routines in, default yes],
              [ if   test "x$enableval" = "xyes" ; then
                   export FAST_MATH=yes
                elif 	test "x$enableval" = "xon" ; then
                   export FAST_MATH=yes
                elif 	test "x$enableval" = "xoff" ; then
                   export FAST_MATH=no
                elif 	test "x$enableval" = "xno" ; then
                   export FAST_MATH=no
                else
                   export FAST_MATH=yes
                fi
              ],
              [export FAST_MATH=yes])

### generic touch wanted. Also used with WeTab, default = no
AC_ARG_ENABLE(touch,
              [  --enable-touch          compile for generic touch-devices],
              [export USE_TOUCH=yes], [export USE_TOUCH=no])

### WeTab compiling code, sets also TOUCH, default = no
AC_ARG_ENABLE(wetab,
              [  --enable-wetab          compile for german tablet WeTab],
              [export USE_WETAB=yes; export USE_TOUCH=yes], [export USE_WETAB=no])
if test x"$enable_wetab" = "xyes"; then
  CFLAGS="\
-falign-functions \
-falign-jumps \
-falign-labels \
-falign-loops \
-march=atom \
-mssse3 \
-mtune=atom \
-mfpmath=sse \
"
  CXXFLAGS=""
fi

### nvidia specific, don't use, default = no
AC_ARG_ENABLE(nvidia,
              [  --enable-nvidia[=ARG]   enable NV-extensions [default=no] ],
              [ if   test "x$enableval" = "xyes" ; then
                   export USE_NVIDIA=yes
                elif 	test "x$enableval" = "xon" ; then
                   export USE_NVIDIA=yes
                elif 	test "x$enableval" = "xoff" ; then
                   export USE_NVIDIA=no
                elif 	test "x$enableval" = "xno" ; then
                   export USE_NVIDIA=no
                else
                   export USE_NVIDIA=no
                fi
              ],
              [export USE_NVIDIA=no])

### want compiling code with sound enabled, default = yes
AC_ARG_ENABLE(sound,
              [  --enable-sound[=ARG]    enable sound [default=yes] ],
              [ if   test "x$enableval" = "xyes" ; then
                   export USE_SOUND=yes
                elif 	test "x$enableval" = "xon" ; then
                   export USE_SOUND=yes
                elif 	test "x$enableval" = "xoff" ; then
                   export USE_SOUND=no
                elif 	test "x$enableval" = "xno" ; then
                   export USE_SOUND=no
                else
                   export USE_SOUND=yes
                fi
              ],
              [export USE_SOUND=yes])

AM_CONDITIONAL(USE_MATHSINGLE, test "x$USE_MATHSINGLE" = "xyes")

AM_CONDITIONAL(USE_NETWORK, test "x$USE_NETWORK" = "xyes")

AM_CONDITIONAL(USE_TOUCH, test "x$USE_TOUCH" = "xyes")

AM_CONDITIONAL(USE_WETAB, test "x$USE_WETAB" = "xyes")

AM_CONDITIONAL(USE_NVIDIA, test "x$USE_NVIDIA" = "xyes")

AM_CONDITIONAL(USE_SOUND, test "x$USE_SOUND" = "xyes")

AM_CONDITIONAL(USE_WIN, test "x$USE_WIN" = "xyes")

AM_CONDITIONAL(USE_SSE, test "x$USE_SSE" = "xyes")

AM_CONDITIONAL(FAST_MATH, test "x$FAST_MATH" = "xyes")

AM_CONDITIONAL(USE_DEBIAN, test "x$USE_DEBIAN" = "xyes")

AC_CONFIG_FILES([Makefile foobillardplus.spec src/Makefile])
AC_OUTPUT

AC_MSG_RESULT([
General configuration ================================================

    CC:                       ${CC}
    CFLAGS:                   ${CFLAGS}
    LDFLAGS:                  ${LDFLAGS}

    Install path:             ${prefix}
    Binary dir:               ${bindir}
    Data rootdir:             ${datarootdir}
    Data dir:                 ${datadir}
    Locale dir:               ${localedir}
    Doc dir:                  ${docdir}
    Debian/Linux:             ${DATA_DIRECTORY}

    Debug flags:              ${DEBUG_CFLAGS}
    Optimization flags:       ${OPTIMIZATION_CFLAGS}
    Mathsingle precision:     ${USE_MATHSINGLE}
    Sound compiling:          ${USE_SOUND}
    Generic Touchdevice:      ${USE_TOUCH}
    WeTab Tablet compiling:   ${USE_WETAB}
    Network compiling:        ${USE_NETWORK}
    use sse:                  ${USE_SSE}
    fast math routines:       ${FAST_MATH}

Now run make ...
])
