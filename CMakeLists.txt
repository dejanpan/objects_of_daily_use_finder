cmake_minimum_required(VERSION 2.4.6)
include($ENV{ROS_ROOT}/core/rosbuild/rosbuild.cmake)

# Set the build type.  Options are:
#  Coverage       : w/ debug symbols, w/o optimization, w/ code-coverage
#  Debug          : w/ debug symbols, w/o optimization
#  Release        : w/o debug symbols, w/ optimization
#  RelWithDebInfo : w/ debug symbols, w/ optimization
#  MinSizeRel     : w/o debug symbols, w/ optimization, stripped binaries
#set(ROS_BUILD_TYPE RelWithDebInfo)

rosbuild_init()
rosbuild_add_boost_directories()

#set the default path for built executables to the "bin" directory
set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)
#set the default path for built libraries to the "lib" directory
set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)

#uncomment if you have defined messages
#rosbuild_genmsg()
#uncomment if you have defined services
#rosbuild_gensrv()

#common commands for building c++ executables and libraries

find_package(Eigen REQUIRED)
include_directories(${Eigen_INCLUDE_DIRS})
find_package(Boost REQUIRED)
find_package(rospack REQUIRED)
message("rospack_LIBRARIES found: ${rospack_LIBRARIES}")
rosbuild_add_library(odu_finder src/odu_finder.cpp)
target_link_libraries(odu_finder ${rospack_LIBRARIES})

rosbuild_add_executable(odu_finder_node src/odu_finder_node.cpp)
target_link_libraries(odu_finder_node odu_finder ${rospack_LIBRARIES})

rosbuild_add_executable(multi_openCV_to_ros src/multi_openCV_to_ros.cpp)
rosbuild_link_boost(multi_openCV_to_ros filesystem system thread)
