#
#

OPENBSMDIR=${SRCTOP}/contrib/openbsm
.PATH: ${OPENBSMDIR}/bin/audit

CFLAGS+=	-I${OPENBSMDIR}

PACKAGE=audit
PROG=	audit
MAN=	audit.8

LIBADD=	bsm System notify

WARNS?=	5

.include <bsd.prog.mk>
