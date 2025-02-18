cmake_minimum_required(VERSION 3.4)
project(dr_mp3 C)

include(conanbuildinfo.cmake)
conan_basic_setup(KEEP_RPATHS)

option(NO_SIMD "Build with define DR_MP3_NO_SIMD" OFF)
option(NO_STDIO "Build with define DR_MP3_NO_STDIO" OFF)

add_library(${CMAKE_PROJECT_NAME} dr_mp3.c)

target_include_directories(${CMAKE_PROJECT_NAME} PRIVATE source_subfolder)

set_target_properties(${CMAKE_PROJECT_NAME} PROPERTIES 
    PUBLIC_HEADER source_subfolder/dr_mp3.h
    C_STANDARD 99
)

if(BUILD_SHARED_LIBS)
    target_compile_definitions(${CMAKE_PROJECT_NAME} PUBLIC -DDRMP3_DLL)
endif()
if(NO_SIMD)
    target_compile_definitions(${CMAKE_PROJECT_NAME} PUBLIC -DDR_MP3_NO_SIMD)
endif()
if(NO_STDIO)
    target_compile_definitions(${CMAKE_PROJECT_NAME} PUBLIC -DDR_MP3_NO_STDIO)
endif()

install(TARGETS ${CMAKE_PROJECT_NAME}
    RUNTIME DESTINATION bin
    LIBRARY DESTINATION lib
    ARCHIVE DESTINATION lib
    PUBLIC_HEADER DESTINATION include
)
