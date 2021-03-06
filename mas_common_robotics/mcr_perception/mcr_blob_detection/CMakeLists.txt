cmake_minimum_required(VERSION 2.8.3)
project(mcr_blob_detection)

find_package(catkin REQUIRED COMPONENTS
    cv_bridge
    dynamic_reconfigure
    geometry_msgs
    image_transport
    roscpp
    sensor_msgs
    std_msgs
    mcr_perception_msgs
    cvblobs
)

generate_dynamic_reconfigure_options(
  ros/config/BlobDetection.cfg
)

catkin_package(
  INCLUDE_DIRS
    ros/include
    common/include
  CATKIN_DEPENDS
    cv_bridge
    dynamic_reconfigure
    geometry_msgs
    image_transport
    roscpp
    sensor_msgs
    std_msgs
    mcr_perception_msgs
    cvblobs
)

include_directories(
  ros/include
  common/include
  ${catkin_INCLUDE_DIRS}
)

add_executable(blob_detection_node
  ros/src/blob_detection_node.cpp
  common/src/blob_detection.cpp
)

add_dependencies(blob_detection_node
  ${PROJECT_NAME}_gencfg
  ${catkin_EXPORTED_TARGETS}
)

target_link_libraries(blob_detection_node
   ${catkin_LIBRARIES}
)

### TESTS
if(CATKIN_ENABLE_TESTING)
  find_package(roslaunch REQUIRED)

  roslaunch_add_file_check(ros/launch)
endif()

### INSTALLS
install(TARGETS blob_detection_node
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

install(DIRECTORY ros/include/${PROJECT_NAME}/
   DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
   FILES_MATCHING PATTERN "*.h"
)

install(DIRECTORY common/include/${PROJECT_NAME}/
   DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
   FILES_MATCHING PATTERN "*.h"
)

install(DIRECTORY ros/launch
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/ros/launch
)

install(DIRECTORY ros/config
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/ros/config
)
