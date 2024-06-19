PORTNAME=	gr
DISTVERSION=	0.73.6
CATEGORIES=	math

MAINTAINER=	y@trombik.org
COMMENT=	GR framework: a graphics library for visualisation applications

LICENSE=	MIT
LICENSE_FILE=	${WRKSRC}/LICENSE.md

BUILD_DEPENDS=	bash:shells/bash
LIB_DEPENDS+=	libxml2.so:textproc/libxml2 \
	libtiff.so:graphics/tiff \
	libcairo.so:graphics/cairo \
	libpixman-1.so:x11/pixman

USES=	gmake pkgconfig xorg gl

USE_GITHUB=	yes
GH_TUPLE=	sciapp:gr:v${DISTVERSION}

MAKE_SHELL=	${LOCALBASE}/bin/bash
GRDIR=        ${PREFIX}/${PORTNAME}
EXTRA_CFLAGS=	${CFLAGS} -I${PREFIX}/include
EXTRA_CXXFLAGS=	${CXXFLAGS} -I${PREFIX}/include
EXTRA_LDFLAGS=	${LDFLAGS} -L${LOCALBASE}/lib
MAKE_ENV=	GRDIR="${GRDIR}" \
			EXTRA_CFLAGS="${EXTRA_CFLAGS}" \
			EXTRA_CXXFLAGS="${EXTRA_CXXFLAGS}" \
			EXTRA_LDFLAGS="${EXTRA_LDFLAGS}" \
			LOCALBASE=${LOCALBASE}

OPTIONS_DEFINE= QT5 FFMPEG
OPTIONS_SUB=	yes
QT5_DESC=	Enable QT5
VIDEO_DESC=		Enable video backends
OPTIONS_DEFAULT=	QT5 FFMPEG
USE_XORG=	x11
USE_GL=	gl

.include <bsd.port.options.mk>

.if ${PORT_OPTIONS:MQT5}
USES+=	qt:5 xorg
USE_QT=	core gui widgets qmake network buildtools:build
USE_XORG+=	xt ice sm

# XXX fixup Makefile
MAKE_ENV+=	GR_QT5=y
.endif

.if ${PORT_OPTIONS:MFFMPEG}
LIB_DEPENDS+=	libavcodec.so:multimedia/ffmpeg
USE_XORG+=	xt
.endif

pre-configure:
	@${REINPLACE_CMD} 's/%%VERSION%%/${DISTVERSION}/' ${WRKSRC}/lib/Version

post-install:
	${STRIP_CMD} ${STAGEDIR}${PREFIX}/gr/lib/*.so
	${MV} ${STAGEDIR}${PREFIX}/gr/lib/pkgconfig/*.pc ${STAGEDIR}${PREFIX}/libdata/pkgconfig/
	${RMDIR} ${STAGEDIR}${PREFIX}/gr/lib/pkgconfig
.if ${PORT_OPTIONS:MQT5}
	${STRIP_CMD} ${STAGEDIR}${PREFIX}/gr/bin/*
.for F in gksqt grplot gksm
	${INSTALL_SCRIPT} ${FILESDIR}/bin.sh ${STAGEDIR}${PREFIX}/bin/${F}
	@${REINPLACE_CMD} 's|%%PREFIX%%|${PREFIX}|' ${STAGEDIR}${PREFIX}/bin/${F}
.endfor
.endif

.include <bsd.port.mk>
