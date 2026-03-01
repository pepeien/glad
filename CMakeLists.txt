##################### Settings #######################
cmake_minimum_required(VERSION 3.16.0)

project(Glad C)

add_library(Glad STATIC ${CMAKE_CURRENT_LIST_DIR}/src/gl.c)
add_library(Glad::Glad ALIAS Glad)

target_include_directories(
    Glad

    PUBLIC
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
        $<INSTALL_INTERFACE:include>
)

set_target_properties(
    Glad

    PROPERTIES
        POSITION_INDEPENDENT_CODE ON
)

if(WIN32)
    target_link_libraries(
        Glad
    
        PUBLIC
            opengl32
    )
endif()

install(
    TARGETS
        Glad
    EXPORT
        GladTargets
    ARCHIVE
        DESTINATION lib
)

install(
    DIRECTORY
        include/
    DESTINATION
        include
)

install(
    EXPORT
        GladTargets
    FILE
        GladTargets.cmake
    NAMESPACE
        Glad::
    DESTINATION
        lib/cmake/Glad
)