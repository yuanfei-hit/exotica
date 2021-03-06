cmake_minimum_required(VERSION 2.8.3)
project(collision_scene_fcl_latest)

add_compile_options(-std=c++11)


find_package(catkin REQUIRED COMPONENTS
  exotica
)

set(exotica_INCLUDES ${catkin_INCLUDE_DIRS})

find_package(catkin REQUIRED COMPONENTS
  fcl_catkin
  geometric_shapes
)

catkin_package(
  INCLUDE_DIRS include
  LIBRARIES collision_scene_fcl_latest
  CATKIN_DEPENDS fcl_catkin geometric_shapes
)

include_directories(
  include
  ${catkin_INCLUDE_DIRS}
  ${exotica_INCLUDES}
)

add_library(${PROJECT_NAME}
  src/CollisionSceneFCLLatest.cpp
)

target_link_libraries(${PROJECT_NAME}
  ${catkin_LIBRARIES}
)

install(TARGETS ${PROJECT_NAME}
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)
install(DIRECTORY include/ DESTINATION include)
install(FILES exotica_plugins.xml DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION})
