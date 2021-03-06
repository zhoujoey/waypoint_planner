cmake_minimum_required(VERSION 2.8.3)
project(vastar_planner)
add_compile_options(-std=c++11)
find_package(catkin REQUIRED
  COMPONENTS
    roscpp
    tf
    nav_core
    costmap_2d
    dynamic_reconfigure
    pluginlib
    rospy
    std_msgs
  )

generate_dynamic_reconfigure_options(
  cfg/VastarPlanner.cfg
)

include_directories(
  include
  ${catkin_INCLUDE_DIRS}
  )
add_definitions(${EIGEN3_DEFINITIONS})

catkin_package(
  INCLUDE_DIRS include
  LIBRARIES vastar_planner
  CATKIN_DEPENDS
    roscpp
    pluginlib
    costmap_2d
    dynamic_reconfigure
    nav_core
    rospy
    std_msgs
)
#set(CMAKE_CXX_FLAGS "-std=c++0x ${CMAKE_CXX_FLAGS}")
add_library(vastar_planner src/vastar_planner.cpp src/vastar.cpp)
add_dependencies(vastar_planner ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})
target_link_libraries(vastar_planner
    ${catkin_LIBRARIES}
    )

install(TARGETS vastar_planner
       ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
       LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
       )

install(DIRECTORY include/${PROJECT_NAME}/
  DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
  PATTERN ".svn" EXCLUDE
)

install(FILES bgp_plugin.xml
    DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
)


