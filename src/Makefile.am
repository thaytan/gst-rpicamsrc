plugin_LTLIBRARIES = libgstrpicamsrc.la

libgstrpicamsrc_la_SOURCES = gstrpicamsrc.c gstrpicamsrc.h \
   RaspiCapture.c RaspiCamControl.c RaspiPreview.c \
   RaspiCapture.h RaspiCamControl.h RaspiPreview.h

# Need -DGST_USE_UNSTABLE_API for GstBaseCameraSrc
libgstrpicamsrc_la_CFLAGS = $(GST_CFLAGS) $(RPI_INCLUDEPATH) -I$(top_srcdir) -DGST_USE_UNSTABLE_API
libgstrpicamsrc_la_LIBADD = $(GST_LIBS) -lgstbasecamerabinsrc-1.0 $(RPI_LIBFLAGS) -lmmal_core -lmmal_util -lmmal_vc_client -lvcos -lbcm_host
libgstrpicamsrc_la_LDFLAGS = $(GST_PLUGIN_LDFLAGS)
libgstrpicamsrc_la_LIBTOOLFLAGS = --tag=disable-static

noinst_HEADERS = gstrpicamsrc.h RaspiCapture.h RaspiCamControl.h RaspiPreview.h
