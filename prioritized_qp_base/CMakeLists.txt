cmake_minimum_required(VERSION 3.8)
project(prioritized_qp_base)

set(CMAKE_CXX_STANDARD 14)
set(CMAKE_BUILD_TYPE Release)

#find_package(catkin REQUIRED)
find_package(ament_cmake REQUIRED)

#find_package(PkgConfig REQUIRED)
#pkg_check_modules(EIGEN eigen3)
find_package(Eigen3 REQUIRED)

#catkin_package(
#  INCLUDE_DIRS include
#  LIBRARIES prioritized_qp_base
#  DEPENDS EIGEN
#)

###########
## Build ##
###########

## Declare a C++ library
add_library(${PROJECT_NAME} SHARED
  src/PrioritizedQPBaseSolver.cpp
  )

ament_target_dependencies(${PROJECT_NAME} Eigen3)
target_link_libraries(${PROJECT_NAME} PUBLIC
  Eigen3::Eigen)

target_include_directories(${PROJECT_NAME} PUBLIC
  include
  ${EIGEN_INCLUDE_DIRS}
)

#############
## Install ##
#############

install(TARGETS ${PROJECT_NAME}
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
  RUNTIME DESTINATION bin
  )

install(DIRECTORY include/${PROJECT_NAME}/
  DESTINATION include/${PROJECT_NAME}
  FILES_MATCHING PATTERN "*.h"
  PATTERN ".svn" EXCLUDE
  )

ament_export_libraries(prioritized_qp_base)
ament_export_include_directories(include)

ament_package()
