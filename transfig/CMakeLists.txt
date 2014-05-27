
#DEFINES = $(USELATEX2E)
#DEPLIBS =
#INCLUDES = -I..

INCLUDE_DIRECTORIES( ${CMAKE_SOURCE_DIR} )

SET(SRCS transfig.c txfile.c mkfile.c sys.c)

ADD_EXECUTABLE( transfig ${SRCS})

#all:: transfig.man fig2dev.man fig2ps2tex.man pic2tpic.man

#transfig.man:: ../doc/transfig.1
#	$(RM) transfig.man
#	-ln -s ../doc/transfig.1 transfig.man
#
#InstallManPage(fig2ps2tex,$(MANDIR))
#InstallManPage(pic2tpic,$(MANDIR))

INSTALL(TARGETS transfig DESTINATION bin)

#
#fig2dev.man:: ../doc/fig2dev.1
#	$(RM) fig2dev.man
#	-ln -s ../doc/fig2dev.1 fig2dev.man
#
#fig2ps2tex.man:: ../doc/fig2ps2tex.1
#	$(RM) fig2ps2tex.man
#	-ln -s ../doc/fig2ps2tex.1 fig2ps2tex.man
#
#pic2tpic.man:: ../doc/pic2tpic.1
#	$(RM) pic2tpic.man
#	-ln -s ../doc/pic2tpic.1 pic2tpic.man
#
#clean::
#	rm -f *.man
