cmake_minimum_required(VERSION 3.5)
project(dolphin-plugins)

set(CMAKE_CXX_EXTENSIONS OFF)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

if("${CMAKE_CXX_COMPILER_ID}" STREQUAL "GNU" OR
        "${CMAKE_CXX_COMPILER_ID}" STREQUAL "Clang")
    add_compile_options("$<$<CONFIG:DEBUG>:-Wall;-Wextra;-pedantic>")
endif()

find_package(KDE4 REQUIRED)
include(KDE4Defaults)
include(MacroLibrary)

add_definitions (${QT_DEFINITIONS} ${KDE4_DEFINITIONS})
add_definitions(-DQT_USE_FAST_CONCATENATION -DQT_USE_FAST_OPERATOR_PLUS)
include_directories (${CMAKE_SOURCE_DIR} ${CMAKE_BINARY_DIR} ${KDE4_INCLUDES} ${KDEPIMLIBS_INCLUDE_DIR})

set(FPHSA_NAME_MISMATCHED TRUE)
macro_optional_find_package(LibKonq REQUIRED)
macro_log_feature(LIBKONQ_FOUND "KDE-Base/LibKonq" "Provides high-level file management functions." "http://dolphin.kde.org/" FALSE "" "Required to build the Dolphin version control plugins.")

if(LIBKONQ_FOUND)
  macro_optional_add_subdirectory(svn)
  macro_optional_add_subdirectory(git)
  macro_optional_add_subdirectory(hg)
  macro_optional_add_subdirectory(bazaar)
  macro_optional_add_subdirectory(dropbox)
endif(LIBKONQ_FOUND)
macro_display_feature_log()
