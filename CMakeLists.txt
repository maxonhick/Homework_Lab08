cmake_minimum_required(VERSION 3.9)

include("cmake/HunterGate.cmake")
HunterGate(
    URL "https://github.com/cpp-pm/hunter/archive/v0.23.251.tar.gz"
    SHA1 "5659b15dc0884d4b03dbd95710e6a1fa0fc3258d"
)

project(print)

set(PRINT_VERSION_STRING "v\${PRINT_VERSION}")

hunter_add_package(GTest)
find_package(GTest CONFIG REQUIRED)

add_library(print STATIC src/print.cpp)

add_executable(demo demo/main.cpp)
target_link_libraries(demo print)

install(TARGETS print DESTINATION lib)
install(TARGETS demo DESTINATION bin)
