AC_INIT([Haskell base package], [1.0], [libraries@haskell.org], [base])

# Safety check: Ensure that we are in the correct source directory.
AC_CONFIG_SRCDIR([include/HsBase.h])

AC_CONFIG_HEADERS([include/HsBaseConfig.h])

# do we have long longs?
AC_CHECK_TYPES([long long])

dnl ** Working vfork?
AC_FUNC_FORK

dnl ** determine whether or not const works
AC_C_CONST

dnl ** check for full ANSI header (.h) files
AC_HEADER_STDC

# check for specific header (.h) files that we are interested in
AC_CHECK_HEADERS([ctype.h dirent.h errno.h fcntl.h limits.h signal.h sys/resource.h sys/select.h sys/syscall.h sys/time.h sys/timeb.h sys/timers.h sys/times.h sys/utsname.h sys/wait.h termios.h time.h utime.h windows.h winsock.h])

# Enable large file support. Do this before testing the types ino_t, off_t, and
# rlim_t, because it will affect the result of that test.
AC_SYS_LARGEFILE

dnl ** check for wide-char classifications
dnl FreeBSD has an emtpy wctype.h, so test one of the affected
dnl functions if it's really there.
AC_CHECK_HEADERS([wctype.h], [AC_CHECK_FUNCS(iswspace)])

AC_CHECK_FUNCS([ftime gmtime_r localtime_r lstat readdir_r])
AC_CHECK_FUNCS([getclock getrusage gettimeofday setitimer times])
AC_CHECK_FUNCS([_chsize ftruncate])

dnl ** check if it is safe to include both <time.h> and <sys/time.h>
AC_HEADER_TIME

dnl ** how do we get a timezone name, and UTC offset ?
AC_STRUCT_TIMEZONE

dnl ** do we have altzone?
FP_DECL_ALTZONE

# map standard C types and ISO types to Haskell types
FPTOOLS_CHECK_HTYPE(char)
FPTOOLS_CHECK_HTYPE(signed char)
FPTOOLS_CHECK_HTYPE(unsigned char)
FPTOOLS_CHECK_HTYPE(short)
FPTOOLS_CHECK_HTYPE(unsigned short)
FPTOOLS_CHECK_HTYPE(int)
FPTOOLS_CHECK_HTYPE(unsigned int)
FPTOOLS_CHECK_HTYPE(long)
FPTOOLS_CHECK_HTYPE(unsigned long)
if test "$ac_cv_type_long_long" = yes; then
FPTOOLS_CHECK_HTYPE(long long)
FPTOOLS_CHECK_HTYPE(unsigned long long)
fi
FPTOOLS_CHECK_HTYPE(float)
FPTOOLS_CHECK_HTYPE(double)
FPTOOLS_CHECK_HTYPE(ptrdiff_t)
FPTOOLS_CHECK_HTYPE(size_t)
FPTOOLS_CHECK_HTYPE(wchar_t)
# Int32 is a HACK for non-ISO C compilers
FPTOOLS_CHECK_HTYPE(sig_atomic_t, Int32)
FPTOOLS_CHECK_HTYPE(clock_t)
FPTOOLS_CHECK_HTYPE(time_t)
FPTOOLS_CHECK_HTYPE(dev_t, Word32)
FPTOOLS_CHECK_HTYPE(ino_t)
FPTOOLS_CHECK_HTYPE(mode_t)
FPTOOLS_CHECK_HTYPE(off_t)
FPTOOLS_CHECK_HTYPE(pid_t)
FPTOOLS_CHECK_HTYPE(gid_t)
FPTOOLS_CHECK_HTYPE(uid_t)
FPTOOLS_CHECK_HTYPE(cc_t)
FPTOOLS_CHECK_HTYPE(speed_t)
FPTOOLS_CHECK_HTYPE(tcflag_t)
FPTOOLS_CHECK_HTYPE(nlink_t)
FPTOOLS_CHECK_HTYPE(ssize_t)
FPTOOLS_CHECK_HTYPE(rlim_t)
FPTOOLS_CHECK_HTYPE(wint_t)

FPTOOLS_CHECK_HTYPE(intptr_t)
FPTOOLS_CHECK_HTYPE(uintptr_t)
# Workaround for OSes that don't have intmax_t and uintmax_t, e.g. OpenBSD.
if test "$ac_cv_type_long_long" = yes; then
  fptools_cv_default_htype_intmax=$fptools_cv_htype_long_long
  fptools_cv_default_htype_uintmax=$fptools_cv_htype_unsigned_long_long
else
  fptools_cv_default_htype_intmax=$fptools_cv_htype_long
  fptools_cv_default_htype_uintmax=$fptools_cv_htype_unsigned_long
fi
FPTOOLS_CHECK_HTYPE(intmax_t, $fptools_cv_default_htype_intmax)
FPTOOLS_CHECK_HTYPE(uintmax_t, $fptools_cv_default_htype_uintmax)

# test errno values
FP_CHECK_CONSTS([E2BIG EACCES EADDRINUSE EADDRNOTAVAIL EADV EAFNOSUPPORT EAGAIN EALREADY EBADF EBADMSG EBADRPC EBUSY ECHILD ECOMM ECONNABORTED ECONNREFUSED ECONNRESET EDEADLK EDESTADDRREQ EDIRTY EDOM EDQUOT EEXIST EFAULT EFBIG EFTYPE EHOSTDOWN EHOSTUNREACH EIDRM EILSEQ EINPROGRESS EINTR EINVAL EIO EISCONN EISDIR ELOOP EMFILE EMLINK EMSGSIZE EMULTIHOP ENAMETOOLONG ENETDOWN ENETRESET ENETUNREACH ENFILE ENOBUFS ENODATA ENODEV ENOENT ENOEXEC ENOLCK ENOLINK ENOMEM ENOMSG ENONET ENOPROTOOPT ENOSPC ENOSR ENOSTR ENOSYS ENOTBLK ENOTCONN ENOTDIR ENOTEMPTY ENOTSOCK ENOTTY ENXIO EOPNOTSUPP EPERM EPFNOSUPPORT EPIPE EPROCLIM EPROCUNAVAIL EPROGMISMATCH EPROGUNAVAIL EPROTO EPROTONOSUPPORT EPROTOTYPE ERANGE EREMCHG EREMOTE EROFS ERPCMISMATCH ERREMOTE ESHUTDOWN ESOCKTNOSUPPORT ESPIPE ESRCH ESRMNT ESTALE ETIME ETIMEDOUT ETOOMANYREFS ETXTBSY EUSERS EWOULDBLOCK EXDEV ENOCIGAR], [#include <stdio.h>
#include <errno.h>])

FP_CHECK_CONSTS([SIGABRT SIGALRM SIGBUS SIGCHLD SIGCONT SIGFPE SIGHUP SIGILL SIGINT SIGKILL SIGPIPE SIGQUIT SIGSEGV SIGSTOP SIGTERM SIGTSTP SIGTTIN SIGTTOU SIGUSR1 SIGUSR2 SIGPOLL SIGPROF SIGSYS SIGTRAP SIGURG SIGVTALRM SIGXCPU SIGXFSZ SIG_BLOCK SIG_SETMASK SIG_UNBLOCK SIG_DFL SIG_IGN SIG_ERR], [
#if HAVE_SIGNAL_H
#include <signal.h>
#endif])

dnl ** can we open files in binary mode?
FP_CHECK_CONST([O_BINARY], [#include <fcntl.h>], [0])

# Check for idiosyncracies in some mingw impls of directory handling.
FP_READDIR_EOF_ERRNO

AC_OUTPUT
