# Created by: Steve Wills <swills@FreeBSD.org>
# $FreeBSD$

PORTNAME=	xpra
PORTVERSION=	0.15.10
PORTREVISION=	6
CATEGORIES=	x11
MASTER_SITES=	http://xpra.org/src/

MAINTAINER=	matthew@reztek.cz
COMMENT=	Xpra gives you persistent remote applications for X

LICENSE=	GPLv2

BUILD_DEPENDS=	cython:lang/cython \
		${PYTHON_SITELIBDIR}/gtk-2.0/gtk/__init__.py:x11-toolkits/py-gtk2
RUN_DEPENDS=	py${PYTHON_SUFFIX}-lz4>=0.7.0_1:archivers/py-lz4 \
		${PYTHON_SITELIBDIR}/rencode/__init__.py:converters/py-rencode \
		${PYTHON_SITELIBDIR}/dbus/__init__.py:devel/py-dbus \
		${PYTHON_SITELIBDIR}/OpenGL/__init__.py:graphics/py-PyOpenGL \
		${PYTHON_SITELIBDIR}/OpenGL_accelerate/__init__.py:graphics/py-PyOpenGL-accelerate \
		${PYTHON_PKGNAMEPREFIX}pillow>0:graphics/py-pillow \
		${PYTHON_SITELIBDIR}/netifaces.so:net/py-netifaces \
		ssh-askpass:security/openssh-askpass \
		setxkbmap:x11/setxkbmap \
		xauth:x11/xauth \
		xkbcomp:x11/xkbcomp \
		${LOCALBASE}/share/X11/xkb/rules/base:x11/xkeyboard-config \
		Xvfb:x11-servers/xorg-vfbserver \
		${PYTHON_SITELIBDIR}/gtk-2.0/gtk/__init__.py:x11-toolkits/py-gtk2 \
		${PYTHON_SITELIBDIR}/gtk-2.0/gtk/gtkgl/__init__.py:x11-toolkits/py-gtkglext

USES=		desktop-file-utils pkgconfig python shared-mime-info tar:xz
USE_GSTREAMER=	good python
USE_PYTHON=	distutils
USE_XORG=	x11 xcomposite xdamage xfixes xkbfile xrandr xtst
PLIST_SUB+=	PORTVERSION=${PORTVERSION} PYTHON_VER=${PYTHON_VER}

OPTIONS_DEFINE=		AVAHI AVCODEC CUPS OPENCL SWSCALE VPX WEBP X264 X265
OPTIONS_DEFAULT=	AVAHI AVCODEC CUPS SWSCALE VPX WEBP X264 X265
OPTIONS_SUB=		yes

AVCODEC_DESC=	Enable avcodec2 decoder
OPENCL_DESC=	Enable OpenCL CSC module
SWSCALE_DESC=	Enable swscale CSC module
VPX_DESC=	Enable VP8 and VP9 encoder
WEBP_DESC=	Enable WebP encoder
X264_DESC=	Enable X264 encoder
X265_DESC=	Enable X265 encoder

AVAHI_RUN_DEPENDS=	${PYTHON_SITELIBDIR}/avahi/__init__.py:net/py-avahi
AVCODEC_VARS=		XPRA_OPTIONS+=dec_avcodec2
CUPS_RUN_DEPENDS=	${PYTHON_SITELIBDIR}/cups.so:print/py-pycups
OPENCL_LIB_DEPENDS=	libOpenCL.so:devel/ocl-icd
OPENCL_RUN_DEPENDS=	${PYTHON_SITELIBDIR}/pyopencl/__init__.py:devel/py-pyopencl
OPENCL_VARS=		XPRA_OPTIONS+=csc_opencl
SWSCALE_VARS=		XPRA_OPTIONS+=csc_swscale
VPX_LIB_DEPENDS=	libvpx.so:multimedia/libvpx
VPX_VARS=		XPRA_OPTIONS+=vpx
WEBP_LIB_DEPENDS=	libwebp.so:graphics/webp
WEBP_VARS=		XPRA_OPTIONS+=webp
X264_LIB_DEPENDS=	libx264.so:multimedia/libx264
X264_VARS=		XPRA_OPTIONS+=enc_x264
X265_LIB_DEPENDS=	libx265.so:multimedia/x265
X265_VARS=		XPRA_OPTIONS+=enc_x265

PYDISTUTILS_PKGNAME=	xpra_all
XPRA_OPTIONS_ALL=	csc_opencl csc_swscale dec_avcodec2
XPRA_OPTIONS_ALL+=	enc_x264 enc_x265 vpx webp
XPRA_OPTIONS_ALL+=	${XPRA_OPTIONS_DISABLED}
XPRA_OPTIONS_DISABLED=	Xdummy Xdummy_wrapper
XPRA_OPTIONS_DISABLED+=	nvenc3 nvenc4 nvenc5

.include <bsd.port.options.mk>

.for opt in ${XPRA_OPTIONS_ALL}
.if empty(XPRA_OPTIONS:M${opt})
PYDISTUTILS_ARGS+=	--without-${opt}
.else
PYDISTUTILS_ARGS+=	--with-${opt}
.endif
.endfor

.if ${PORT_OPTIONS:MAVCODEC} || ${PORT_OPTIONS:MSWSCALE}
LIB_DEPENDS+=	libavcodec.so:multimedia/ffmpeg
.endif

.include <bsd.port.pre.mk>

PYDISTUTILS_BUILDARGS+=		${PYDISTUTILS_ARGS}
PYDISTUTILS_CONFIGUREARGS+=	${PYDISTUTILS_ARGS}
PYDISTUTILS_INSTALLARGS+=	${PYDISTUTILS_ARGS}

post-patch:
	${REINPLACE_CMD} -e 's|/usr/share|${LOCALBASE}/share|' \
	    ${WRKSRC}/xpra/platform/pycups_printing.py

post-install:
	${RM} ${STAGEDIR}${ETCDIR}/xpra.conf
	${INSTALL_DATA} ${WRKSRC}/build/etc/xpra/xpra.conf \
	    ${STAGEDIR}${ETCDIR}/xpra.conf.sample
	${FIND} ${STAGEDIR}${PYTHON_SITELIBDIR}/${PORTNAME} -name '*.so' \
	    -exec ${STRIP_CMD} {} \;

.include <bsd.port.post.mk>
