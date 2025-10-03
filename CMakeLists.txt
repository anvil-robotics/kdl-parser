cmake_minimum_required(VERSION 3.15)

project(kdl_parser)

find_package(Eigen3 REQUIRED)
find_package(orocos_kdl REQUIRED)
find_package(urdfdom_headers REQUIRED)

add_library(kdl_parser src/kdl_parser.cpp)
target_sources(
  kdl_parser PUBLIC
  FILE_SET HEADERS
  BASE_DIRS include
  FILES
    include/kdl_parser/kdl_parser.hpp
    include/kdl_parser/visibility_control.hpp
)
target_compile_features(kdl_parser PRIVATE cxx_std_17)
target_link_libraries(
  kdl_parser
  PUBLIC Eigen3::Eigen orocos-kdl urdfdom_headers::urdfdom_headers
)

if(WIN32)
  target_compile_definitions(kdl_parser PRIVATE "KDL_PARSER_BUILDING_DLL")
endif()

install(
  TARGETS kdl_parser
  EXPORT kdl_parser_targets
  LIBRARY DESTINATION lib
  FILE_SET HEADERS
)

install(
  EXPORT kdl_parser_targets
  FILE kdl_parser-targets.cmake
  NAMESPACE kdl_parser::
  DESTINATION lib/cmake/kdl_parser
)

install(
  FILES cmake/kdl_parser-config.cmake
  DESTINATION lib/cmake/kdl_parser
)
