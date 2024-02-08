cmake_minimum_required(VERSION 2.6)
Project("SDFGen")

# Set the build type.  Options are:
#  Coverage       : w/ debug symbols, w/o optimization, w/ code-coverage
#  Debug          : w/ debug symbols, w/o optimization
#  Release        : w/o debug symbols, w/ optimization
#  RelWithDebInfo : w/ debug symbols, w/ optimization
#  MinSizeRel     : w/o debug symbols, w/ optimization, stripped binaries

# SET(CMAKE_BUILD_TYPE Coverage)
SET(CMAKE_BUILD_TYPE Release)

#set the default path for built executables to the "bin" directory
set(EXECUTABLE_OUTPUT_PATH ${CMAKE_CURRENT_BINARY_DIR}/bin)

#These flags might not work on every system, especially the release flags, comment out as needed
#set(CMAKE_CXX_FLAGS "-O2")
set(CMAKE_CXX_FLAGS_RELEASE "-O2 -march=native")
#set(ENABLE_CXX_EXCEPTIONS 1)

#checks if VTK is available
find_package(VTK QUIET)
if(VTK_FOUND)
	set(HAVE_VTK 1)
endif()

#Generates config.h replacing expressions in config.h.in with actual values
configure_file(
  "${CMAKE_CURRENT_SOURCE_DIR}/config.h.in"
  "${CMAKE_CURRENT_SOURCE_DIR}/config.h"
)

add_executable(${PROJECT_NAME} main.cpp  makelevelset3.cpp)
#target_compile_options (SDFGen PUBLIC -fexceptions)
if(VTK_FOUND)
	include_directories(${VTK_INCLUDE_DIRS})
	target_link_libraries(${PROJECT_NAME} ${VTK_LIBRARIES})
endif()