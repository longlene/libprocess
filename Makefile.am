AM_CXXFLAGS = -Wall

AM_CXXFLAGS += -Wsign-compare

AM_CXXFLAGS += -Wformat-security

ACLOCAL_AMFLAGS = -I m4

AUTOMAKE_OPTIONS = foreign


SUBDIRS = include src


if !ENABLE_LIBEVENT
LIB_EV = -lev
endif

