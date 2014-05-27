
INCLUDE_DIRECTORIES( ${CMAKE_CURRENT_SOURCE_DIR}/.. )

# ******
# You should point XFIGLIBDIR to the same directory you did when you compiled
# and installed xfig.

#set( XFIGLIBDIR "/usr/local/lib/X11/xfig" )

# ****************
# Change RGB if necessary, to point to your rgb.txt color database
# file (for X11).  This is needed for the -g (background color) option.
# Note that this changed slightly from the previous versions - you need
# the full name (rgb.txt) now.
#
#TODO path as a parameter
SET (RGB \"/opt/X11/share/X11/rgb.txt\")

ADD_DEFINITIONS( -DUSE_INLINE )


option (USEPNG "Use PNG" OFF)
option (USEXPM "Use XPM" ON)

IF (USEXPM)
  SET( XPMLIBDIR  /opt/X11/lib/ )
  SET( XPMINC /opt/X11/include/ )
ENDIf (USEXPM)

# ****************
# Comment out the DDNFSS = ... line if you don't want NFSS2 font selection
#	for LaTeX texts
# Comment out the DDLATEX2E_GRAPHICS = ... line if you don't want to use the
#	standard graphics package distributed w/ LaTeX2e
# UNcomment the DDEPSFIG = ... line if you want to use the deprecated epsfig macros
#	distributed w/ LaTeX2e (DDLATEX2E_GRAPHICS must also be defined)
#
#     Hint: Users of LaTeX2e will probably want to include both DDNFSS
#		and DDLATEX2E_GRAPHICS options; users of LaTeX 2.09 will
#		probably want to include neither.

ADD_DEFINITIONS( -DNFSS)
ADD_DEFINITIONS( -DLATEX2E_GRAPHICS )
#add_definitions( -DEPSFIG )

# ****************
# Comment out the DDDVIPS to NOT use dvips for the DVI-to-PostScript driver
ADD_DEFINITIONS( -DDVIPS )



# **************************************************************
# *** You shouldn't have to change anything below this point ***
# **************************************************************

#DIR_DEFS=	-DBITMAPDIR=\\\"$(DESTDIR)$(XFIGLIBDIR)/bitmaps\\\"


#ADD_definitions( -DBITMAPDIR ${XFIGLIBDIR} )

IF (USEXPM)
  ADD_DEFINITIONS( -DUSE_XPM )
  #Add_definitions( -L${XPMLIBDIR} -lXpm -lX11 )

  FIND_LIBRARY(XPMLIBS Xpm)
  MESSAGE( "found Xpm ${XPMLIBS}" )
  #find_library(X11LIBS X11)
  #message( "found X11 ${X11LIBS}" )
ENDIF (USEXPM)


#fig2dev.h:218:13: error: redefinition of 'sys_nerr' with a different type
#extern int              sys_nerr, errno;
#TODO check for architecture

IF( APPLE )
  ADD_DEFINITIONS( -D__DARWIN__ )
ENDIF( APPLE )


ADD_SUBDIRECTORY( dev )


INCLUDE_DIRECTORIES( ${XPMINC} )      # one header is #inclue X11/fo.h
INCLUDE_DIRECTORIES( ${XPMINC}/X11 )  # however other #include xpm.h


MESSAGE("Replacing RGB path in colors.c")
# replacement string
SET( REPLACEMENT_TEXT  ${RGB} )
# replacement expression to search for
SET (SEARCH_REGEX "RGB_FILE")
FILE(READ colors.c FILE_CONTENT)
STRING(REGEX REPLACE "${SEARCH_REGEX}" "${REPLACEMENT_TEXT}"
        MODIFIED_FILE_CONTENT "${FILE_CONTENT}")
FILE(WRITE colors_out.c "${MODIFIED_FILE_CONTENT}")


#if !(defined(SunOSPlatform) && (OSMajorVersion>=5))
#IF(CMAKE_SYSTEM MATCHES "SunOS-5.*")
#http://www.na-mic.org/svn/Slicer3-lib-mirrors/trunk/CMake/Modules/Platform/SunOS.cmake
IF( NOT CMAKE_SYSTEM MATCHES "SunOS*")
  SET(GETOPTC getopt.c)
ENDIF()


SET(fig_SRCS  fig2dev.c psfonts.c iso2tex.c arrow.c bound.c
  #colors.c
  colors_out.c
  trans_spline.c
	free.c read.c read1_3.c latex_line.c localmath.c
  #)#$(STRSTRC)
  ${GETOPTC} )


ADD_EXECUTABLE( fig2dev ${fig_SRCS})

TARGET_LINK_LIBRARIES( fig2dev dev ${XPMLIBS})

INSTALL(TARGETS fig2dev DESTINATION bin)

