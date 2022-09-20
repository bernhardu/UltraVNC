cmake_minimum_required(VERSION 3.1.0)

project(uvnc_settings LANGUAGES C CXX RC)

if ("${CMAKE_CXX_COMPILER_ID}" STREQUAL "GNU")
  set(CMAKE_C_FLAGS "-g -O0 -Wno-write-strings")
  set(CMAKE_CXX_FLAGS "-g -O0 -Wno-write-strings -Wno-conversion-null")
endif()



set(VNCVIEWER_SOURCES
    capture.cpp
    connections.cpp
    createsfx2.cpp
    createsfx3.cpp
    dsmplugin.cpp
    filetransfer.cpp
    firewall.cpp
    inifile.cpp
    log.cpp
    misc.cpp
    network.cpp
    security.cpp
    service.cpp
    stdafx.cpp
    upnp.cpp
    uvnc_settings.cpp
    videodrivercheck.cpp
    vncOSVersion.cpp
    vncsetauth.cpp
)
add_executable(uvnc_settings)
target_include_directories(uvnc_settings PRIVATE "${CMAKE_SOURCE_DIR}/../DSMPlugin")
target_include_directories(uvnc_settings PRIVATE "${CMAKE_SOURCE_DIR}/../vncviewer/omnithread")
foreach(file ${VNCVIEWER_SOURCES})
    target_sources(uvnc_settings PRIVATE "${CMAKE_SOURCE_DIR}/../uvnc_settings2/uvnc_settings/${file}")
endforeach(file)
target_sources(uvnc_settings PRIVATE "${CMAKE_SOURCE_DIR}/../uvnc_settings2/uvnc_settings/uvnc_settings.rc")
set_source_files_properties(${CMAKE_SOURCE_DIR}/../uvnc_settings2/uvnc_settings/uvnc_settings.rc LANGUAGE RC)
if ("${CMAKE_CXX_COMPILER_ID}" STREQUAL "GNU")
  set_source_files_properties(${CMAKE_SOURCE_DIR}/../uvnc_settings2/uvnc_settings/uvnc_settings.rc PROPERTIES COMPILE_FLAGS "-Ocoff")
endif()
if( ${CMAKE_SYSTEM_PROCESSOR} STREQUAL "x86_64" OR ${CMAKE_SYSTEM_PROCESSOR} STREQUAL "AMD64")
    target_compile_definitions(uvnc_settings PRIVATE _X64)
endif()
target_link_libraries(uvnc_settings
    libomnithread_vncviewer
    comctl32
    version
)
set_target_properties(uvnc_settings PROPERTIES WIN32_EXECUTABLE TRUE)
