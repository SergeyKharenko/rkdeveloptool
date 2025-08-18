cmake_minimum_required(VERSION 3.20)
project(rkdeveloptool VERSION 1.32
        DESCRIPTION "Rockchip rkdeveloptool"
        LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)
set(PROJECT_BUG_REPORT_ADDRESS "Eddie Cai <eddie.cai.linux@gmail.com>")

find_package(PkgConfig REQUIRED)

pkg_check_modules(LIBUSB1 REQUIRED libusb-1.0)

add_compile_options(
        -Wall
        -Werror
        -Wextra
        -Wreturn-type
        -fno-strict-aliasing
)

add_library(rkutils STATIC
        crc.cpp
        RKBoot.cpp
        RKComm.cpp
        RKDevice.cpp
        RKImage.cpp
        RKLog.cpp
        RKScan.cpp
)

add_executable(rkdeveloptool main.cpp)

add_dependencies(rkdeveloptool rkutils)

target_compile_definitions(rkutils
        PRIVATE
        _FILE_OFFSET_BITS=64
        _LARGE_FILE
)
target_include_directories(rkutils PRIVATE ${LIBUSB1_INCLUDE_DIRS})
target_link_libraries(rkutils PRIVATE ${LIBUSB1_LIBRARIES})

target_compile_definitions(rkdeveloptool PRIVATE
        PACKAGE_VERSION=\"${PROJECT_VERSION}\")
target_include_directories(rkdeveloptool PRIVATE ${LIBUSB1_INCLUDE_DIRS})
target_link_libraries(rkdeveloptool PRIVATE
        rkutils
        ${LIBUSB1_LIBRARIES}
)

install(TARGETS rkdeveloptool RUNTIME DESTINATION bin)
