cmake_minimum_required(VERSION 3.0.5)
project(l1ang_planner)

## Find catkin macros and libraries
find_package(catkin REQUIRED COMPONENTS
	roscpp
	sensor_msgs
	nav_core
	pluginlib
	costmap_2d
	tf2
	tf2_ros
	tf2_geometry_msgs
	nav_msgs
	geometry_msgs
	)

###################################
## catkin specific configuration ##
###################################
catkin_package(INCLUDE_DIRS include
	CATKIN_DEPENDS
	roscpp sensor_msgs
	nav_core pluginlib costmap_2d
	tf2 tf2_ros tf2_geometry_msgs nav_msgs geometry_msgs
	)

#---------------------------------------------------------------------------------------
# Set default build to release
#---------------------------------------------------------------------------------------
if(NOT CMAKE_BUILD_TYPE)
	set(CMAKE_BUILD_TYPE "Release" CACHE STRING "Choose Release or Debug" FORCE)
endif()

#---------------------------------------------------------------------------------------
# Compiler config
#---------------------------------------------------------------------------------------
set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

#Include directories
include_directories(include ${catkin_INCLUDE_DIRS})

#---------------------------------------------------------------------------------------
# move_base 局部规划插件 (L1angLocalPlanner，替代 dwa_local_planner)
#---------------------------------------------------------------------------------------
add_library(l1ang_local_planner
	src/l1ang_local_planner.cpp
	)

target_link_libraries(l1ang_local_planner
	${catkin_LIBRARIES}
	)

#---------------------------------------------------------------------------------------
# Install
#---------------------------------------------------------------------------------------

install(TARGETS l1ang_local_planner
	ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
	LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
	RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
	)

install(FILES plugin.xml
	DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
	)

install(DIRECTORY include/${PROJECT_NAME}/
	DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
	)

install(DIRECTORY config/
	DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/config
	)

install(DIRECTORY launch/
	DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/launch
	)
