
# mkdir build
# cd build/
# cmake .. -DCMAKE_INSTALL_PREFIX:PATH=/usr/local
# make install

PROJECT(transfig C)
CMAKE_MINIMUM_REQUIRED(VERSION 2.6)
CMAKE_POLICY(VERSION 2.6)


ADD_SUBDIRECTORY( fig2dev )
ADD_SUBDIRECTORY( transfig )


#manual:

#transfig.man:: doc/transfig.1
#	-ln -s ../doc/transfig.1 transfig.man
#	cd doc/manual; $(MAKE); latex manual; latex manual

#clean::
#	cd doc/manual; $(MAKE) clean

