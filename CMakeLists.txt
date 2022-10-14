cmake_minimum_required(VERSION 3.21)
project(flann_vendor)

find_package(ament_cmake REQUIRED)

include(ExternalProject)

ExternalProject_Add(flann_src
  GIT_REPOSITORY https://github.91chi.fun//https://github.com/flann-lib/flann.git
  GIT_TAG 1.9.1
  # UPDATE_COMMAND "cd  ../flann_src && git submodule update --recursive --init"
  CONFIGURE_COMMAND cd ../flann_src-build && cmake ../flann_src -DCMAKE_INSTALL_PREFIX=${CMAKE_INSTALL_PREFIX}/ -G Ninja -DCMAKE_BUILD_TYPE=Release -DBUILD_visualization=OFF
  BUILD_COMMAND cd ../flann_src-build && cmake --build . --config Release
  INSTALL_COMMAND cd ../flann_src-build && cmake --install .
)

add_library(${PROJECT_NAME} src/dependency_tracker.cc)

install(TARGETS ${PROJECT_NAME} EXPORT export_${PROJECT_NAME}
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
  RUNTIME DESTINATION bin
)

# install(DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/install/
#   DESTINATION ${CMAKE_INSTALL_PREFIX}/
# )

ament_package(CONFIG_EXTRAS "cmake/flann-extras.cmake.in" "cmake/FindFLANN.cmake.in")