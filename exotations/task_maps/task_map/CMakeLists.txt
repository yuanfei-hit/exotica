cmake_minimum_required(VERSION 2.8.3)
project(task_map)

add_definitions(-DEXOTICA_DEBUG_MODE)

find_package(catkin REQUIRED COMPONENTS
  exotica
  exotica_python
  geometry_msgs
)

AddInitializer(
  CoM
  IMesh
  EffPosition
  EffOrientation
  EffFrame
  JointLimit
  Distance
  Identity
  SphereCollision
  Sphere
  CollisionCheck
)
GenInitializers()

catkin_package(
  INCLUDE_DIRS include
  LIBRARIES ${PROJECT_NAME}
  CATKIN_DEPENDS exotica geometry_msgs
)

include_directories(
  include
  ${catkin_INCLUDE_DIRS}
)

set(SOURCES
    src/CoM.cpp
    src/IMesh.cpp
    src/EffPosition.cpp
    src/EffOrientation.cpp
    src/EffFrame.cpp
    src/JointLimit.cpp
    src/Distance.cpp
    src/Identity.cpp
    src/SphereCollision.cpp
    src/CollisionCheck.cpp)

add_library(${PROJECT_NAME} ${SOURCES})


target_link_libraries(${PROJECT_NAME} ${catkin_LIBRARIES})

add_dependencies(${PROJECT_NAME} ${PROJECT_NAME}_initializers)

pybind_add_module(${PROJECT_NAME}_py MODULE src/task_map_py.cpp ${SOURCES})
install(TARGETS ${PROJECT_NAME}_py LIBRARY DESTINATION ${CATKIN_GLOBAL_PYTHON_DESTINATION})

## Install
install(TARGETS ${PROJECT_NAME}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION})
install(DIRECTORY include/ DESTINATION include)
install(FILES exotica_plugins.xml DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION})

if(CATKIN_ENABLE_TESTING)
    add_executable(test_maps tests/test_maps.cpp)
    target_link_libraries(test_maps ${PROJECT_NAME} ${catkin_LIBRARIES})
    install(TARGETS test_maps RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION})
endif(CATKIN_ENABLE_TESTING)
