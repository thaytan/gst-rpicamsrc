AM_CPPFLAGS = \
    $(GST_CFLAGS) \
    $(RPI_INCLUDEPATH)

AM_CFLAGS = \
    -Wall \
    -Wextra \
    -Werror

if BUILD_GST_WEBRTC_EXAMPLE
WEBRTC_EXAMPLE = webrtc-unidirectional-h264
else
WEBRTC_EXAMPLE =
endif

noinst_PROGRAMS = \
    test-color-balance \
    test-orientation \
    $(WEBRTC_EXAMPLE)

test_color_balance_SOURCES = test_color_balance.c

test_color_balance_LDADD = \
    $(GST_LIBS) \
    $(RPI_LIBFLAGS)

test_orientation_SOURCES = test_orientation.c

test_orientation_LDADD = \
    $(GST_LIBS) \
    $(RPI_LIBFLAGS)

webrtc_unidirectional_h264_SOURCES= webrtc-unidirectional-h264.c
webrtc_unidirectional_h264_CFLAGS= $(GST_WEBRTC_EXAMPLE_CFLAGS)
webrtc_unidirectional_h264_LDADD= $(GST_WEBRTC_EXAMPLE_LIBS) $(GST_LIBS) $(RPI_LIBFLAGS)
