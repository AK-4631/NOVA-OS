# NOVA OS root build.
#
# Host developer entry point: configures and runs the system-service unit
# tests that run on any desktop (Windows/mingw or Linux). The Qt Quick
# shell builds here too when Qt6 is available; on the target it is built
# inside the Buildroot image instead (buildroot/external/package/nova-shell).
cmake_minimum_required(VERSION 3.20)
project(nova-os LANGUAGES C CXX)

include(CTest)

add_subdirectory(system)

# Host-side build of the NOVA shell when Qt6 Quick is installed.
find_package(Qt6 COMPONENTS Core Quick QUIET)
if (Qt6_FOUND)
    message(STATUS "Qt6 found: configuring nova-shell host build")
    add_subdirectory(ui/shell)
else()
    message(STATUS "Qt6 not found: skipping nova-shell host build (built on target via Buildroot)")
endif()
