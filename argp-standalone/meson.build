project('argp-standalone', 'c')

argp_source = files([
	'argp-ba.c',
	'argp-eexst.c',
	'argp-fmtstream.c',
	'argp-help.c',
	'argp-parse.c',
	'argp-pv.c',
	'argp-pvh.c',
	# 'argp-test.c'
])

install_headers('argp.h')

conf_data = configuration_data()

cc = meson.get_compiler('c')

conf_data.set10('HAVE_LIMITS_H', cc.check_header('limits.h'))
conf_data.set10('HAVE_MALLOC_H', cc.check_header('malloc.h'))
conf_data.set10('HAVE_UNISTD_H', cc.check_header('unistd.h'))
conf_data.set10('HAVE_CONFIG_H', true)
conf_data.set10('HAVE_LIBINTL_H', cc.check_header('libintl.h'))
conf_data.set10('HAVE_ALLOCA_H', cc.check_header('alloca.h'))
# conf_data.set10('_LIBC', true)

conf_data.set10('HAVE_FLOCKFILE', cc.has_function('flockfile'))
conf_data.set10('HAVE_FPUTS_UNLOCKED', cc.has_function('fputs_unlocked', prefix : '#include <stdio.h>'))
conf_data.set10('HAVE_FWRITE_UNLOCKED', cc.has_function('fwrite_unlocked', prefix : '#include <stdio.h>'))
conf_data.set10('HAVE_STRDUP', cc.has_function('strdup'))
conf_data.set10('HAVE_ASPRINTF', cc.has_function('asprintf'))
conf_data.set10('HAVE_STRCHRNUL', cc.has_function('strchrnul'))
 
conf_data.set10('HAVE_DECL_PROGRAM_INVOCATION_NAME',
			    cc.has_function('program_invocation_name', prefix : '#include <errno.h>'))
conf_data.set10('HAVE_DECL_PROGRAM_INVOCATION_SHORT_NAME',
				cc.has_function('program_invocation_short_name', prefix : '#include <errno.h>'))

deps = []
if cc.check_header('libintl.h')
	deps += dependency('intl')
endif

configure_file(input : 'config.h.in',
               output : 'config.h',
               configuration : conf_data)

add_global_arguments('-DHAVE_CONFIG_H=1', language : 'c')

if not cc.has_function('strchrnul')
	argp_source += files(['strchrnul.c'])
endif
if not cc.has_function('strndup')
	argp_source += files(['strndup.c'])
endif
if not cc.has_function('strcasecmp')
	argp_source += files(['strcasecmp.c'])
endif
if not cc.has_function('mempcpy')
	argp_source += files(['mempcpy.c'])
endif

library('argp',
	argp_source,
	include_directories: '.',
	dependencies: deps,
	install : true
)
