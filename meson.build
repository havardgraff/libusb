project('libusb', 'c',
  version : '1.0.0',
  meson_version : '>= 0.45.0',
  default_options : [
    'warning_level=1',
    'c_std=gnu99',
  ])

add_project_arguments('-O2', language: 'c')

cc = meson.get_compiler('c')
m_dep = cc.find_library('m', required : true)

pkgconf = configuration_data()
pkgconf.set('prefix', join_paths(get_option('prefix')))
pkgconf.set('exec_prefix', '${prefix}')
pkgconf.set('libdir', '${prefix}/@0@'.format(get_option('libdir')))
pkgconf.set('includedir', '${prefix}/@0@'.format(get_option('includedir')))
pkgconf.set('VERSION', meson.project_version())
pkgconf.set('LIBS', '')

pkg_install_dir = join_paths(get_option('libdir'), 'pkgconfig')
configure_file(
  input: 'libusb-1.0.pc.in',
  output: 'libusb-1.0.pc',
  configuration: pkgconf,
  install_dir: pkg_install_dir)

### Configuration ###
config_h_inc = include_directories('.')
libusb_conf = configuration_data()

headers = [
  'sys/time.h',
  'asm/types.h',
  'sys/types.h',
  'syslog.h',
  'strings.h',
  'poll.h',
  'missing.h',
]

foreach h : headers
  if cc.has_header(h)
    define = 'HAVE_' + h.underscorify().to_upper()
    libusb_conf.set10(define, 1)
  endif
endforeach

# The check for some of these has been simplified because it 'works
# for me'.
functions = [
]

foreach f : functions
  if cc.has_function(f)
    define = 'HAVE_' + f.underscorify().to_upper()
    libusb_conf.set10(define, 1)
  endif
endforeach

libusb_conf.set('POLL_NFDS_TYPE', 'nfds_t')
libusb_conf.set('OS', host_machine.system().to_upper())
libusb_conf.set10('THREADS_POSIX', 1)

configure_file(input : 'config.meson',
  output : 'config.h',
  configuration : libusb_conf)

subdir('libusb')