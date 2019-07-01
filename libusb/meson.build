install_headers(['libusb.h'])

sources = [
  'core.c',
  'descriptor.c',
  'hotplug.c',
  'io.c',
  'strerror.c',
  'sync.c'
]

c_args = []
link_args = []

if host_machine.system() == 'linux'
  sources += [
    'os/poll_posix.c',
    'os/threads_posix.c',
    'os/linux_usbfs.c',
   ]

  c_args += ['-pthread']
  link_args += ['-pthread']

  if cc.has_header('libudev.h')
    sources += ['os/linux_udev.c']
  else
    sources += ['os/linux_netlink.c']
    c_args += ['-D_GNU_SOURCE']
  endif
endif

libusb = library('usb-1.0',
  sources,
  c_args : c_args,
  link_args : link_args,
  include_directories: [config_h_inc],
  install: true)
