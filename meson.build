project(
  'lavalauncher',
  'c',
  version: '1.6',
  license: 'GPLv3',
  default_options: [
    'c_std=c11',
    'warning_level=3',
    'werror=true',
  ]
)

cc = meson.get_compiler('c')

add_project_arguments(cc.get_supported_arguments([
  '-Wno-unused-parameter',
  '-Wpointer-arith',
  '-Wformat-security',
  '-Wformat',
]), language: 'c')

wayland_protocols = dependency('wayland-protocols')
wayland_client    = dependency('wayland-client')
cairo             = dependency('cairo')
realtime          = cc.find_library('rt')

subdir('protocol')

lavalauncher_inc = include_directories('src')

executable(
  'lavalauncher',
  files(
    'src/lavalauncher.c',
    'src/config.c',
    'src/draw.c',
    'src/buffer.c',
  ),
  dependencies: [
    wayland_client,
    wayland_protocols,
    cairo,
    wl_protocols,
    realtime,
  ],
  include_directories: lavalauncher_inc,
  install: true,
)

scdoc = dependency(
  'scdoc',
  version: '>=1.9.2',
  native: true,
)
if scdoc.found()
  scdoc_prog = find_program(scdoc.get_pkgconfig_variable('scdoc'), native: true)
  sh = find_program('sh', native: true)
  mandir = get_option('mandir')
  man_files = [
    'doc/lavalauncher.1.scd'
  ]
  foreach filename : man_files
    topic = filename.split('.')[-3].split('/')[-1]
    section = filename.split('.')[-2]
    output = '@0@.@1@'.format(topic, section)

    custom_target(
      output,
      input: filename,
      output: output,
      command: [
        sh, '-c', '@0@ < @INPUT@ > @1@'.format(scdoc_prog.path(), output)
      ],
      install: true,
      install_dir: '@0@/man@1@'.format(mandir, section)
    )
  endforeach
endif
