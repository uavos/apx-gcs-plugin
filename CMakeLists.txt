cmake_minimum_required(VERSION 3.19 FATAL_ERROR)

# paths used by apx_module
get_filename_component(APX_SHARED_DIR
                       "${CMAKE_CURRENT_SOURCE_DIR}/apx-gcs/shared" ABSOLUTE)
get_filename_component(APX_MODULES_DIR
                       "${CMAKE_CURRENT_SOURCE_DIR}/apx-gcs/src" ABSOLUTE)

list(APPEND CMAKE_MODULE_PATH ${APX_SHARED_DIR}/cmake)
list(APPEND CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/apx-gcs/cmake)

set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/static)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/out/lib)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/out/bin)

set(APX_PLUGINS_OUTPUT_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/out/plugins)

set(APX_RESOURCES_DIR "${CMAKE_SOURCE_DIR}/apx-gcs/resources")

if(CMAKE_BUILD_TYPE STREQUAL Release)
  set(CMAKE_INTERPROCEDURAL_OPTIMIZATION ON) # LTO
endif()

option(CMAKE_VERBOSE_MAKEFILE "" OFF)

set(CMAKE_PREFIX_PATH
    "$ENV{HOME}/QtOS/5.15.2/clang_64"
    CACHE PATH "Qt installation path")

set(CMAKE_INSTALL_PREFIX
    "${CMAKE_CURRENT_BINARY_DIR}/install"
    CACHE PATH "")

set(CMAKE_OSX_DEPLOYMENT_TARGET
    "10.13"
    CACHE STRING "Minimum OS X deployment version")

project(
  gcs-plugin
  VERSION "1.0.0"
  DESCRIPTION "APX Ground Control Plugin"
  HOMEPAGE_URL "https://github.com/uavos/apx-gcs-plugin"
  LANGUAGES CXX C)

include(apx)

include(ccache)

set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)
set(CMAKE_AUTOUIC ON)

find_package(
  Qt5
  COMPONENTS Core QuickCompiler
  REQUIRED)
get_filename_component(QT_DIR "${Qt5Core_DIR}/../../.." ABSOLUTE)

include(apx_gcs_qt)
include(apx_gcs_qrc)
include(apx_gcs_lib)
include(apx_gcs_plugin)
include(apx_gcs_framework)
include(apx_gcs_install)

apx_plugin()
