cmake_minimum_required(VERSION 3.17)
project(rtsp_streamer)

set(CMAKE_CXX_STANDARD 14) #setting C++ 14 standard
set(DEFAULT_GST_DEBUG "*:6" CACHE STRING "Default value for GST_DEBUG")

add_definitions("-DDEFAULT_GST_DEBUG=\"${DEFAULT_GST_DEBUG}\"")
add_executable(rtsp_streamer ./Testing/test.cpp)

find_package(OpenCV QUIET)
find_package(PkgConfig) #finding pkg-config is a helper tool

#using pkg-config to getting Gstreamer
pkg_check_modules(GSTREAMER REQUIRED gstreamer-rtsp-server-1.0)

if (OpenCV_FOUND)
    message("\n${PROJECT_NAME}...")
    message(STATUS "Found OpenCV installed in the system, will use it to display image in AdvancedSensing APIs")
    message(STATUS " - Includes: ${OpenCV_INCLUDE_DIRS}")
    message(STATUS " - Libraries: ${OpenCV_LIBRARIES}")
    add_definitions(-DOPEN_CV_INSTALLED)
else ()
    message(STATUS "Did not find OpenCV in the system, image data is inside RecvContainer as raw data")
endif ()


#including GStreamer header files directory
include_directories(
        ${GLIB_INCLUDE_DIRS}
        ${GSTREAMER_INCLUDE_DIRS}
        ${GSTREAMER_RTSP_INCLUDE_DIRS}
        ${GSTREAMER_BASE_INCLUDE_DIRS}
)


#linking GStreamer library directory
link_directories(
        ${GLIB_LIBRARY_DIRS}
        ${GSTREAMER_LIBRARY_DIRS}
        ${GSTREAMER_RTSP_INCLUDE_DIRS}
        ${GSTREAMER_BASE_INCLUDE_DIRS}
)

if (OpenCV_FOUND)
    target_link_libraries(${PROJECT_NAME}
            ${OpenCV_LIBS}
            )
endif ()


#linking Gstreamer library with target executable
target_link_libraries(${PROJECT_NAME}
        ${GSTREAMER_LIBRARIES}
        )