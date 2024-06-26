cmake_minimum_required(VERSION 3.5)
project(sheet_generation)

# Set the build type if it's not already set
# set(CMAKE_BUILD_TYPE Debug)
set(CMAKE_BUILD_TYPE Release)
set(CMAKE_CXX_STANDARD 17)

# Add pybind11 submodule
add_subdirectory(pybind11)

# Add YAML-CPP submodule
add_subdirectory(yaml-cpp)

find_package(Eigen3 3.3 REQUIRED NO_MODULE)
find_package(Threads REQUIRED)  # Find the threading library
find_package(LibArchive REQUIRED)
find_package(TBB REQUIRED)

pybind11_add_module(sheet_generation solver.cpp)
pybind11_add_module(pointcloud_processing pointcloud_processing.cpp)
# Link YAML-CPP library
target_link_libraries(sheet_generation PRIVATE yaml-cpp Eigen3::Eigen ${CMAKE_THREAD_LIBS_INIT})
target_link_libraries(pointcloud_processing PRIVATE ${CMAKE_THREAD_LIBS_INIT} ${LibArchive_LIBRARIES} TBB::tbb)

