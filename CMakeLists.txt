cmake_minimum_required(VERSION 2.8.3)
project(skeleton_markers)

find_package(orocos_kdl REQUIRED)
find_package(catkin REQUIRED COMPONENTS
  roscpp
  rospy
  tf
  geometry_msgs
  message_generation)

# Find OpenNI
find_package(PkgConfig)
pkg_check_modules(OpenNI REQUIRED libopenni)

# Find Nite
find_path(Nite_INCLUDEDIR
          NAMES XnVNite.h
          HINTS /usr/include/nite /usr/local/include/nite)
find_library(Nite_LIBRARY
             NAMES XnVNite_1_3_1
             HINTS /usr/lib /usr/local/lib
             PATH_SUFFIXES lib) 

link_directories(
  ${catkin_LIBRARY_DIRS}
  ${Boost_LIBRARY_DIRS}
  ${orocos_kdl_LIBRARY_DIRS}
  ${OpenNI_LIBRARIES}
  ${Nite_LIBRARY}
)

include_directories(${catkin_INCLUDEDIR}
                    ${OpenNI_INCLUDEDIR}
                    ${Nite_INCLUDEDIR}
                    ${orocos_kdl_INCLUDE_DIRS})

set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)

set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)

add_message_files (
  FILES
  Skeleton.msg
)

generate_messages(
  DEPENDENCIES geometry_msgs std_msgs
)

catkin_package(
    DEPENDS rospy roscpp visualization_msgs std_msgs geometry_msgs
    openni_camera openni_tracker tf
    CATKIN-DEPENDS message_runtime
    INCLUDE_DIRS # TODO
    LIBRARIES # TODO
)

add_executable(skeleton_tracker
  src/skeleton_tracker.cpp
  src/KinectController.cpp
  src/KinectDisplay.cpp)

target_link_libraries(skeleton_tracker
  glut
  ${catkin_LIBRARIES}
  ${OpenNI_LIBRARIES}
  ${Nite_LIBRARY}
  ${orocos_kdl_LIBRARIES})

install(TARGETS skeleton_tracker RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION})
