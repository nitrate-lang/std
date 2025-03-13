cmake_minimum_required(VERSION 3.15)
set(THIS_PROJECT_NAME "nitrate-std")
project(${THIS_PROJECT_NAME})

################################################################################
# USER CONFIGURATION SECTION
################################################################################
set(PACKAGE_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}")
find_program(NITRATE_TOOL_EXE "nitrate" REQUIRED)

################################################################################
# SCRIPT INVARIANT ASSERTIONS
################################################################################
message(STATUS "Found compiler at ${NITRATE_TOOL_EXE}")
if(NOT BUILD_MODE)
  set(BUILD_MODE "--rapid")
elseif(NOT BUILD_MODE STREQUAL "--rapid" AND NOT BUILD_MODE STREQUAL "--debug" AND NOT BUILD_MODE STREQUAL "--release")
  message(FATAL_ERROR "Invalid build mode: ${BUILD_MODE}. Valid options are '--rapid', '--debug', '--release'")
else()
  message(STATUS "Building ${THIS_PROJECT_NAME} in ${BUILD_MODE} mode")
endif()

################################################################################
# INVOKE THE NITRATE BUILD TOOL
################################################################################
add_custom_target(
  ${THIS_PROJECT_NAME} # Set the target name
  ALL # Run this target everytime; Caching is the nitrate build tools job.
  COMMAND ${NITRATE_TOOL_EXE} build ${BUILD_MODE} ${PACKAGE_DIRECTORY} # Invoke the build tool
)
