AUTOMAKE_OPTIONS = subdir-objects

if !ENABLE_LIBEVENT
LIB_EV = -lev
endif

noinst_LTLIBRARIES = libprocess.la

libprocess_la_CPPFLAGS = \
 -I$(top_srcdir)/include \
 $(AM_CPPFLAGS)

libprocess_la_SOURCES = \
	authenticator_manager.hpp \
	authenticator_manager.cpp \
	authenticator.cpp \
	clock.cpp \
	config.hpp \
	decoder.hpp \
	encoder.hpp \
	event_loop.hpp \
	firewall.hpp \
	gate.hpp \
	help.cpp \
	http.cpp \
	io.cpp \
	latch.cpp \
	logging.cpp \
	metrics/metrics.cpp \
	mime.cpp \
	pid.cpp \
	poll_socket.cpp \
	poll_socket.hpp \
	profiler.cpp \
	process.cpp \
	process_reference.hpp \
	reap.cpp \
	socket.cpp \
	subprocess.cpp \
	subprocess_posix.cpp \
	time.cpp \
	timeseries.cpp

if ENABLE_SSL
libprocess_la_SOURCES += \
	libevent_ssl_socket.cpp \
	libevent_ssl_socket.hpp \
	openssl.cpp \
	openssl.hpp \
	ssl/utilities.cpp
endif

if ENABLE_LIBEVENT
libprocess_la_SOURCES += \
	libevent.hpp \
	libevent.cpp \
	libevent_poll.cpp
else
libprocess_la_SOURCES += \
	libev.hpp \
	libev.cpp \
	libev_poll.cpp
endif

libprocess_la_LIBADD = \
	$(LIB_GLOG)		\
	$(LIB_EV)

