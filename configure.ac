## 
## Copyright (c) 2011 Joseph Gaeddert
## Copyright (c) 2011 Virginia Polytechnic Institute & State University
##
## This file is part of liquid.
##
## liquid is free software: you can redistribute it and/or modify
## it under the terms of the GNU General Public License as published by
## the Free Software Foundation, either version 3 of the License, or
## (at your option) any later version.
##
## liquid is distributed in the hope that it will be useful,
## but WITHOUT ANY WARRANTY; without even the implied warranty of
## MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
## GNU General Public License for more details.
##
## You should have received a copy of the GNU General Public License
## along with liquid.  If not, see <http://www.gnu.org/licenses/>.

# 
# liquid WLAN library configure
# Process with autoconf to generate configure script
#

AC_INIT([liquid-wlan],[0.0.0],[liquid@vt.edu])
AC_CONFIG_SRCDIR([src/libliquid_wlan.c])

# permit auxiliary scripts directory (e.g. config.sub, config.guess, install-sh)
AC_CONFIG_AUX_DIR(scripts/)

# Specify 'C' language
AC_LANG(C)

AC_CONFIG_HEADER(config.h)
AH_TOP([
#ifndef __LIQUID_WLAN_CONFIG_H__
#define __LIQUID_WLAN_CONFIG_H__
])
AH_BOTTOM([
#endif // __LIQUID_WLAN_CONFIG_H__
])

# Check for necessary programs
AC_PROG_CC
AC_PROG_SED
AC_PROG_INSTALL
AC_PROG_RANLIB

# Check for necessary libraries, library functions
AC_FUNC_ERROR_AT_LINE
AC_FUNC_MALLOC
AC_FUNC_REALLOC

# AC_CHECK_LIB (library, function, [action-if-found], [action-if-not-found], [other-libraries])
AC_CHECK_LIB([c],[main], [],[AC_MSG_ERROR(Could not use standard C library)],   [])
AC_CHECK_LIB([m],[main], [],[AC_MSG_ERROR(Could not use standard math library)],[])

# AC_CHECK_FUNC(function, [action-if-found], [action-if-not-found])
AC_CHECK_FUNC([malloc],  [],[AC_MSG_ERROR(Could not use malloc())])
AC_CHECK_FUNC([realloc], [],[AC_MSG_ERROR(Could not use realloc())],)
AC_CHECK_FUNC([free],    [],[AC_MSG_ERROR(Could not use free())],)
AC_CHECK_FUNC([memset],  [],[AC_MSG_ERROR(Could not use memset())],)
AC_CHECK_FUNC([memmove], [],[AC_MSG_ERROR(Could not use memove())],)

AC_CHECK_FUNC([sinf],    [],[AC_MSG_ERROR(Could not use sinf())],)
AC_CHECK_FUNC([cosf],    [],[AC_MSG_ERROR(Could not use cosf())],)
AC_CHECK_FUNC([expf],    [],[AC_MSG_ERROR(Could not use expf())],)
AC_CHECK_FUNC([cargf],   [],[AC_MSG_ERROR(Could not use cargf())],)
AC_CHECK_FUNC([cexpf],   [],[AC_MSG_ERROR(Could not use cexpf())],)
AC_CHECK_FUNC([crealf],  [],[AC_MSG_ERROR(Could not use crealf())],)
AC_CHECK_FUNC([cimagf],  [],[AC_MSG_ERROR(Could not use cimagf())],)
AC_CHECK_FUNC([sqrtf],   [],[AC_MSG_ERROR(Could not use sqrtf())],)

# Check for necessary header files
AC_CHECK_HEADERS([stdio.h stdlib.h complex.h string.h getopt.h sys/resource.h float.h inttypes.h limits.h stdlib.h string.h unistd.h])
if test -z "$HAVE_stdio.h"
then
    AC_MSG_ERROR([Need stdio.h!])
fi

# Check for optional header files, libraries, programs
AC_CHECK_HEADERS(fftw3.h)
AC_CHECK_LIB([fftw3f], [fftwf_plan_dft_1d], [],
             [AC_MSG_WARN(fftw3 library useful but not required)],
             [])

CFLAGS="$CFLAGS -I../liquid-dsp/include"
CPPFLAGS="$CPPFLAGS -I../liquid-dsp/include"
AC_CHECK_HEADERS(liquid.h)

LDFLAGS="$LDFLAGS -L../liquid-dsp -L../kissfft/.libs -lkissfft"        
AC_CHECK_LIB([liquid], [modem_create], [],
             [AC_MSG_ERROR(Need liquid-dsp library!)],
             [])
#AC_CHECK_LIB([liquidfpm], [q32_mul], [],
#             [AC_MSG_WARN(fixed-point math library useful but not required)],
#             [])

# Checks for typedefs, structures, and compiler characteristics.
AC_C_INLINE
AC_TYPE_SIZE_T
AC_TYPE_UINT32_T
AC_TYPE_UINT8_T

# Check size of certain variables
AC_CHECK_SIZEOF(int)
AC_CHECK_SIZEOF(unsigned int)

# Check canonical system
AC_CANONICAL_TARGET
case $target_cpu in
i386|i486|i586|i686|x86|x86_64)
    MLIBS=""
    case $target_os in
    darwin*)
        ARCH_OPTION="-march=core2";;
    *)
        ARCH_OPTION="";;
    esac;;
powerpc*)
    MLIBS=""
    ARCH_OPTION="-fno-common -faltivec";;
*)
    # unknown architecture : use portable C version
    MLIBS=""
    ARCH_OPTION="";;
esac

case $target_os in
darwin*)
    SH_LIB=libliquid-wlan.dylib
    REBIND=""
    ;;
*)
    SH_LIB=libliquid-wlan.so
    REBIND=ldconfig
    ;;
esac

#
# autoconf variable substitutions
#
AC_SUBST(LIBS)                      # shared libraries (-lc, -lm, etc.)
AC_SUBST(MLIBS)                     # 

AC_SUBST(SH_LIB)                    # output shared library target
AC_SUBST(REBIND)                    # rebinding tool (e.g. ldconfig)
AC_SUBST(ARCH_OPTION)               # compiler architecture option

AC_CONFIG_FILES([makefile])
AC_OUTPUT
