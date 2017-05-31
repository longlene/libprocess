#                                               -*- Autoconf -*-
# Process this file with autoconf to produce a configure script.

AC_PREREQ([2.69])
AC_INIT([libprocess], [0.0.1])

AC_LANG([C++])

# Checks for programs.
AC_PROG_CXX([g++])
AC_PROG_CC([gcc])

AX_COMPILER_VERSION
AX_COMPILER_VENDOR

AC_CONFIG_MACRO_DIR([m4])


AM_INIT_AUTOMAKE([-Wall -Werror -Wno-portability foreign])

LT_PREREQ([2.2])
LT_INIT
LT_LANG([C++])
LT_OUTPUT

AX_CXX_COMPILE_STDCXX([11], [noext], [mandatory])

# Optional features
AC_ARG_ENABLE([libevent],
              AS_HELP_STRING([--enable-libevent],
                             [use libevent instead of libev]),
              [], [enable_libevent=no])

AC_ARG_ENABLE([ssl],
              AS_HELP_STRING([--enable-ssl],
                             [use ssl for libprocess communication]),
              [], [enable_ssl=no])


# Checks for libraries.

AC_CHECK_HEADERS([boost/version.hpp], [found_boost=yes])
if test "x$found_boost" != "xyes"; then
    AC_MSG_ERROR([cannot find Boost])
fi

AC_CHECK_HEADERS([glog/logging.h],
                     [AC_LINK_IFELSE(
                      [AC_LANG_PROGRAM([#include <glog/logging.h>], [])],
                      [TEST_LIBS="$TEST_LIBS -lglog"] [found_glog=yes],
                      [AC_MSG_WARN([glog is not installed.])])])

if test "x$found_glog" != "xyes"; then
    AC_MSG_ERROR([cannot find glog])
fi

AC_CHECK_HEADER([gmock/gmock.h],
                  [found_gmock=yes],
                  [])
if test "x$found_gmock" = "xyes"; then
    if test "x$check_gtest" = "xyes"; then
      AC_CHECK_LIB([gtest],
                   [_init],
                   [],
                   [AC_MSG_ERROR([gtest missing])])
    fi
else
    AC_MSG_ERROR([cannot find gmock])
fi

if test "x$enable_libevent" = "xyes"; then
  AC_CHECK_HEADERS([event2/event.h],
                   [AC_CHECK_LIB([event],
                                 [event_base_new],
                                 [],
                                 [AC_MSG_ERROR([cannot find libevent
-------------------------------------------------------------------
libevent version 2+ is required for libprocess to build.
-------------------------------------------------------------------
                                 ])])],
                   [AC_MSG_ERROR([cannot find libevent headers
-------------------------------------------------------------------
libevent version 2+ headers are required for libprocess to build.
-------------------------------------------------------------------
  ])])

  AC_CHECK_HEADERS([event2/thread.h],
                   [AC_CHECK_LIB([event_pthreads],
                                 [evthread_use_pthreads],
                                 [],
                                 [AC_MSG_ERROR([cannot find libevent_pthreads
-------------------------------------------------------------------
libevent_pthreads version 2+ is required for libprocess to build.
-------------------------------------------------------------------
                                 ])])],
                   [AC_MSG_ERROR([cannot find libevent_pthreads headers
-------------------------------------------------------------------
libevent_pthreads version 2+ headers are required for libprocess to build.
-------------------------------------------------------------------
  ])])
fi


AM_CONDITIONAL([ENABLE_LIBEVENT], [test x"$enable_libevent" = "xyes"])


AC_CHECK_HEADERS([google/protobuf/message.h], [found_protobuf=yes],
                   [found_protobuf=no])

if test "x$found_protobuf" = "xyes"; then
    saved_LIBS="$LIBS"
    LIBS="-lprotobuf $LIBS"
    AC_LANG_PUSH([C++])
    AC_LINK_IFELSE([AC_LANG_PROGRAM([#include <google/protobuf/stubs/common.h>],
                    [ google::protobuf::ShutdownProtobufLibrary(); ])],
            [ found_protobuf=yes ],
            [ found_protobuf=no ])
    AC_LANG_POP()
    LIBS="$saved_LIBS"
fi

if test "x$found_protobuf" != "xyes"; then
    AC_MSG_ERROR([cannot find protobuf])
fi

AC_CHECK_HEADERS([ev.h], [found_libev=yes])

if test "x$found_libev" = "xyes"; then
    AC_MSG_WARN([libev found must have been built with -DEV_CHILD_ENABLE=0])
else
    AC_MSG_ERROR([cannot find libev
-------------------------------------------------------------------
You may want specify the location of libev by providing a prefix
path via --with-libev=DIR, or check that the path you provided is
correct if you're already doing this.
-------------------------------------------------------------------
])
fi

if test "x$enable_ssl" = "xyes"; then
  AC_CHECK_HEADERS([openssl/ssl.h],
                   [AC_CHECK_LIB([ssl],
                                 [SSL_CTX_new],
                                 [],
                                 [AC_MSG_ERROR([cannot find libssl])])],
                   [AC_MSG_ERROR([cannot find libssl headers ])])

AC_CHECK_LIB([crypto], [RAND_poll], [], [AC_MSG_ERROR([cannot find libcrypto
])])
fi

if test "x$enable_ssl" = "xyes"; then
if test "x$enable_libevent" = "xyes"; then
    AC_CHECK_HEADERS([event2/bufferevent_ssl.h],
                     [AC_CHECK_LIB([event_openssl],
                                   [bufferevent_openssl_get_ssl],
                                   [],
                                   [AC_MSG_ERROR([cannot find libevent_openssl
-------------------------------------------------------------------
libevent_openssl version 2+ is required for an SSL-enabled build.
-------------------------------------------------------------------
                                   ])])],
                     [AC_MSG_ERROR([cannot find libevent_openssl headers
-------------------------------------------------------------------
libevent_openssl version 2+ headers are required for an SSL-enabled build.
-------------------------------------------------------------------
])])
else
    AC_MSG_ERROR([SSL is currently only supported with libevent])
fi
fi

AM_CONDITIONAL([ENABLE_SSL], [test x"$enable_ssl" = "xyes"])


AC_CHECK_HEADERS([stout/try.hpp], [found_stout=yes])

if test "x$found_stout" != "xyes"; then
    AC_MSG_ERROR([cannot find stout])
fi


AC_CONFIG_FILES([Makefile
                 include/Makefile
                 src/Makefile])
AC_OUTPUT

