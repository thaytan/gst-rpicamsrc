project('gst-rpicamsrc', 'c', version : '1.0.0',
  meson_version : '>= 0.34.0', default_options : ['b_asneeded=false'])

gst_req = '>= 1.0.0'

gst_dep = dependency('gstreamer-1.0', version : gst_req, required : false)
gstbase_dep = dependency('gstreamer-base-1.0', version : gst_req, required : false)
gstvideo_dep = dependency('gstreamer-video-1.0', version : gst_req, required : false)

if not gst_dep.found() or not gstbase_dep.found() or not gstvideo_dep.found()
  error('''
        You need to install or upgrade the GStreamer development
        packages on your system. On debian-based systems these are
        libgstreamer1.0-dev and libgstreamer-plugins-base1.0-dev.
        on RPM-based systems gstreamer1.0-devel, libgstreamer1.0-devel
        or similar. The minimum version required is ''' + gst_req)
endif

plugins_install_dir = '@0@/gstreamer-1.0'.format(get_option('libdir'))

cc = meson.get_compiler('c')

rpi_path = get_option('with-rpi-header-dir')
rpi_paths = ['-I' + rpi_path, '-I' + rpi_path + '/interface/vcos/pthreas', '-I' + rpi_path + '/interface/vmcs_host/linux']

if not cc.has_header('bcm_host.h', args : rpi_paths)
  error('Could not find bcm_host.h. Please pass the location of this header via -Dwith-rpi-header-dir=/path')
endif

rpi_lib_path = get_option('with-rpi-lib-dir')
mmal_deps = []
foreach rpi_lib : ['mmal_core', 'mmal_util', 'mmal_vc_client', 'vcos', 'bcm_host']
  l = cc.find_library(rpi_lib, dirs : rpi_lib_path, required : false)
  if not l.found()
    error('''
          Could not find lib@0@ in standard library paths and @1@.
	  Please pass the location of these libs via -Dwith-rpi-lib-dir=/path.
	  '''.format(rpi_lib, rpi_lib_path))
  endif
  mmal_deps += [l]
endforeach

config = configuration_data()
config.set('PACKAGE', '"gst-rpicamsrc"')
config.set('VERSION', '"@0@"'.format(meson.project_version()))
config.set('PACKAGE_VERSION', '"@0@"'.format(meson.project_version()))
config.set('GST_PACKAGE_NAME', '"GStreamer RaspberryPi Camera Source"')
config.set('GST_PACKAGE_ORIGIN', '"Unknown package origin"')
config.set('GST_LICENSE', '"LGPL"')

gst_rpicamsrc_args = ['-DHAVE_CONFIG_H'] + rpi_paths

if get_option('default_library') == 'static'
  gst_rpicamsrc_args += ['-DGST_STATIC_COMPILATION']
endif

config_inc = include_directories('.')

subdir('src')
#subdir('examples') # FIXME

configure_file(output : 'config.h', configuration : config)
