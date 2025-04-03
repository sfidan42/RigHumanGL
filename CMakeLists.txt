cmake_minimum_required(VERSION 3.10)
project(RigHumanGL)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

include_directories(
    include
    ${CMAKE_SOURCE_DIR}/external/include
)

set(SOURCES
    main.cpp
    ${CMAKE_SOURCE_DIR}/external/source/glad.c
)

# Create executable
add_executable(${PROJECT_NAME} ${SOURCES})

# Link GLFW and OpenGL libraries
target_link_libraries(${PROJECT_NAME}
    ${CMAKE_SOURCE_DIR}/external/source/libglfw3.a
    GL
)

# On Linux, we need to link additional system libraries
if(UNIX AND NOT APPLE)
    target_link_libraries(${PROJECT_NAME}
        dl
        X11
        pthread
    )
endif()

# On macOS, we need to link additional frameworks
if(APPLE)
    find_library(COCOA_LIBRARY Cocoa)
    find_library(IOKIT_LIBRARY IOKit)
    find_library(COREVIDEO_LIBRARY CoreVideo)
    find_library(OPENGL_LIBRARY OpenGL)
    target_link_libraries(${PROJECT_NAME}
        ${COCOA_LIBRARY}
        ${IOKIT_LIBRARY}
        ${COREVIDEO_LIBRARY}
        ${OPENGL_LIBRARY}
    )
endif()