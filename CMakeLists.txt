cmake_minimum_required(VERSION 2.8.3)
project(yaml_cpp_catkin)

find_package(catkin_simple REQUIRED)
catkin_simple(ALL_DEPS_REQUIRED)

include(ExternalProject)

file(MAKE_DIRECTORY ${CATKIN_DEVEL_PREFIX}/include)

ExternalProject_Add(yaml_cpp_src
  URL https://github.com/jbeder/yaml-cpp/archive/yaml-cpp-0.5.3.tar.gz
  UPDATE_COMMAND ""
  CMAKE_ARGS -DCMAKE_INSTALL_PREFIX:PATH=${CATKIN_DEVEL_PREFIX} -DBUILD_SHARED_LIBS=ON
  BUILD_COMMAND ${CMAKE_COMMAND} --build ./ --target install
  INSTALL_COMMAND ""
)
ExternalProject_CatkinInstall(yaml_cpp_src)

cs_add_library(${PROJECT_NAME} src/dependency_tracker.cc)
add_dependencies(${PROJECT_NAME} yaml_cpp_src)
target_link_libraries(${PROJECT_NAME} ${CATKIN_DEVEL_PREFIX}/lib/libyaml-cpp${CMAKE_SHARED_LIBRARY_SUFFIX})

cs_install()

cs_export(INCLUDE_DIRS ${CATKIN_DEVEL_PREFIX}/include
  CFG_EXTRAS yaml-cpp-extras.cmake)
