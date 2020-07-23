cmake_minimum_required( VERSION 2.8.11 )
project( QmlVlcPlugin )

if( NOT MSVC )
    add_definitions( -std=c++11 )
endif()

if( QT_STATIC )
    add_definitions( -DQT_OPENGL_ES_2_ANGLE_STATIC )
endif()

add_subdirectory( libvlc_wrapper )

set( CMAKE_AUTOMOC ON )

find_package( Qt5
	COMPONENTS Core Qml Quick 
	)

file( GLOB SOURCE_FILES RELATIVE ${CMAKE_CURRENT_SOURCE_DIR}
    [^.]*.cpp
    [^.]*.h
    )

file( GLOB PLUGIN_SOURCE_FILES RELATIVE ${CMAKE_CURRENT_SOURCE_DIR}
    QmlVlcPlugin.cpp
    QmlVlcPlugin.h
    )

list( REMOVE_ITEM SOURCE_FILES ${PLUGIN_SOURCE_FILES} )

add_library( QmlVlc STATIC ${SOURCE_FILES} )
set_target_properties( QmlVlc PROPERTIES POSITION_INDEPENDENT_CODE ON )
target_include_directories( QmlVlc PUBLIC "." )
target_link_libraries( QmlVlc Qt5::Quick libvlc_wrapper )
