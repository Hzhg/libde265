#                                               -*- Autoconf -*-
# Process this file with autoconf to produce a configure script.

AC_PREREQ([2.68])
AC_INIT([libde265], [0.1], [farin@struktur.de])
AC_CONFIG_SRCDIR([libde265/de265.c])
AC_CONFIG_HEADERS([config.h])

LIBDE265_CURRENT=0
LIBDE265_REVISION=0
LIBDE265_AGE=0

AC_SUBST(LIBDE265_CURRENT)
AC_SUBST(LIBDE265_REVISION)
AC_SUBST(LIBDE265_AGE)

dnl Initialize libtool
LT_INIT
AC_CONFIG_MACRO_DIR([m4])

# Checks for programs.
AC_PROG_CXX
AC_PROG_CC
AC_PROG_INSTALL
AC_PROG_LN_S
AC_PROG_LIBTOOL

# Initialize automake stuff
AM_INIT_AUTOMAKE

CFLAGS+=" -std=c99"
CXXFLAGS+=" -std=c++0x"

dnl Use -Wall if we have gcc.
changequote(,)dnl
if test "x$GCC" = "xyes"; then
  case " $CFLAGS " in
  *[\ \	]-Wall[\ \	]*) ;;
  *) CFLAGS="$CFLAGS -Wall" ;;
  esac
fi
changequote([,])dnl


# Checks for header files.
AC_CHECK_HEADERS([stdint.h stdlib.h string.h])

# Checks for typedefs, structures, and compiler characteristics.
AC_HEADER_STDBOOL
AC_TYPE_INT16_T
AC_TYPE_INT32_T
AC_TYPE_INT8_T
AC_TYPE_UINT16_T
AC_TYPE_UINT32_T
AC_TYPE_UINT64_T
AC_TYPE_UINT8_T

# Checks for library functions.
AC_FUNC_MALLOC
AC_CHECK_FUNCS([memmove memset])

AC_SEARCH_LIBS([sqrt], [m])


# --- additional logging ---

AC_ARG_ENABLE(log-error,
              [AS_HELP_STRING([--enable-log-error],
                              [turn on logging at error level (default=yes)])],
  [enable_log_error=$enableval],
  [enable_log_error=yes])
if eval "test $enable_log_error = yes"; then
  CFLAGS+=" -DDE265_LOG_ERROR"
fi

AC_ARG_ENABLE(log-info,
              [AS_HELP_STRING([--enable-log-info],
                              [turn on logging at info level (default=no)])],
  [enable_log_info=$enableval],
  [enable_log_info=no])
if eval "test $enable_log_info = yes"; then
  CFLAGS+=" -DDE265_LOG_INFO"
fi

AC_ARG_ENABLE(log-trace,
              [AS_HELP_STRING([--enable-log-trace],
                              [turn on logging at trace level (default=no)])],
  [enable_log_trace=$enableval],
  [enable_log_trace=no])
if eval "test $enable_log_trace = yes"; then
  CFLAGS+=" -DDE265_LOG_TRACE"
fi


# --- enable example programs ---

AC_ARG_ENABLE([dec265], AS_HELP_STRING([--disable-dec265], [Do not build dec265 decoder program.]))
AC_ARG_ENABLE([sherlock265], AS_HELP_STRING([--disable-sherlock265], [Do not build sherlock265 visual inspection program.]))

if eval "test x$enable_dec265 = x"      ; then enable_dec265=yes ; fi
if eval "test x$enable_sherlock265 = x" ; then enable_sherlock265=yes ; fi

if eval "test x$enable_dec265 = xyes" || eval "test x$enable_sherlock265 = xyes" ; then
  AC_CHECK_PROG([HAVE_VIDEOGFX], [libvideogfx-config], [yes])
  if eval "test x$HAVE_VIDEOGFX = xyes" ; then
    VIDEOGFX_CFLAGS=`libvideogfx-config --cflags`
    VIDEOGFX_LIBS=`libvideogfx-config --libs`
    AC_SUBST(VIDEOGFX_CFLAGS)
    AC_SUBST(VIDEOGFX_LIBS)
  else
    echo "Did not find libvideogfx (is libvideogfx-config in your PATH ?)."
    echo "... Disabling building of dec265 and sherlock265 ..."
    enable_dec265="no"
    enable_sherlock265="no"
  fi
fi

if eval "test $enable_sherlock265 = yes" ; then
  PKG_CHECK_MODULES([QT], [QtCore QtGui])
fi

AM_CONDITIONAL([ENABLE_DEC265], [test "x$enable_dec265" != "xno"])
AM_CONDITIONAL([ENABLE_SHERLOCK265], [test "x$enable_sherlock265" != "xno"])


# --- output configuration results ---

AC_CONFIG_FILES([Makefile])
AC_CONFIG_FILES([libde265/Makefile])
AC_CONFIG_FILES([dec265/Makefile])
AC_CONFIG_FILES([sherlock265/Makefile])
AC_OUTPUT


echo "---------------------------------------"
echo "Building dec265 example:" $enable_dec265
echo "Building sherlock265 example:" $enable_sherlock265
echo "---------------------------------------"
