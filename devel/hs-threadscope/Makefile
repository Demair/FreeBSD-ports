# $FreeBSD$

PORTNAME=	threadscope
PORTVERSION=	0.2.7
CATEGORIES=	devel haskell

MAINTAINER=	haskell@FreeBSD.org
COMMENT=	Graphical tool for profiling parallel Haskell programs

LICENSE=	BSD3CLAUSE

USE_CABAL=	cairo ghc-events>=0.4.2 glib gtk>=0.12 gtk2hs-buildtools mtl \
		pango text

EXECUTABLE=	threadscope

STANDALONE=	yes

.include "${.CURDIR}/../../lang/ghc/bsd.cabal.mk"
.include <bsd.port.mk>
