cmake_minimum_required(VERSION 2.8.3)
project(beginner_tutorials)

## Compile as C++11, supported in ROS Kinetic and newer
  add_compile_options(-std=c++11)

## Find catkin macros and libraries
find_package(catkin REQUIRED COMPONENTS
  roscpp
	rospy
  std_msgs
	message_generation
  tf
  
)
## Generate messages in the 'msg' folder
  add_message_files(
    FILES
    Num.msg
    
  )
## Generate services in the 'srv' folder
  add_service_files(
    FILES
    changeString.srv
    
  )
## Generate added messages and services with any dependencies listed here
  generate_messages(
    DEPENDENCIES
    std_msgs
  )


catkin_package(
#  INCLUDE_DIRS include
#  LIBRARIES beginner_tutorials
	 CATKIN_DEPENDS roscpp std_msgs message_runtime
#  DEPENDS system_lib
)

include_directories(
# include
  ${catkin_INCLUDE_DIRS}
)

add_executable(talker src/talker.cpp)
target_link_libraries(talker ${catkin_LIBRARIES})
add_dependencies(talker beginner_tutorials_generate_messages_cpp)

add_executable(listener src/listener.cpp)
target_link_libraries(listener ${catkin_LIBRARIES})
add_dependencies(listener beginner_tutorials_generate_messages_cpp)

## For testing purposes

if(CATKIN_ENABLE_TESTING)
  find_package(rostest REQUIRED)

  add_rostest_gtest(
  unitTest 
  test/test.launch 
  test/main.cpp 
  test/test.cpp
  )
  target_link_libraries(
  unitTest
  ${catkin_LIBRARIES}
  )
  add_dependencies(
  talker
  unitTest
  ${catkin_EXPORTED_TARGETS}
  )
endif()





