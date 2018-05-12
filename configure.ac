dnl required version of autoconf
AC_PREREQ([2.53])

AC_INIT([gstrpicamsrc],[1.0.0])

dnl required versions of gstreamer and plugins-base
GST_REQUIRED=1.0.0
GSTPB_REQUIRED=1.0.0
GSTPB_BAD_REQUIRED=1.0.0

AC_CONFIG_SRCDIR([src/gstrpicamsrc.c])
AC_CONFIG_HEADERS([config.h])

dnl required version of automake
AM_INIT_AUTOMAKE([-Wno-portability 1.11 no-dist-gzip dist-xz tar-ustar subdir-objects]) 

dnl enable mainainer mode by default
AM_MAINTAINER_MODE([enable])

dnl Pretty Printing
m4_ifdef([AM_SILENT_RULES],[AM_SILENT_RULES([yes])],
  [AM_DEFAULT_VERBOSITY=1
   AC_SUBST(AM_DEFAULT_VERBOSITY)])

dnl check for tools (compiler etc.)
AC_PROG_CC

dnl required version of libtool
LT_PREREQ([2.2.6])
LT_INIT

dnl give error and exit if we don't have pkgconfig
AC_CHECK_PROG(HAVE_PKGCONFIG, pkg-config, [ ], [
  AC_MSG_ERROR([You need to have pkg-config installed!])
])
AC_PATH_PROG(GLIB_MKENUMS, glib-mkenums)

dnl Ensure we have basic Raspberry Pi headers and libs
dnl we need.
AC_ARG_WITH([rpi-header-dir],
    AS_HELP_STRING([--with-rpi-header-dir=PATH], [path to where RPi headers are installed (/opt/vc/include)]),,
    [with_rpi_header_dir='/opt/vc/include'])

oldCFLAGS="$CPPFLAGS"
RPIPATH="$with_rpi_header_dir"
RPI_INCLUDEPATH="-I$RPIPATH -I$RPIPATH/interface/vcos/pthreads -I$RPIPATH/interface/vmcs_host/linux"
CPPFLAGS="$CPPFLAGS $RPI_INCLUDEPATH"
AC_CHECK_HEADER([bcm_host.h], [], [AC_MSG_ERROR([Raspberry Pi files not found in $with_rpi_header_dir])])
AC_SUBST(RPI_INCLUDEPATH)
CPPFLAGS="$oldCPPFLAGS"

oldLDFLAGS="$LDFLAGS"
RPI_LIBFLAGS="-L/opt/vc/lib"
LDFLAGS="$LDFLAGS $RPI_LIBFLAGS"
AC_CHECK_LIB([bcm_host], [bcm_host_init], [], [AC_MSG_ERROR([Raspberry Pi files not found in /opt/vc])])
AC_SUBST(RPI_LIBFLAGS)
LDFLAGS="$oldLDFLAGS"


dnl gstreamer-plugins-bad-1.0 >= GSTPB_BAD_REQUIRED
PKG_CHECK_MODULES(GST, [
  gstreamer-1.0 >= $GST_REQUIRED
  gstreamer-base-1.0 >= $GST_REQUIRED
  gstreamer-video-1.0 >= $GSTPB_REQUIRED
], [
  AC_SUBST(GST_CFLAGS)
  AC_SUBST(GST_LIBS)
], [
  AC_MSG_ERROR([
      You need to install or upgrade the GStreamer development
      packages on your system. On debian-based systems these are
      libgstreamer1.0-dev and libgstreamer-plugins-base1.0-dev.
      on RPM-based systems gstreamer1.0-devel, libgstreamer1.0-devel
      or similar. The minimum version required is $GST_REQUIRED.
  ])
])

dnl WebRTC needed to build the streaming example 
PKG_CHECK_MODULES(GST_WEBRTC_EXAMPLE, [ gstreamer-sdp-1.0 gstreamer-webrtc-1.0 libsoup-2.4 >= 2.50 json-glib-1.0 ], HAVE_GST_WEBRTC=yes, HAVE_GST_WEBRTC=no)
AM_CONDITIONAL(BUILD_GST_WEBRTC_EXAMPLE, test "x$HAVE_GST_WEBRTC" = "xyes")

dnl check if compiler understands -Wall (if yes, add -Wall to GST_CFLAGS)
AC_MSG_CHECKING([to see if compiler understands -Wall])
save_CFLAGS="$CFLAGS"
CFLAGS="$CFLAGS -Wall"
AC_COMPILE_IFELSE([AC_LANG_PROGRAM([ ], [ ])], [
  GST_CFLAGS="$GST_CFLAGS -Wall"
  AC_MSG_RESULT([yes])
], [
  AC_MSG_RESULT([no])
])

dnl set the plugindir where plugins should be installed (for src/Makefile.am)
if test "x${prefix}" = "x$HOME"; then
  plugindir="$HOME/.gstreamer-1.0/plugins"
else
  plugindir="`pkg-config gstreamer-1.0 --variable pluginsdir`"
fi
AC_SUBST(plugindir)

dnl set proper LDFLAGS for plugins
GST_PLUGIN_LDFLAGS='-module -avoid-version -export-symbols-regex [_]*\(gst_\|Gst\|GST_\).*'
AC_SUBST(GST_PLUGIN_LDFLAGS)

AC_CONFIG_FILES([Makefile src/Makefile examples/Makefile])
AC_OUTPUT

