## Process this file with automake to produce Makefile.in

SUBDIRS = data

MODULE_SO = libudisks2_lsm.so

NULL =

INCLUDES = \
	-I$(top_builddir) -I$(top_srcdir) \
	-DPACKAGE_LIBEXEC_DIR=\""$(libexecdir)"\" \
	-DPACKAGE_SYSCONF_DIR=\""$(sysconfdir)"\" \
	-DPACKAGE_DATA_DIR=\""$(datadir)"\" \
	-DPACKAGE_BIN_DIR=\""$(bindir)"\" \
	-DPACKAGE_LOCALSTATE_DIR=\""$(localstatedir)"\" \
	-DPACKAGE_LOCALE_DIR=\""$(localedir)"\" \
	-DPACKAGE_LIB_DIR=\""$(libdir)"\" \
	-D_POSIX_PTHREAD_SEMANTICS -D_REENTRANT \
	-DUDISKS_COMPILATION \
	-DBUILD_DIR=\"$(abs_top_builddir)/\" \
	$(POLKIT_GOBJECT_1_CFLAGS) \
	$(GLIB_CFLAGS) \
	$(GIO_CFLAGS) \
	$(GUDEV_CFLAGS) \
	$(WARN_CFLAGS) \
	$(NULL)

$(dbus_built_sources): data/org.freedesktop.UDisks2.lsm.xml Makefile.am
	$(AM_V_GEN) gdbus-codegen \
		--interface-prefix org.freedesktop.UDisks2. \
		--c-namespace UDisks \
		--generate-c-code lsm_generated \
		--generate-docbook doc_generated \
		$<
	$(NULL)


module_flags = \
	-export_dynamic \
	-avoid-version \
	-module \
	-no-undefined \
	-export-symbols-regex \
	'^udisks_module_.*|g_io_module_load|g_io_module_unload|g_io_module_query'

dbus_built_sources = \
	lsm_generated.h    lsm_generated.c \
	$(NULL)

BUILT_SOURCES = \
	$(dbus_built_sources) \
	$(NULL)

libudisks2_lsmdir=$(libdir)/udisks2/modules
libudisks2_lsm_LTLIBRARIES=libudisks2_lsm.la

libudisks2_lsmincludedir=$(includedir)/udisks2/udisks

libudisks2_lsminclude_HEADERS= \
	$(top_srcdir)/modules/udisksmoduleiface.h \
	$(top_srcdir)/modules/udisksmoduleifacetypes.h \
	$(top_srcdir)/modules/udisksmoduleobject.h \
	$(NULL)

libudisks2_lsm_la_SOURCES = \
	$(BUILT_SOURCES) \
	$(top_srcdir)/modules/udisksmoduleiface.h \
	$(top_srcdir)/modules/udisksmoduleifacetypes.h \
	$(top_srcdir)/modules/udisksmoduleobject.h \
	$(top_srcdir)/modules/udisksmoduleobject.c \
	lsm_types.h \
	lsm_module_iface.c \
	lsm_data.h lsm_data.c \
	lsm_linux_drive.c \
	lsm_linux_manager.c \
	$(NULL)

libudisks2_lsm_la_CPPFLAGS = \
	-DG_LOG_DOMAIN=\"libudisks2-lsm\" \
	$(NULL)

libudisks2_lsm_la_CFLAGS = \
	$(GLIB_CFLAGS) \
	$(GIO_CFLAGS) \
	$(GUDEV_CFLAGS) \
	$(POLKIT_GOBJECT_1_CFLAGS) \
	$(LIBLSM_CFLAGS) \
	$(LIBCONFIG_CFLAGS) \
	$(NULL)

libudisks2_lsm_la_LDFLAGS = $(module_flags)

libudisks2_lsm_la_LIBADD = \
	$(GLIB_LIBS) \
	$(GIO_LIBS) \
	$(GUDEV_LIBS) \
	$(POLKIT_GOBJECT_1_LIBS) \
	$(LIBLSM_LIBS) \
	$(LIBCONFIG_LIBS) \
	$(NULL)

# ------------------------------------------------------------------------------

CLEANFILES = doc_generated-*.xml lsm_generated.[ch]

EXTRA_DIST = \
	$(NULL)

include ../Makefile.uninstalled

all-local: module_link

dist-hook:
	(for i in $(BUILT_SOURCES); do rm -f $(distdir)/$$i; done)

clean-local: module_unlink
	rm -f *~ \
		$(BUILT_SOURCES)
