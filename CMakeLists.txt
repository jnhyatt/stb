cmake_minimum_required (VERSION 3.0)

project (stb
    VERSION 1.0.0
    LANGUAGES C
)

add_library (stb_image INTERFACE)
add_library (stb::stb_image ALIAS stb_image)

add_library (stb_truetype INTERFACE)
add_library (stb::stb_truetype ALIAS stb_truetype)

target_include_directories (stb_image
    INTERFACE
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
        $<INSTALL_INTERFACE:include>
)

target_include_directories (stb_truetype
    INTERFACE
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
        $<INSTALL_INTERFACE:include>
)

include (CMakePackageConfigHelpers)

write_basic_package_version_file (
    stbConfigVersion.cmake
    VERSION ${PACKAGE_VERSION}
    COMPATIBILITY SameMajorVersion
)

configure_file (
    cmake/stbConfig.cmake.in
    stbConfig.cmake
    @ONLY
)

install (
    FILES
        stb_image.h
        stb_truetype.h
    DESTINATION
        include
)

install (
    TARGETS stb_image stb_truetype
    EXPORT stbTargets
    INCLUDES DESTINATION include
)

install (
    EXPORT stbTargets
    FILE stbTargets.cmake
    NAMESPACE stb::
    DESTINATION lib/cmake/stb
)

install (
    FILES
        ${CMAKE_CURRENT_BINARY_DIR}/stbConfig.cmake
        ${CMAKE_CURRENT_BINARY_DIR}/stbConfigVersion.cmake
    DESTINATION
        lib/cmake/stb
)
