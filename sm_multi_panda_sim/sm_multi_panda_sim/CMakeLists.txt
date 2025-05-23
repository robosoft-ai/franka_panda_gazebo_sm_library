cmake_minimum_required(VERSION 3.5)
project(sm_multi_panda_sim)

# Default to C++17
if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 17)
endif()

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

# find dependencies
find_package(ament_cmake REQUIRED)
find_package(ros_timer_client REQUIRED)
find_package(smacc2 REQUIRED)
find_package(Boost COMPONENTS thread REQUIRED)
find_package(moveit2z_client REQUIRED)

find_package(sr_all_events_go REQUIRED)
find_package(ros_publisher_client REQUIRED)
find_package(keyboard_client REQUIRED)

set(THIS_PACKAGE_INCLUDE_DEPENDS
  ros_timer_client
  smacc2
  moveit2z_client
  sr_all_events_go
  ros_publisher_client
  keyboard_client
)

include_directories(
  include
  ${smacc2_INCLUDE_DIRS}
  ${moveit2z_client_INCLUDE_DIRS}
)

add_executable(${PROJECT_NAME}_node
  src/${PROJECT_NAME}/${PROJECT_NAME}_node.cpp
)

target_link_libraries(${PROJECT_NAME}_node
  ${smacc2_LIBRARIES}
  ${ros_timer_client_LIBRARIES}
  ${Boost_LIBRARIES}
  ${moveit2z_client_LIBRARIES}
)

ament_target_dependencies(${PROJECT_NAME}_node ${THIS_PACKAGE_INCLUDE_DEPENDS})

install(
  DIRECTORY include/
  DESTINATION include
)

install(DIRECTORY
  # config
  launch
  # rviz
  # urdf
  # srdf
  DESTINATION share/${PROJECT_NAME}
)

install(TARGETS
  ${PROJECT_NAME}_node
  DESTINATION lib/${PROJECT_NAME}
)

ament_package()
