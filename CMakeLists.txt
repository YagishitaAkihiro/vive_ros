cmake_minimum_required(VERSION 2.8.3)
project(vive_ros)

## Find catkin macros and libraries
find_package(catkin REQUIRED COMPONENTS
  roscpp
  tf
  std_srvs
  geometry_msgs
)
INCLUDE(FindPkgConfig)

PKG_SEARCH_MODULE(SDL2 REQUIRED sdl2)

set(CMAKE_BUILD_TYPE "Release")

# TODO: make find script, to use find_package(openvr REQUIRED)
set(OPENVR "$ENV{HOME}/openvr")
find_library(OPENVR_LIB openvr_api "${OPENVR}/lib/linux64" NO_DEFAULT_PATH)
message("Found libopenvr_api: ${OPENVR_LIB}")


###################################
## catkin specific configuration ##
###################################
catkin_package()

###########
## Build ##
###########
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 ")

## Specify additional locations of header files
## Your package locations should be listed before other locations
# TODO: find openvr
include_directories(
  include
  ${catkin_INCLUDE_DIRS}
  ${OPENVR}/headers
)
INCLUDE_DIRECTORIES(${SDL2_INCLUDE_DIRS})

# VIVE node:
add_executable(vive_node 
  src/vive_node.cpp
  src/vr_interface.cpp
)
target_link_libraries(vive_node ${catkin_LIBRARIES} ${OPENVR_LIB} ${SDL2_LIBRARIES})
