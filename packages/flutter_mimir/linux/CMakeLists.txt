set(MimirVersion "mimir-v0.1.6") # generated; do not edit

# The Flutter tooling requires that developers have CMake 3.10 or later
# installed. You should not increase this version, as doing so will cause
# the plugin to fail to compile for some customers of the plugin.
cmake_minimum_required(VERSION 3.10)

# Project-level configuration.
set(PROJECT_NAME "flutter_mimir")
project(${PROJECT_NAME} LANGUAGES CXX)

# Download the binaries if they are not already present.
set(MimirLibRoot "${CMAKE_CURRENT_SOURCE_DIR}/${MimirVersion}")
set(MimirArchivePath "${MimirLibRoot}.tar.gz")
if(NOT EXISTS ${MimirArchivePath})
  file(DOWNLOAD
    "https://github.com/GregoryConrad/mimir/releases/download/${MimirVersion}/EmbeddedMilli.tar.gz"
    ${MimirArchivePath}
    TLS_VERIFY ON
  )
endif()

# Extract the binaries, overriding any already present.
file(REMOVE_RECURSE ${MimirLibRoot})
file(MAKE_DIRECTORY ${MimirLibRoot})
execute_process(
  COMMAND ${CMAKE_COMMAND} -E tar xzf ${MimirArchivePath}
  WORKING_DIRECTORY ${MimirLibRoot}
)

# List of absolute paths to libraries that should be bundled with the plugin.
# This list could contain prebuilt libraries, or libraries created by an
# external build triggered from this build file.
set(flutter_mimir_bundled_libraries
  "${MimirLibRoot}/${FLUTTER_TARGET_PLATFORM}/libembedded_milli.so"
  PARENT_SCOPE
)
