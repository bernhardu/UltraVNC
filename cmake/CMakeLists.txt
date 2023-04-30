cmake_minimum_required(VERSION 3.1.0)

project(UltraVNC)

if ("${CMAKE_CXX_COMPILER_ID}" STREQUAL "GNU")
  set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -g -O0 -Wno-write-strings")
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -g -O0 -Wno-write-strings -Wno-conversion-null")
endif()

add_subdirectory(libzstd)
add_subdirectory(libjpeg)
add_subdirectory(libudt4)
add_subdirectory(libudtcloud)
add_subdirectory(uvnc_settings)
add_subdirectory(vnchooks)
add_subdirectory(vncviewer)
add_subdirectory(winvnc)
