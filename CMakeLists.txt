cmake_minimum_required(VERSION 3.10)

add_subdirectory(msdfgen)

project(msdf-atlas-gen)

# Set the C++ standard
set(CMAKE_CXX_STANDARD 17)

# Directory for the output of the library
set(OUTPUT_DIR ${CMAKE_BINARY_DIR}/output)

# Include directories
include_directories(
    ${CMAKE_CURRENT_SOURCE_DIR}/msdf-atlas-gen
    ${CMAKE_CURRENT_SOURCE_DIR}/msdfgen
    ${CMAKE_CURRENT_SOURCE_DIR}/msdfgen/include
)

# Source files
file(GLOB_RECURSE SOURCE_FILES
    "${CMAKE_CURRENT_SOURCE_DIR}/msdf-atlas-gen/*.h"
    "${CMAKE_CURRENT_SOURCE_DIR}/msdf-atlas-gen/*.hpp"
    "${CMAKE_CURRENT_SOURCE_DIR}/msdf-atlas-gen/*.cpp"
)

# Define the static library
add_library(msdf-atlas-gen STATIC ${SOURCE_FILES})

# Definitions
target_compile_definitions(msdf-atlas-gen PRIVATE _CRT_SECURE_NO_WARNINGS)

# Linking with the msdfgen library
target_link_libraries(msdf-atlas-gen PRIVATE msdfgen)

# Compiler options and properties per configuration
foreach(TARGET msdf-atlas-gen)
    set_target_properties(${TARGET} PROPERTIES
        ARCHIVE_OUTPUT_DIRECTORY_DEBUG ${OUTPUT_DIR}/debug/${TARGET}
        ARCHIVE_OUTPUT_DIRECTORY_RELEASE ${OUTPUT_DIR}/release/${TARGET}
        ARCHIVE_OUTPUT_DIRECTORY_MINSIZEREL ${OUTPUT_DIR}/dist/${TARGET}
        LIBRARY_OUTPUT_DIRECTORY_DEBUG ${OUTPUT_DIR}/debug/${TARGET}
        LIBRARY_OUTPUT_DIRECTORY_RELEASE ${OUTPUT_DIR}/release/${TARGET}
        LIBRARY_OUTPUT_DIRECTORY_MINSIZEREL ${OUTPUT_DIR}/dist/${TARGET}
        RUNTIME_OUTPUT_DIRECTORY_DEBUG ${OUTPUT_DIR}/debug/${TARGET}
        RUNTIME_OUTPUT_DIRECTORY_RELEASE ${OUTPUT_DIR}/release/${TARGET}
        RUNTIME_OUTPUT_DIRECTORY_MINSIZEREL ${OUTPUT_DIR}/dist/${TARGET}
    )
endforeach()
