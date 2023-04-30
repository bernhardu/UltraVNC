cmake_minimum_required(VERSION 3.1.0)

project(vnchooks LANGUAGES C CXX RC)

set(VNCVIEWER_SOURCES
    VNCHooks.cpp
)
add_library(vnchooks SHARED)
set_target_properties(vnchooks PROPERTIES PREFIX "")
foreach(file ${VNCVIEWER_SOURCES})
    target_sources(vnchooks PRIVATE "${CMAKE_SOURCE_DIR}/../winvnc/vnchooks/${file}")
endforeach(file)
target_sources(vnchooks PRIVATE "${CMAKE_SOURCE_DIR}/../winvnc/vnchooks/vnchooks.rc")
set_source_files_properties(${CMAKE_SOURCE_DIR}/../winvnc/vnchooks/vnchooks.rc LANGUAGE RC)
if ("${CMAKE_CXX_COMPILER_ID}" STREQUAL "GNU")
  set_source_files_properties(${CMAKE_SOURCE_DIR}/../winvnc/vnchooks/vnchooks.rc PROPERTIES COMPILE_FLAGS "-Ocoff")
endif()
