AC_INIT(libtirpc, 0.3.2)
AM_INIT_AUTOMAKE([silent-rules])
AM_SILENT_RULES([yes])
AC_CONFIG_SRCDIR([src/auth_des.c])
AC_CONFIG_MACRO_DIR([m4])
AC_PROG_CC

AC_CHECK_HEADER([gssapi/gssapi.h], [HAVE_GSSAPI_H=yes], [HAVE_GSSAPI_H=no])

AC_ARG_ENABLE(gssapi,
	[AC_HELP_STRING([--disable-gssapi], [Disable GSSAPI support @<:@default=no@:>@])],
      [],[enable_gssapi=yes])
AM_CONDITIONAL(GSS, test "x$enable_gssapi" = xyes)

if test "x$enable_gssapi" = xyes; then
	if test "x$HAVE_GSSAPI_H" = xno; then
		AC_MSG_ERROR([gssapi.h not found. Use --disable-gssapi, or install GSS-API.])
	fi
	AC_CHECK_TOOL([KRB5_CONFIG], [krb5-config], [no])
	if test "x$KRB5_CONFIG" = xno; then
		AC_MSG_ERROR([krb5-config tool not found. Use --disable-gssapi, or install Kerberos.])
	fi
	GSSAPI_CFLAGS=`${KRB5_CONFIG} --cflags gssapi`
	GSSAPI_LIBS=`${KRB5_CONFIG} --libs gssapi`
	AC_SUBST([GSSAPI_CFLAGS])
	AC_SUBST([GSSAPI_LIBS])
fi

AC_ARG_ENABLE(ipv6,
	[AC_HELP_STRING([--disable-ipv6], [Disable IPv6 support @<:@default=no@:>@])],
	[],[enable_ipv6=yes])
AM_CONDITIONAL(INET6, test "x$disable_ipv6" != xno)
if test "x$enable_ipv6" != xno; then
	AC_DEFINE(INET6, 1, [Define to 1 if IPv6 is available])
fi

AC_ARG_ENABLE(symvers,
	[AC_HELP_STRING([--disable-symvers], [Disable symbol versioning @<:@default=no@:>@])],
      [],[enable_symvers=yes])
AM_CONDITIONAL(SYMVERS, test "x$enable_symvers" = xyes)

AC_CONFIG_HEADERS([config.h])
AC_PROG_LIBTOOL
AC_HEADER_DIRENT
AC_PREFIX_DEFAULT(/usr)
AC_CHECK_HEADERS([arpa/inet.h fcntl.h libintl.h limits.h locale.h netdb.h netinet/in.h stddef.h stdint.h stdlib.h string.h sys/ioctl.h sys/param.h sys/socket.h sys/time.h syslog.h unistd.h features.h gssapi/gssapi_ext.h])
AC_CHECK_LIB([pthread], [pthread_create])
AC_CHECK_FUNCS([getrpcbyname getrpcbynumber setrpcent endrpcent getrpcent])

AC_CONFIG_FILES([Makefile src/Makefile man/Makefile doc/Makefile])
AC_OUTPUT(libtirpc.pc)


