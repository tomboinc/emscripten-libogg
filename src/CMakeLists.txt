
cmake_minimum_required(VERSION 2.8)
project( ogg ) 
set( ogg_VERSION_MAJOR 1)
set( ogg_VERSION_MINOR 3)
set( ogg_VERSION_PATCH 1)

add_library( ogg STATIC framing.c bitwise.c )

set_target_properties( ogg
	PROPERTIES
		LINK_FLAGS -no-undefined
		INCLUDE_DIRECTORIES "../include"
		PUBLIC_HEADER "../include/ogg/ogg.h;../include/ogg/os_types.h"
)

# Set variables for substitution in pkg-config file
set( prefix      /usr/local          )
set( exec_prefix \${prefix}          )
set( libdir      \${exec_prefix}/lib )
set( includedir  \${prefix}/include  )

set( VERSION ${ogg_VERSION_MAJOR}.${ogg_VERSION_MINOR}.${ogg_VERSION_PATCH} )

configure_file( ../ogg.pc.in ${CMAKE_BINARY_DIR}/ogg.pc @ONLY )

install( TARGETS ogg
	DESTINATION
		ARCHIVE DESTINATION lib
		LIBRARY DESTINATION lib
		PUBLIC_HEADER DESTINATION include/ogg
)

install( FILES ${CMAKE_BINARY_DIR}/ogg.pc DESTINATION lib/pkgconfig )

