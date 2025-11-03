cmake_minimum_required(VERSION 3.13)

project(grbda VERSION 2.1.0)

# Default to C++17
set(CMAKE_CXX_STANDARD 17)
set(CXX_STANDARD 17)

if(NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE RelWithDebInfo)
endif()

add_compile_options(-Wall -Wextra -Wpedantic -Wconversion -Wdouble-promotion -Wfloat-equal -Wshadow -frecord-gcc-switches)

find_package(ament_cmake REQUIRED)
find_package(Eigen3 REQUIRED)
find_package(casadi REQUIRED)
find_package(urdfdom REQUIRED)
find_package(urdfdom_headers REQUIRED)

file(GLOB_RECURSE GRBDA_SRCS "src/*.cpp")

add_library(${PROJECT_NAME} SHARED ${GRBDA_SRCS})

target_include_directories(
  ${PROJECT_NAME}
  PUBLIC
  "$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>"
  "$<INSTALL_INTERFACE:include>"
)

target_link_libraries(
  ${PROJECT_NAME}
  PUBLIC
  casadi::casadi
  urdfdom::urdfdom_model
  urdfdom::urdf_parser
  urdfdom_headers::urdfdom_headers
  Eigen3::Eigen
)

install(
  TARGETS ${PROJECT_NAME}
  EXPORT export_{PROJECT_NAME}
  LIBRARY DESTINATION lib
  ARCHIVE DESTINATION lib
  RUNTIME DESTINATION bin
  INCLUDES DESTINATION include
)

install(
  DIRECTORY include/
  DESTINATION include
)

ament_export_targets(export_${PROJECT_NAME} HAS_LIBRARY_TARGET)
ament_export_dependencies(
  casadi
  urdfdom
  urdfdom_headers
)