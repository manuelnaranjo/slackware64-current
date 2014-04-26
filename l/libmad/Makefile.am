
MAINTAINERCLEANFILES = Makefile.in

BUILT_SOURCES = mad.h
include_HEADERS = mad.h

mad.h: config.status config.h Makefile  \
		$(srcdir)/COPYRIGHT $(srcdir)/mad.h.sed $(exported_headers)
	(sed -e '1s|.*|/*|' -e '1b' -e '$$s|.*| */|' -e '$$b'  \
		-e 's/^.*/ *&/' $(srcdir)/COPYRIGHT; echo;  \
	echo "# ifdef __cplusplus";  \
	echo 'extern "C" {';  \
	echo "# endif"; echo;  \
	if [ ".$(FPM)" != "." ]; then  \
		echo ".$(FPM)" | sed -e 's|^\.-D|# define |'; echo;  \
	fi;  \
	sed -ne 's/^# *define  *\(HAVE_.*_ASM\).*/# define \1/p'  \
		config.h; echo;  \
	sed -ne 's/^# *define  *OPT_\(SPEED\|ACCURACY\).*/# define OPT_\1/p'  \
		config.h; echo;  \
	sed -ne 's/^# *define  *\(SIZEOF_.*\)/# define \1/p'  \
		config.h; echo;  \
	for header in $(exported_headers); do  \
		echo;  \
		sed -n -f $(srcdir)/mad.h.sed $(srcdir)/$$header;  \
	done; echo;  \
	echo "# ifdef __cplusplus";  \
	echo '}';  \
	echo "# endif") >$@

lib_LTLIBRARIES = libmad.la

exported_headers = \
version.h \
fixed.h \
bit.h \
timer.h \
stream.h \
frame.h \
synth.h \
decoder.h

libmad_la_SOURCES = \
version.c \
fixed.c \
bit.c \
timer.c \
stream.c \
frame.c \
synth.c \
decoder.c \
layer12.c \
layer3.c \
huffman.c \
$(exported_headers) \
global.h \
layer12.h \
layer3.h \
huffman.h \
D.dat \
imdct_s.dat \
qc_table.dat \
rq_table.dat \
sf_table.dat
#synth_mmx.S
#imdct_l_arm.S

.c.s:
	$(COMPILE) -S $<

libmad_la_CPPFLAGS = \
@FPM@ \
@ASO@

libmad_la_LIBADD = @ASO_OBJS@

version_current = 2
version_revision = 1
version_age = 2
version_info = $(version_current):$(version_revision):$(version_age)

libmad_la_LDFLAGS = -no-undefined -version-info $(version_info)

bin_PROGRAMS = minimad

minimad_SOURCES = minimad.c
minimad_LDADD = libmad.la

pkgconfigdir = $(libdir)/pkgconfig
pkgconfig_DATA = mad.pc

EXTRA_DIST = \
mad.pc.in \
mad.sed \
msvc++/config.h \
msvc++/libmad.dsp \
msvc++/mad.h

clean-local:
	$(RM) mad.h
