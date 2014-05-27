
#INCLUDES = -I.. -I../..
INCLUDE_DIRECTORIES( ${CMAKE_CURRENT_SOURCE_DIR} ${CMAKE_CURRENT_SOURCE_DIR}/..  ${CMAKE_CURRENT_SOURCE_DIR}/../.. )

# work around the expansion of BITMAPDIR variables
#http://magnesjaastad.wordpress.com/2012/05/06/search-and-replace-using-cmake/
SET(BITMAPDIR \"/foo/bar/\")
MESSAGE("Replacing text")
SET ( FILE_LIST gentk.c genptk.c) #files affected
# read replacement text from file
#set( REPLACEMENT_TEXT  \"/foobar/dir/\" )
SET( REPLACEMENT_TEXT  ${BITMAPDIR} )
# replacement expression to search for
SET (SEARCH_REGEX "BITMAPDIR")
# replace text in all files
MESSAGE("Replacing text")
FOREACH (installFile ${FILE_LIST})
  FILE(READ ${installFile} FILE_CONTENT)
  STRING(REGEX REPLACE "${SEARCH_REGEX}" "${REPLACEMENT_TEXT}"
        MODIFIED_FILE_CONTENT "${FILE_CONTENT}")
  FILE(WRITE ${installFile}_out.c "${MODIFIED_FILE_CONTENT}")
ENDFOREACH (installFile)


IF (USEPNG)
  SET( READPNGS readpng.c)
ENDIF (USEPNG)


IF (USEXPM)
 SET( READXPMS readxpm.c)
 INCLUDE_DIRECTORIES( ${XPMINC} )      # one header is #inclue X11/fo.h
 INCLUDE_DIRECTORIES( ${XPMINC}/X11 )  # however other #include xpm.h
ENDIF (USEXPM)


SET(T_SRCS genbox.c gencgm.c gendxf.c genepic.c gengbx.c genibmgl.c genlatex.c genmap.c genmf.c genpic.c
	genpictex.c genps.c genpdf.c genpstex.c genpstricks.c gentextyl.c
  gentk.c_out.c  # <- gentk.c
  genptk.c_out.c # <- genptk.c
  gentpic.c
	genbitmaps.c genge.c genmp.c genemf.c gensvg.c genshape.c setfigfont.c psencode.c
	readpics.c readeps.c readgif.c readpcx.c readppm.c readpng.c readxpm.c
	readxbm.c readtif.c readjpg.c asc85ec.c ${READPNGS} ${READXPMS} )


ADD_LIBRARY( dev ${T_SRCS})



