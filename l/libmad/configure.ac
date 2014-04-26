dnl -*- m4 -*-
dnl
dnl libmad - MPEG audio decoder library
dnl Copyright (C) 2000-2004 Underbit Technologies, Inc.
dnl
dnl This program is free software; you can redistribute it and/or modify
dnl it under the terms of the GNU General Public License as published by
dnl the Free Software Foundation; either version 2 of the License, or
dnl (at your option) any later version.
dnl
dnl This program is distributed in the hope that it will be useful,
dnl but WITHOUT ANY WARRANTY; without even the implied warranty of
dnl MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
dnl GNU General Public License for more details.
dnl
dnl You should have received a copy of the GNU General Public License
dnl along with this program; if not, write to the Free Software
dnl Foundation, Inc., 59 Temple Place, Suite 330, Boston, MA  02111-1307  USA
dnl

AC_PREREQ([2.59])

AC_INIT([MPEG Audio Decoder], [0.15.1b], [support@underbit.com], [libmad])

AC_CONFIG_SRCDIR([decoder.h])
AC_CONFIG_HEADER([config.h])

AM_INIT_AUTOMAKE([1.6 dist-bzip2])
m4_ifdef([AM_SILENT_RULES], [AM_SILENT_RULES([yes])])

LT_INIT([win32-dll disable-static])

AC_CANONICAL_HOST

### Additional options to configure

AC_ARG_ENABLE([speed],
   [AS_HELP_STRING([--enable-speed],
       [optimize for speed over accuracy @<:@default=disabled@:>@])],
   [
    if test "x${enableval}" = "xyes" ; then
       optimize_for="yes"
    else
       optimize_for="no"
    fi
   ],
   [optimize_for="no"])

AC_MSG_CHECKING([whether to optimize for speed or for accuracy])
if test "x${optimize_for}" = "xyes" ; then
   AC_MSG_RESULT([speed])
   AC_DEFINE([OPT_SPEED], [1], [Define to optimize for speed over accuracy.])
else
   AC_MSG_RESULT([accuracy])
   AC_DEFINE([OPT_ACCURACY], [1], [Define to optimize for accuracy over speed.])
fi

AC_ARG_ENABLE([fpm],
  [AS_HELP_STRING([--enable-fpm=ARCH],
      [use ARCH-specific fixed-point math routines
       (one of: intel, arm, mips, sparc, ppc, 64bit, float, no, yes (=auto) @<:@default=auto@:>@)])],
  [fpm=$enableval],
  [fpm=yes])

case "${fpm}" in
   yes)   FPMV=""        ;;
   no)    FPMV="DEFAULT" ;;
   intel) FPMV="INTEL"   ;;
   arm)   FPMV="ARM"     ;;
   mips)  FPMV="MIPS"    ;;
   sparc) FPMV="SPARC"   ;;
   ppc)   FPMV="PPC"     ;;
   64bit) FPMV="64BIT"   ;;
   float) FPMV="FLOAT"   ;;
   *)     AC_MSG_ERROR([bad option for fixed_point math routines])
esac

if test -z "$FPMV" && test "$GCC" = "yes" ; then
   case "$host" in
      i?86-*)     FPMV="INTEL"   ;;
      arm*-*)     FPMV="ARM"     ;;
      mips*-*)    FPMV="MIPS"    ;;
      sparc*-*)   FPMV="SPARC"   ;;
      powerpc*-*) FPMV="PPC"     ;;
      *)          FPMV="DEFAULT" ;;
   esac
fi

AC_MSG_CHECKING([for architecture-specific fixed-point math routines])
AC_MSG_RESULT([$FPMV])

if test "x$FPMV" = "xDEFAULT" ; then
   AC_MSG_WARN([default fixed-point math will yield limited accuracy])
fi

FPM="-DFPM_$FPMV"
AC_SUBST([FPM])

AC_ARG_ENABLE([sso],
   [AS_HELP_STRING([--enable-sso],
       [enable subband synthesis optimization @<:@default=disabled@:>@])],
   [
    if test "x${enableval}" = "xyes" ; then
       sso="yes"
    else
       sso="no"
    fi
   ],
   [sso="no"])

AC_MSG_CHECKING([whether to enable subband synthesis optimization])
AC_MSG_RESULT([$sso])

if test "x${sso}" = "xyes" ; then
   AC_DEFINE([OPT_SSO], [1], [Define to enable fast subband synthesis approximation optimization.])
fi

AC_ARG_ENABLE([aso],
   [AS_HELP_STRING([--disable-aso],
       [disable architecture-specific optimization @<:@default=disabled@:>@])],
   [
    if test "x${enableval}" = "xyes" ; then
       aso="yes"
    else
       aso="no"
    fi
   ],
   [aso="no"])

AC_MSG_CHECKING([whether to enable subband synthesis optimization])
AC_MSG_RESULT([$aso])

if test "x${aso}" = "xyes" ; then
   case "$host" in
      i?86-*)
         : #ASO="$ASO -DASO_INTERLEAVE1"
         ASO="$ASO -DASO_ZEROCHECK"
         : #not yet #ASO="$ASO -DASO_SYNTH"
         : #not yet #ASO_OBJS="synth_mmx.lo"
         ;;
      arm*-*)
         ASO="$ASO -DASO_INTERLEAVE1"
         ASO="$ASO -DASO_IMDCT"
         ASO_OBJS="imdct_l_arm.lo"
         ;;
      mips*-*)
         ASO="$ASO -DASO_INTERLEAVE2"
         ASO="$ASO -DASO_ZEROCHECK"
         ;;
   esac
fi

AC_SUBST([ASO])
AC_SUBST([ASO_OBJS])

AC_ARG_ENABLE([strict-iso],
   [AS_HELP_STRING([--enable-strict-iso],
       [enable strict ISO/IEC interpretations @<:@default=disabled@:>@])],
   [
    if test "x${enableval}" = "xyes" ; then
       strict_iso="yes"
    else
       strict_iso="no"
    fi
   ],
   [strict_iso="no"])

AC_MSG_CHECKING([whether to enable ISO/IEC interpretation])
AC_MSG_RESULT([$strict_iso])

if test "x${strict_iso}" = "xyes" ; then
   AC_DEFINE([OPT_STRICT], [1], [Define to enable strict interpretation of the ISO/IEC standards.])
fi

AC_ARG_ENABLE([profiling],
   [AS_HELP_STRING([--enable-profiling],
       [enable profiling @<:@default=disabled@:>@])],
   [
    if test "x${enableval}" = "xyes" ; then
       profiling="yes"
    else
       profiling="no"
    fi
   ],
   [profiling="no"])

AC_MSG_CHECKING([whether to enable profiling])
AC_MSG_RESULT([$profiling])

if test "x${profiling}" = "xyes" ; then
   AC_DEFINE([DEBUG], [1], [Define to enable diagnostic debugging support.])
else
   AC_DEFINE([NDEBUG], [1], [Define to disable diagnostic debugging support.])
fi

AM_CONDITIONAL([HAVE_PROFILING], [test "x${profiling}" = "xyes"])

AC_ARG_ENABLE([experimental],
   [AS_HELP_STRING([--enable-experimental],
       [enable experimental code @<:@default=disabled@:>@])],
   [
    if test "x${enableval}" = "xyes" ; then
       experimental="yes"
    else
       experimental="no"
    fi
   ],
   [experimental="no"])

AC_MSG_CHECKING([whether to enable experimental code])
AC_MSG_RESULT([$experimental])

if test "x${experimental}" = "xyes" ; then
   AC_DEFINE([EXPERIMENTAL], [1], [Define to enable experimental code.])
fi

### Checks for programs
AC_PROG_CC
AM_PROG_AS

### Checks for libraries

### Checks for header files
AC_HEADER_SYS_WAIT
AC_CHECK_HEADERS([assert.h limits.h unistd.h sys/types.h fcntl.h errno.h])

### Checks for types
AC_TYPE_PID_T

AC_CHECK_SIZEOF(int, 2)
AC_CHECK_SIZEOF(long, 4)
AC_CHECK_SIZEOF(long long, 8)

### Checks for structures

### Checks for compiler characteristics
AC_C_INLINE
AC_C_BIGENDIAN

### Checks for linker characteristics

### Checks for library functions
AC_CHECK_FUNCS([waitpid fcntl pipe fork])

AC_CONFIG_FILES([
Makefile
msvc++/Makefile
libmad.list
mad.pc
])

AC_OUTPUT

echo
echo
echo
echo "------------------------------------------------------------------------"
echo "$PACKAGE_NAME $PACKAGE_VERSION"
echo "------------------------------------------------------------------------"
echo
echo "Configuration Options Summary:"
echo
echo "  OS...................: ${host_os}"
echo "  Windows version......: ${_efl_windows_version}"
echo
if test "x${optimize_for}" = "xyes" ; then
echo "  optimization.........: speed"
else
echo "  Optimization.........: accuracy"
fi
echo "  Fixed-point math.....: ${FPMV}"
echo "  Subband synthesis....: ${sso}"
echo "  Strict ISO/IEC.......: ${strict_iso}"
echo "  Profiling............: ${profiling}"
echo "  Experimental code....: ${experimental}"
echo
echo "Compilation............: make (or gmake)"
echo "  CPPFLAGS.............: $CPPFLAGS"
echo "  CFLAGS...............: $CFLAGS"
echo "  CCAS.................: $CCAS"
echo "  CCASFLAGS............: $CCASFLAGS"
echo "  LDFLAGS..............: $LDFLAGS"
echo
echo "Installation...........: make install (as root if needed, with 'su' or 'sudo')"
echo "  prefix...............: $prefix"
echo
