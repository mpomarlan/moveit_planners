cmake_minimum_required(VERSION 2.8.3)
project(moveit_planners_ompl)

if(NOT CMAKE_CONFIGURATION_TYPES AND NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE Release)
endif()

find_package(Boost REQUIRED system filesystem date_time thread serialization)
find_package(catkin REQUIRED COMPONENTS
  moveit_core 
  moveit_ros_planning
  roscpp
  rosconsole
  pluginlib
  tf
  dynamic_reconfigure
  eigen_conversions
  )

find_package(OMPL REQUIRED)

generate_dynamic_reconfigure_options("ompl_interface/cfg/OMPLDynamicReconfigure.cfg")

catkin_package(
  LIBRARIES
    moveit_ompl_interface
    ${OMPL_LIBRARIES}
  INCLUDE_DIRS
    ompl_interface/include
    ${OMPL_INCLUDE_DIRS}
  CATKIN_DEPENDS
    moveit_core
)

include_directories(SYSTEM
                    ${Boost_INCLUDE_DIRS})

include_directories(ompl_interface/include
                    ${OMPL_INCLUDE_DIRS}
                    ${catkin_INCLUDE_DIRS})

link_directories(${OMPL_LIBRARY_DIRS})
link_directories(${catkin_LIBRARY_DIRS})
link_directories(${Boost_LIBRARY_DIRS})

add_subdirectory(ompl_interface)

install(FILES ompl_interface_plugin_description.xml
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION})
