MK_AUTO_OBJ=    yes
APP=	        WindowServer
.PATH:          ${.CURDIR}
SRCS=           WindowServer.m \
                BSDFramebuffer.m \
		WSInput.m \
		WSAppRecord.m \
		WSWindowRecord.m \
		WSDisplay.m \
		main.m
RESOURCES=	${.CURDIR}/Icon.png \
		${MAKEOBJDIR}/libinput/libinput.so.1 \
		${MAKEOBJDIR}/libinput/libinput.so \
		${MAKEOBJDIR}/libevdev/libevdev.so.2 \
		${MAKEOBJDIR}/libevdev/libevdev.so \
		${.CURDIR}/Cursors/arrowCursor.png \
		${.CURDIR}/Cursors/resizeCursor.png \
		${.CURDIR}/Decorations/close.png \
		${.CURDIR}/Decorations/closeHover.png \
		${.CURDIR}/Decorations/mini.png \
		${.CURDIR}/Decorations/miniHover.png \
		${.CURDIR}/Decorations/zoom.png \
		${.CURDIR}/Decorations/zoomUpHover.png \
		${.CURDIR}/Decorations/zoomDownHover.png

MK_WERROR=	no
WARNS=	        1
SYSROOT=        --sysroot=${OBJTOP}/tmp -B${OBJTOP}/tmp/usr/bin
CFLAGS+=	-fobjc-arc -g -D__RAVYNOS__ -D__MACH__ -D__WSRPC -pthread \
		-I${.CURDIR} -I${MAKEOBJDIR} -I${.CURDIR}/libinput/src \
		-I${.CURDIR}/libinput/include -I${SRCTOP}/Frameworks \
		-I${.CURDIR}/libxkbcommon/include -I${SRCTOP}/CoreServices \
		-I${OBJTOP}/Frameworks/Foundation/Headers -I${OBJTOP}/tmp/usr/include \
		-I${SRCTOP}/gnu/lib/libobjc2 -I${SRCTOP}/sys/sys -I${SRCTOP}/sys \
		-I${SRCTOP}/Frameworks/AppKit/Headers

LDFLAGS+=	-Wl,-R'$$ORIGIN/../Resources' -L${MAKEOBJDIR} \
		-L${OBJTOP}/tmp/lib -L${OBJTOP}/tmp/usr/lib \
		-L${OBJTOP}/Frameworks/Foundation/Foundation.framework \
		-L${OBJTOP}/Frameworks/CoreGraphics/CoreGraphics.framework \
		-L${OBJTOP}/Frameworks/CoreFoundation/CoreFoundation.framework \
		-L${OBJTOP}/Frameworks/Onyx2D/Onyx2D.framework \
		-L${OBJTOP}/Frameworks/AppKit/AppKit.framework \
		-L${OBJTOP}/Frameworks/CoreText/CoreText.framework \
                -Wl,-R/System/Library/Frameworks/OpenGL.framework/Versions/A \
                -Wl,-R/System/Library/Frameworks/Foundation.framework/Versions/A \
                -Wl,-R/System/Library/Frameworks/CoreGraphics.framework/Versions/A \
                -Wl,-R/System/Library/Frameworks/CoreFoundation.framework/Versions/A \
                -Wl,-R/System/Library/Frameworks/Onyx2D.framework/Versions/A \
                -Wl,-R/System/Library/Frameworks/AppKit.framework/Versions/A \
                -Wl,-R/System/Library/Frameworks/CoreText.framework/Versions/A \
		${MAKEOBJDIR}/libxkbcommon/src/*.pico \
		${MAKEOBJDIR}/libxkbcommon/src/compose/*.pico \
		${MAKEOBJDIR}/libxkbcommon/src/xkbcomp/*.pico \
		${OBJTOP}/tmp/lib/libutil.so.9 -L${MAKEOBJDIR}/libinput -linput \
		-ludev -lCoreText -lAppKit -lFoundation -lCoreGraphics -lcrypt -llaunch \
		-lCoreFoundation -lOnyx2D -lxml2 -pthread -lobjc -lSystem -lkvm -lm \
		${OBJTOP}/tmp/lib/libc.so.7
#.export SYSROOT

SUBDIR+=        libevdev .WAIT
SUBDIR+=	mtdev .WAIT
SUBDIR+=        libinput .WAIT
SUBDIR+=	libxkbcommon

${MAKEOBJDIR}/Resources:
	cp -fv ${.CURDIR}/libinput/quirks/* ${APP_DIR}/Contents/Resources/
	${MAKE} -C ${.CURDIR}/libevdev tools
	${MAKE} -C ${.CURDIR}/libevdev tools-install APP_DIR=${APP_DIR}
	${MAKE} -C ${.CURDIR}/libinput tools
	${MAKE} -C ${.CURDIR}/libinput tools-install APP_DIR=${APP_DIR}

make-obj-dirs: .EXEC
	mkdir -p ${MAKEOBJDIR}
.for subdir in ${SUBDIR}
	mkdir -p ${MAKEOBJDIR}/${subdir}
.endfor
	mkdir -p ${MAKEOBJDIR}/LoadingWindow
	mkdir -p ${MAKEOBJDIR}/LoginWindow
	mkdir -p ${MAKEOBJDIR}/SystemUIServer
	mkdir -p ${MAKEOBJDIR}/libxkbcommon/src/xkbcomp
	mkdir -p ${MAKEOBJDIR}/libxkbcommon/src/compose

clean: .PHONY
	rm -rf ${MAKEOBJDIR}

.include <sys.mk>
.include <bsd.subdir.mk>
.include <rvn.app.mk>

${OBJS}: ${MAKEOBJDIR}/Resources

client: client.m message.h
	mkdir -p ${.CURDIR}/client.app/Contents/ravynOS
	cd ${.CURDIR}; ${CC} -g -fobjc-arc -fobjc-runtime=gnustep-2.0 -fobjc-nonfragile-abi \
		-fPIC -o ${.CURDIR}/client.app/Contents/ravynOS/client client.m \
		-framework Foundation -framework CoreFoundation \
		-framework CoreGraphics -framework AppKit -lSystem

