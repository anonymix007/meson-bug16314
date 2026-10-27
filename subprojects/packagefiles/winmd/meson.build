project('winmd', 'cpp')

fs = import('fs')

winmd_dir = meson.current_source_dir() / 'src'

winmd_dep = declare_dependency(
    include_directories: [
        include_directories(fs.relative_to(winmd_dir, '.')),
    ],
)

meson.override_dependency('winmd', winmd_dep)
