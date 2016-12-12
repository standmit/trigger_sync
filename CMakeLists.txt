cmake_minimum_required(VERSION 2.8.3)
project(trigger_sync)

## Find catkin macros and libraries
## if COMPONENTS list like find_package(catkin REQUIRED COMPONENTS xyz)
## is used, also find other catkin packages

find_package(catkin REQUIRED COMPONENTS
  std_msgs
  roscpp
  rospy
  nav_msgs
  sensor_msgs
  tf
  message_filters
  cv_bridge
  image_transport
  image_geometry
  message_filters
  camera_info_manager
)

## Generate messages in the 'msg' folder
add_message_files(
  DIRECTORY msg
  FILES
  Event.msg
  EventStamped.msg

)

## Generate added messages and services with any dependencies listed here
generate_messages(
  DEPENDENCIES
  std_msgs  # Or other packages containing msgs
)

###################################
## catkin specific configuration ##
###################################
## The catkin_package macro generates cmake config files for your package
## Declare things to be passed to dependent projects
## INCLUDE_DIRS: uncomment this if you package contains header files
## LIBRARIES: libraries you create in this project that dependent projects also need
## CATKIN_DEPENDS: catkin_packages dependent projects also need
## DEPENDS: system dependencies of this project that dependent projects also need
catkin_package(
  INCLUDE_DIRS include
  LIBRARIES trigger_sync_lib
  CATKIN_DEPENDS roscpp
)

###########
## Build ##
###########

## Specify additional locations of header files
## Your package locations should be listed before other locations
include_directories(include ${catkin_INCLUDE_DIRS})

## Declare a cpp library
add_library(trigger_sync_lib
   src/trigger_sync.cpp
)
target_link_libraries(trigger_sync_lib ${catkin_LIBRARIES})
add_dependencies(trigger_sync_lib trigger_sync_generate_messages_cpp)

## Declare a cpp executable
add_executable(pps_trigger             src/pps_trigger.cpp)
add_executable(pps_config              src/pps_config.c)
add_executable(local_clock_syncroniser src/local_clock_syncroniser_node.cpp)
add_executable(test_trigger_sync       src/test_trigger_sync.cpp)

# Link executables to libraries
target_link_libraries(pps_trigger       ${catkin_LIBRARIES} trigger_sync_lib)
target_link_libraries(test_trigger_sync ${catkin_LIBRARIES} trigger_sync_lib)
target_link_libraries(local_clock_syncroniser ${catkin_LIBRARIES} trigger_sync_lib)

install(PROGRAMS src/plot_clock.py
  DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)
