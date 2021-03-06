cmake_minimum_required(VERSION 2.8.3)
project(lane_detection)

# set default build type
IF(NOT CMAKE_BUILD_TYPE)
  SET(CMAKE_BUILD_TYPE Release)
ENDIF(NOT CMAKE_BUILD_TYPE)

find_package(catkin REQUIRED COMPONENTS
  roscpp
  rospy
  dynamic_reconfigure
  image_transport
  image_geometry
  cv_bridge
  tf
  tf2_ros
  costmap_converter
  visualization_msgs
)
find_package(OpenCV REQUIRED)

generate_dynamic_reconfigure_options(
  cfg/LaneDetection.cfg
)

catkin_package(
  DEPENDS OpenCV
)

include_directories(
  ${catkin_INCLUDE_DIRS}
  ${OpenCV_INCLUDE_DIRS}
)

add_executable(lane_detection_node
  src/lane_detection_node.cpp
  src/LaneDetection.cpp
)
target_link_libraries(lane_detection_node
  ${catkin_LIBRARIES}
)
add_dependencies(lane_detection_node
  ${PROJECT_NAME}_gencfg
)

install(
  TARGETS
    lane_detection_node
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

if(CATKIN_ENABLE_TESTING)
  add_subdirectory(tests)
endif()
