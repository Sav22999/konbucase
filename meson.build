project('com.github.ryonakano.konbucase',
    'vala', 'c',
    version: '3.1.0',
    meson_version: '>=0.49.0'
)

app_nickname = 'konbucase'

i18n = import('i18n')
gnome = import('gnome')

add_project_arguments(
    '-DGETTEXT_PACKAGE="@0@"'.format(meson.project_name()),
    language: 'c'
)

config_data = configuration_data()
config_data.set_quoted('LOCALEDIR', get_option('prefix') / get_option('localedir'))
config_data.set_quoted('GETTEXT_PACKAGE', meson.project_name())
config_file = configure_file(
    input: 'src' / 'Config.vala.in',
    output: '@BASENAME@',
    configuration: config_data
)

asresources = gnome.compile_resources(
    'as-resources',
    'data' / app_nickname + '.gresource.xml',
    source_dir: 'data',
    c_name: 'as'
)

gtksourceview_dep = dependency('gtksourceview-4', required: false)
if gtksourceview_dep.found() == false
    gtksourceview_dep = dependency('gtksourceview-3.0')
endif

chcase_subproject = subproject('chcase')
chcase_deps = chcase_subproject.get_variable('libchcase')

sources = files(
    'src' / 'Widgets' / 'ComboEntry.vala',
    'src' / 'Application.vala',
    'src' / 'MainWindow.vala',
    'src' / 'TextType.vala'
)

executable(
    meson.project_name(),
    asresources,
    config_file,
    sources,
    dependencies: [
        dependency('granite', version: '>=5.4.0'),
        dependency('gdk-3.0'),
        dependency('gtk+-3.0'),
        chcase_deps,
        gtksourceview_dep,
    ],
    install: true
)

subdir('data')
subdir('po')

meson.add_install_script('meson' / 'post_install.py')
