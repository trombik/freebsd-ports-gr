PORTNAME=	gr
DISTVERSION=	0.73.6
CATEGORIES=	math

MAINTAINER=	y@trombik.org
COMMENT=	GR framework: a graphics library for visualisation applications

LICENSE=	MIT
LICENSE_FILE=	${WRKSRC}/LICENSE.md

BUILD_DEPENDS=	bash:shells/bash
LIB_DEPENDS+=	libxml2.so:textproc/libxml2 \
	libpng16.so:graphics/png \
	libqhull_r.so:math/qhull

USES=	gmake cmake:noninja jpeg pkgconfig

USE_GITHUB=	yes
GH_TUPLE=	sciapp:gr:v${DISTVERSION}

MAKE_SHELL=	${LOCALBASE}/bin/bash
CMAKE_INSTALL_PREFIX=        ${PREFIX}/${PORTNAME}

OPTIONS_DEFINE= QT5
OPTIONS_SUB=	yes
QT5_DESC=	Enable QT5
OPTIONS_DEFAULT=	QT5

.include <bsd.port.options.mk>

.if ${PORT_OPTIONS:MQT5}
USES+=	qt:5 xorg
USE_QT=	core gui widgets qmake network buildtools:build
USE_XORG=	x11 xt ice sm
LIB_DEPENDS+=	libGLX.so:graphics/libglvnd
.endif

pre-configure:
	@${REINPLACE_CMD} 's/%%VERSION%%/${DISTVERSION}/' ${WRKSRC}/lib/Version

pre-install:
	${MKDIR} ${STAGEDIR}/${CMAKE_INSTALL_PREFIX}

post-install:
	${STRIP_CMD} ${STAGEDIR}${PREFIX}/gr/lib/*.so
	${MV} ${STAGEDIR}${PREFIX}/gr/lib/pkgconfig/*.pc ${STAGEDIR}${PREFIX}/libdata/pkgconfig/
	${RMDIR} ${STAGEDIR}${PREFIX}/gr/lib/pkgconfig
.if ${PORT_OPTIONS:MQT5}
.for F in gksqt grplot
	${INSTALL_SCRIPT} ${FILESDIR}/bin.sh ${STAGEDIR}${PREFIX}/bin/${F}
	@${REINPLACE_CMD} 's|%%PREFIX%%|${PREFIX}|' ${STAGEDIR}${PREFIX}/bin/${F}
.endfor
.endif

.include <bsd.port.mk>
