cmake_minimum_required(VERSION 3.8)
project(prioritized_qp_osqp)

add_compile_options(-std=c++14 -Wno-unused-result)
set(CMAKE_BUILD_TYPE Release)

#find_package(catkin REQUIRED COMPONENTS
#  prioritized_qp_base
#  osqp-cpp)
find_package(ament_cmake REQUIRED)
find_package(prioritized_qp_base REQUIRED)
find_package(osqp-cpp REQUIRED)
find_package(Eigen3 REQUIRED)

#catkin_package(
#  INCLUDE_DIRS include
#  LIBRARIES prioritized_qp_osqp
#  CATKIN_DEPENDS prioritized_qp_base osqp-cpp
#  #DEPENDS OsqpEigen
#)

###########
## Build ##
###########

## Declare a C++ library
add_library(${PROJECT_NAME} SHARED
  src/prioritized_qp_osqp.cpp
  )

target_link_libraries(${PROJECT_NAME} PUBLIC
  ${prioritized_qp_base_LIBRARIES}
  ${osqp-cpp_LIBRARIES}
  Eigen3::Eigen
  )

target_include_directories(${PROJECT_NAME} PUBLIC
  include
  ${prioritized_qp_base_INCLUDE_DIRS}
  ${osqp-cpp_INCLUDE_DIRS}
  )

#############
## Install ##
#############

## Mark libraries for installation
## See http://docs.ros.org/melodic/api/catkin/html/howto/format1/building_libraries.html
install(TARGETS ${PROJECT_NAME}
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
  RUNTIME DESTINATION bin
  )

## Mark cpp header files for installation
install(DIRECTORY include/${PROJECT_NAME}/
  DESTINATION include/${PROJECT_NAME}
  FILES_MATCHING PATTERN "*.h"
  PATTERN ".svn" EXCLUDE
  )

#############
## Testing ##
#############

## Add gtest based cpp test target and link libraries
# catkin_add_gtest(${PROJECT_NAME}-test test/test_prioritized_qp.cpp)
# if(TARGET ${PROJECT_NAME}-test)
#   target_link_libraries(${PROJECT_NAME}-test ${PROJECT_NAME})
# endif()

## Add folders to be run by python nosetests
# catkin_add_nosetests(test)

ament_export_libraries(prioritized_qp_osqp)
ament_export_dependencies(prioritized_qp_base osqp-cpp)
ament_export_include_directories(include)

ament_package()
