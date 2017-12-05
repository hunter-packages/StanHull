# Copyright (c) 2016 David Hirvonen
# Copyright (c) 2017 Ruslan Baratov
# All rights reserved.

cmake_minimum_required(VERSION 3.2)
project(stanhull VERSION 1.0.0)

add_library(stanhull stanhull.cpp stanhull.h)

set(generated_headers "${CMAKE_CURRENT_BINARY_DIR}/generated_headers")
set(stanhull_export "${generated_headers}/stanhull/STANHULL_EXPORT.h")
include(GenerateExportHeader)
generate_export_header(stanhull EXPORT_FILE_NAME ${stanhull_export})

target_include_directories(
    stanhull
    PUBLIC
    "$<BUILD_INTERFACE:${generated_headers}>"
    "$<BUILD_INTERFACE:${CMAKE_CURRENT_LIST_DIR}>"
)

# Installation (https://github.com/forexample/package-example) {

set(config_install_dir "lib/cmake/${PROJECT_NAME}")
set(include_install_dir "include")

set(generated_dir "${CMAKE_CURRENT_BINARY_DIR}/generated")

set(version_config "${generated_dir}/${PROJECT_NAME}ConfigVersion.cmake")
set(project_config "${generated_dir}/${PROJECT_NAME}Config.cmake")
set(TARGETS_EXPORT_NAME "${PROJECT_NAME}Targets")
set(namespace "${PROJECT_NAME}::")

include(CMakePackageConfigHelpers)

# Use:
#   * PROJECT_VERSION
write_basic_package_version_file(
    "${version_config}" COMPATIBILITY SameMajorVersion
)

# Use variables:
#   * TARGETS_EXPORT_NAME
#   * PROJECT_NAME
configure_package_config_file(
    "cmake/Config.cmake.in"
    "${project_config}"
    INSTALL_DESTINATION "${config_install_dir}"
)

install(
    TARGETS stanhull
    EXPORT "${TARGETS_EXPORT_NAME}"
    LIBRARY DESTINATION "lib"
    ARCHIVE DESTINATION "lib"
    RUNTIME DESTINATION "bin"
    INCLUDES DESTINATION "${include_install_dir}"
)

install(
    FILES "${stanhull_export}" stanhull.h
    DESTINATION "${include_install_dir}/${PROJECT_NAME}"
)

install(
    FILES "${project_config}" "${version_config}"
    DESTINATION "${config_install_dir}"
)

install(
    EXPORT "${TARGETS_EXPORT_NAME}"
    NAMESPACE "${namespace}"
    DESTINATION "${config_install_dir}"
)

# }
