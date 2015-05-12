AM_CPPFLAGS = \
    $(GST_CFLAGS) \
    $(RPI_INCLUDEPATH)

AM_CFLAGS = \
    -Wall \
    -Wextra \
    -Werror

noinst_PROGRAMS = \
    test-color-balance \
    test-orientation

test_color_balance_SOURCES = test_color_balance.c

test_color_balance_LDADD = \
    $(GST_LIBS) \
    $(RPI_LIBFLAGS)

test_orientation_SOURCES = test_orientation.c

test_orientation_LDADD = \
    $(GST_LIBS) \
    $(RPI_LIBFLAGS)

