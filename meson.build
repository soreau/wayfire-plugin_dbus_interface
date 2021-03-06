project(
	'wayfire-plugin-dbus_interface',
    'c',
	'cpp',
	version: '0.3',
	license: 'MIT',
	meson_version: '>=0.51.0',
	default_options: [
		'cpp_std=c++17',
        'c_std=c17',
		'werror=false',
	],
)

gio = dependency('gio-2.0')
wayfire = dependency('wayfire', version: '>=0.6.0', required : true)
wlroots = dependency('wlroots')
wfconfig = dependency('wf-config')
glib = dependency('glib-2.0')
xcb = dependency('xcb')
xcbres = dependency('xcb-res')


add_project_arguments(['-DWLR_USE_UNSTABLE'], language: ['cpp', 'c'])
add_project_arguments(['-DWAYFIRE_PLUGIN'], language: ['cpp', 'c'])
add_project_link_arguments(['-rdynamic'], language:'cpp')



######################### Finish ###########################
schemas_dir = join_paths(get_option('prefix'), 'share/glib-2.0/schemas')
install_data('wf-prop', install_dir: join_paths(get_option('prefix'), 'bin'))
install_data('org.wayland.compositor.dbus.gschema.xml', install_dir: schemas_dir)

r = run_command('glib-compile-schemas', schemas_dir)
if r.returncode() == 0
    message('\'glib-compile-schemas ' + schemas_dir + '\' succeeded')
else
    message(r.stdout().strip())
    message(r.stderr().strip())
    message('\'glib-compile-schemas ' + schemas_dir + '\' failed')
endif

pms = shared_module('dbus_interface', 'dbus_interface.cpp',
    dependencies: [wayfire, wlroots, gio, xcb, xcbres],
    install: true, install_dir: wayfire.get_variable(pkgconfig: 'plugindir'),
    cpp_args : ['-Wno-write-strings', '-Wno-unused-parameter', '-Wno-format-security'])
	install_data('dbus_interface.xml', install_dir: wayfire.get_variable(pkgconfig: 'metadatadir'))


summary = [
	'',
	'----------------',
	'wayfire-plugins-dbus_interface @0@'.format(meson.project_version()),
	'----------------',
	''
]
message('\n'.join(summary))
