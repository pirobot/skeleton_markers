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


link_directories(
  ${catkin_LIBRARY_DIRS}
  ${Boost_LIBRARY_DIRS}
  ${orocos_kdl_LIBRARY_DIRS}
  ${OpenNI_LIBRARIES}
)

include_directories(${catkin_INCLUDE_DIRS}
                    ${OpenNI_INCLUDE_DIRS}
                    ${orocos_kdl_INCLUDE_DIRS})

catkin_python_setup()

add_message_files (
  FILES
  Skeleton.msg
)

generate_messages(
  DEPENDENCIES geometry_msgs std_msgs
)

catkin_package(
    DEPENDS openni_camera openni_tracker
    CATKIN DEPENDS message_runtime
                   rospy
		   roscpp
		   tf
		   visualization_msgs
		   std_msgs
		   geometry_msgs
)

add_executable(skeleton_tracker
  src/skeleton_tracker.cpp
  src/KinectController.cpp
  src/KinectDisplay.cpp)

target_link_libraries(skeleton_tracker
  glut
  ${catkin_LIBRARIES}
  ${OpenNI_LIBRARIES}
  ${orocos_kdl_LIBRARIES})

add_dependencies(skeleton_tracker ${skeleton_markers_EXPORTED_TARGETS})

install(TARGETS skeleton_tracker RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION})

message("DEBUG variable catkin_INCLUDE_DIRS: ${catkin_INCLUDE_DIRS}")
message("DEBUG variable OpenNI_INCLUDE_DIRS: ${OpenNI_INCLUDE_DIRS}")
message("DEBUG variable orocos_kdl_INCLUDE_DIRS: ${orocos_kdl_INCLUDE_DIRS}")

