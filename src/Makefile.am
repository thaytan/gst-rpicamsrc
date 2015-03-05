plugin_LTLIBRARIES = libgstrpicamsrc.la

ENUM_FILES = gstrpicam_types.h
BUILT_SOURCES = gstrpicam-enum-types.h gstrpicam-enum-types.c

libgstrpicamsrc_la_SOURCES = \
   $(ENUM_FILES) $(BUILT_SOURCES) \
   gstrpicamsrc.c gstrpicamsrc.h \
   gstrpicamsrcdeviceprovider.c gstrpicamsrcdeviceprovider.h \
   RaspiCapture.c RaspiCamControl.c RaspiPreview.c \
   RaspiCLI.c

# Need -DGST_USE_UNSTABLE_API for GstBaseCameraSrc
libgstrpicamsrc_la_CFLAGS = $(GST_CFLAGS) $(RPI_INCLUDEPATH) -I$(top_srcdir)
libgstrpicamsrc_la_LIBADD = $(GST_LIBS) $(RPI_LIBFLAGS) -lmmal_core -lmmal_util -lmmal_vc_client -lvcos -lbcm_host
libgstrpicamsrc_la_LDFLAGS = $(GST_PLUGIN_LDFLAGS)
libgstrpicamsrc_la_LIBTOOLFLAGS = --tag=disable-static

gstrpicam-enum-types.h: gstrpicam-enums-template.h $(ENUM_FILES) $(GLIB_MKENUMS)
	$(AM_V_GEN) (cd $(srcdir) && $(GLIB_MKENUMS) --template gstrpicam-enums-template.h $(ENUM_FILES)) > $@

gstrpicam-enum-types.c: gstrpicam-enum-types.h gstrpicam-enums-template.c $(ENUM_FILES) $(GLIB_MKENUMS)
	$(AM_V_GEN) (cd $(srcdir) && $(GLIB_MKENUMS) --template gstrpicam-enums-template.c $(ENUM_FILES)) > $@

noinst_HEADERS = gstrpicamsrc.h RaspiCapture.h RaspiCamControl.h \
    RaspiPreview.h RaspiCLI.h \
    gstrpicam-enum-types.h $(ENUM_FILES)
CLEANFILES = $(BUILT_SOURCES)
