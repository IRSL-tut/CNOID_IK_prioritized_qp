cmake_minimum_required(VERSION 3.8)
project(prioritized_qp_osqp)

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

find_package(ament_cmake REQUIRED)
find_package(prioritized_qp_base REQUIRED)
find_package(osqp-cpp REQUIRED)
find_package(PkgConfig REQUIRED)
pkg_check_modules(EIGEN eigen3)

###########
## Build ##
###########

## Declare a C++ library
add_library(${PROJECT_NAME} SHARED
  src/prioritized_qp_osqp.cpp
)
add_library(${PROJECT_NAME}::${PROJECT_NAME} ALIAS ${PROJECT_NAME})
target_compile_features(${PROJECT_NAME} PUBLIC c_std_99 cxx_std_17)  # Require C99 and C++17
target_include_directories(${PROJECT_NAME} PUBLIC
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
  $<INSTALL_INTERFACE:include>
  ${prioritized_qp_base_INCLUDE_DIRS}
  ${osqp-cpp_INCLUDE_DIRS}
  ${EIGEN_INCLUDE_DIRS}
)
target_link_libraries(${PROJECT_NAME} PUBLIC
  ${prioritized_qp_base_LIBRARIES}
  ${osqp-cpp_LIBRARIES}
  ${EIGEN_LIBRARIES}
)

#############
## Install ##
#############

install(
  DIRECTORY include/
  DESTINATION include/
)
install(
  TARGETS ${PROJECT_NAME}
  EXPORT export_${PROJECT_NAME}
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
  RUNTIME DESTINATION bin
)
ament_export_include_directories(
  "include/${PROJECT_NAME}"
)
ament_export_libraries(
  ${PROJECT_NAME}
)
ament_export_targets(
  export_${PROJECT_NAME}
)
ament_package()
