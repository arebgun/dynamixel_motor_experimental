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

#set the default path for built executables to the "bin" directory
set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)
#set the default path for built libraries to the "lib" directory
set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)

#uncomment if you have defined messages
#rosbuild_genmsg()
#uncomment if you have defined services
#rosbuild_gensrv()

#common commands for building c++ executables and libraries
#rosbuild_add_library(${PROJECT_NAME} src/example.cpp)
#target_link_libraries(${PROJECT_NAME} another_library)
#rosbuild_add_boost_directories()
#rosbuild_link_boost(${PROJECT_NAME} thread)
#rosbuild_add_executable(example examples/example.cpp)
#target_link_libraries(example ${PROJECT_NAME})

include(ExternalProject)
ExternalProject_Add(
    gearbox
    SVN_REPOSITORY http://gearbox.svn.sf.net/svnroot/gearbox/gearbox/tags/RELEASE_10.11
    PATCH_COMMAND patch -p1 < ${PROJECT_SOURCE_DIR}/gearbox_ros.patch
    CMAKE_ARGS -D CMAKE_BUILD_TYPE=RELEASE
        -D GEARBOX_INSTALL=${PROJECT_SOURCE_DIR}/gearbox
        -D boostPythonLib='boost_python'
        -D ENABLE_LIB_FLEXIPORT=ON
        -D ENABLE_LIB_BASICEXAMPLE=OFF
        -D ENABLE_LIB_GBXADVANCEDEXAMPLE=OFF
        -D ENABLE_LIB_GBXGARMINACFR=OFF
        -D ENABLE_LIB_GBXICEUTILACFR=OFF
        -D ENABLE_LIB_GBXNOVATELACFR=OFF
        -D ENABLE_LIB_GBXSERIALACFR=OFF
        -D ENABLE_LIB_GBXSICKACFR=OFF
        -D ENABLE_LIB_GBXSMARTBATTERYACFR=OFF
        -D ENABLE_LIB_GBXUTILACFR=OFF
        -D ENABLE_LIB_HOKUYO_AIST=OFF
        -D FLEXIPORT_BUILD_BINDINGS=OFF
        -D FLEXIPORT_INCLUDE_LOGGING=ON
        -D GBX_BUILD_TESTS=OFF
        -D HOKUYO_AIST_BUILD_BINDINGS=OFF
    UPDATE_COMMAND ""
    BUILD_COMMAND make
    INSTALL_COMMAND make install
)