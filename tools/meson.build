common_inc = include_directories('../common')
tools_link_args = ['-static', '-static-libstdc++', '-static-libgcc']

if not meson.get_compiler('cpp').links('int main() { return 0; }',
  args: tools_link_args,
  name: 'fully static C++ linking',
)
  error('The Linux command-line tools require static C/C++ runtime libraries. ' +
        'On Fedora, install glibc-static and libstdc++-static, then rerun Meson setup.')
endif

executable('runner_probe',
  '../common/runner_discovery.cpp',
  'runner_probe.cpp',
  include_directories: common_inc,
  link_args: tools_link_args,
  install: false,
)

executable('dlssnr-shmctl',
  'shmctl.cpp',
  include_directories: common_inc,
  link_args: tools_link_args,
  install: false,
)
