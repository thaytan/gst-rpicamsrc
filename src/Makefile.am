plugin_LTLIBRARIES = libgstrpicamsrc.la

libgstrpicamsrc_la_SOURCES = gstrpicamsrc.c gstrpicamsrc.h

# Need -DGST_USE_UNSTABLE_API for GstBaseCameraSrc
libgstrpicamsrc_la_CFLAGS = $(GST_CFLAGS) $(RPI_INCLUDEPATH) -I$(top_srcdir) -DGST_USE_UNSTABLE_API
libgstrpicamsrc_la_LIBADD = $(GST_LIBS) -lgstbasecamerabinsrc-1.0
libgstrpicamsrc_la_LDFLAGS = $(GST_PLUGIN_LDFLAGS)
libgstrpicamsrc_la_LIBTOOLFLAGS = --tag=disable-static

noinst_HEADERS = gstrpicamsrc.h
