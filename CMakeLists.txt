cmake_minimum_required(VERSION 3.15)
project(bubbleRobClient_remoteApi)

set(CMAKE_INCLUDE_CURRENT_DIR ON)
set(CMAKE_MACOSX_RPATH 1)

find_package(Boost REQUIRED COMPONENTS system)
if(UNIX AND NOT APPLE)
    find_package(Threads REQUIRED)
endif()

if(NOT LIBPLUGIN_DIR)
    if(DEFINED ENV{COPPELIASIM_ROOT_DIR})
        set(LIBPLUGIN_DIR $ENV{COPPELIASIM_ROOT_DIR}/programming/libPlugin)
    else()
        set(LIBPLUGIN_DIR ${CMAKE_CURRENT_SOURCE_DIR}/../libPlugin)
    endif()
endif()
list(APPEND CMAKE_MODULE_PATH ${LIBPLUGIN_DIR}/cmake)
find_package(CoppeliaSim 4.5.0.0 REQUIRED)

coppeliasim_add_executable(bubbleRobClient_remoteApi
    SOURCES
    bubbleRobClient.cpp
    ${COPPELIASIM_COMMON_DIR}/shared_memory.c
    ../remoteApi/extApi.c
    ../remoteApi/extApiPlatform.c
)
target_compile_definitions(bubbleRobClient_remoteApi PRIVATE NON_MATLAB_PARSING)
target_compile_definitions(bubbleRobClient_remoteApi PRIVATE MAX_EXT_API_CONNECTIONS=255)
target_include_directories(bubbleRobClient_remoteApi PRIVATE ${COPPELIASIM_INCLUDE_DIR})
target_include_directories(bubbleRobClient_remoteApi PRIVATE ../remoteApi)
target_link_libraries(bubbleRobClient_remoteApi Boost::system)
if(UNIX AND NOT APPLE)
    target_link_libraries(bubbleRobClient_remoteApi Threads::Threads rt)
endif()
