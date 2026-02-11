cmake_minimum_required(VERSION 3.8)
project(hoverboard_driver)

if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 17)
endif()

# 1. Find ROS 2 packages instead of catkin
find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(std_msgs REQUIRED)
find_package(hardware_interface REQUIRED)
find_package(controller_manager REQUIRED)
find_package(control_toolbox REQUIRED)
# Note: dynamic_reconfigure is replaced by ROS 2 parameters

# 2. Define the executable
add_executable(${PROJECT_NAME}_node 
  src/hoverboard.cpp 
  src/pid.cpp
  src/main.cpp
)

# 3. Add Include directories
target_include_directories(${PROJECT_NAME}_node PUBLIC
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
  $<INSTALL_INTERFACE:include>
)

# 4. Link dependencies (Ament handles Boost & System libs automatically here)
ament_target_dependencies(${PROJECT_NAME}_node
  rclcpp
  std_msgs
  hardware_interface
  controller_manager
  control_toolbox
)

# 5. Install the executable
install(TARGETS ${PROJECT_NAME}_node
  DESTINATION lib/${PROJECT_NAME})

# 6. REQUIRED: Must be at the very end
ament_package()
