project('meson-bug16XXX')

windows_rs_subproject = subproject('windows-rs')
winmd_dep = subproject('winmd', required: true)
winmd_dir = winmd_dep.get_variable('winmd_dir')

cmake = import('cmake')
cppwinrt_opts = cmake.subproject_options()
cppwinrt_opts.add_cmake_defines({'EXTERNAL_WINMD_INCLUDE_DIR': winmd_dir})
cppwinrt_opts.append_compile_args('cpp', f'-I@winmd_dir@')
cppwinrt_subproject = cmake.subproject('cppwinrt', native: true, options: cppwinrt_opts, required: true)
cppwinrt_exe = cppwinrt_subproject.target('cppwinrt')

metadata_dir = windows_rs_subproject.get_variable('metadata_dir')

cppwinrt_headers_dir = meson.current_build_dir() / 'cppwinrt-headers'

cppwinrt_headers = custom_target('cppwinrt-headers',
  output        : 'cppwinrt-headers.stamp',
  command       : [
    find_program('sh'),
    '-c',
    '"$1" -input "$2" -output "$3" && touch "$4"',
    'sh',
    cppwinrt_exe,
    metadata_dir,
    cppwinrt_headers_dir,
    '@OUTPUT@',
  ],
  console: true,
  build_by_default: true,
  install: false,
)

cppwinrt_dep = declare_dependency(
  sources: cppwinrt_headers,
  compile_args: ['-I' + cppwinrt_headers_dir],
)
meson.override_dependency('cppwinrt-headers', cppwinrt_dep)
