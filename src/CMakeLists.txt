
cmake_minimum_required(VERSION 2.8)
project( ogg C ) 
set( ogg_VERSION_MAJOR 1)
set( ogg_VERSION_MINOR 3)
set( ogg_VERSION_PATCH 1)

# Set variables for substitution in pkg-config file
set( prefix      ${CMAKE_INSTALL_PREFIX} )
set( exec_prefix \${prefix}              )
set( libdir      \${exec_prefix}/lib     )
set( includedir  \${prefix}/include      )

set( VERSION ${ogg_VERSION_MAJOR}.${ogg_VERSION_MINOR}.${ogg_VERSION_PATCH} )

configure_file( ../ogg.pc.in ${CMAKE_BINARY_DIR}/ogg.pc @ONLY )

# Check include files
include( CheckIncludeFiles )
check_include_files( inttypes.h  INCLUDE_INTTYPES_H  )
check_include_files( stdint.h    INCLUDE_STDINT_H    )
check_include_files( sys/types.h INCLUDE_SYS_TYPES_H )

# Detect type sizes
include( CheckTypeSize )
set( CMAKE_EXTRA_INCLUDE_FILES stdint.h )
check_type_size( int16_t  SIZE16  )
check_type_size( uint16_t USIZE16 )
check_type_size( int32_t  SIZE32  )
check_type_size( uint32_t USIZE32 )
check_type_size( int64_t  SIZE64  )
set( CMAKE_EXTRA_INCLUDE_FILES )
# above check does not work so override manually

set( SIZE16  int16_t  )
set( USIZE16 uint16_t )
set( SIZE32  int32_t  )
set( USIZE32 uint32_t )
set( SIZE64  int64_t  )

configure_file( ../include/ogg/config_types.h.in ../include/ogg/config_types.h )


add_library( ogg STATIC framing.c bitwise.c )

set_target_properties( ogg
	PROPERTIES
		LINK_FLAGS -no-undefined
		INCLUDE_DIRECTORIES "../include"
		PUBLIC_HEADER "../include/ogg/ogg.h;../include/ogg/os_types.h;../include/ogg/config_types.h"
)

# Set installation

install( TARGETS ogg
	DESTINATION
		ARCHIVE DESTINATION lib
		LIBRARY DESTINATION lib
		PUBLIC_HEADER DESTINATION include/ogg
)

install( FILES ${CMAKE_BINARY_DIR}/ogg.pc DESTINATION lib/pkgconfig )

