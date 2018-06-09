cmake_minimum_required(VERSION 3.0)
project(valuable VERSION 2.2.0 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 14)
find_package(GTest REQUIRED)

add_library(valuable INTERFACE)
add_library(valuable::valuable ALIAS valuable)

target_include_directories(valuable
  INTERFACE
    $<BUILD_INTERFACE:${CMAKE_CURRENT_LIST_DIR}/valuable>)

# Create an test target

enable_testing()
add_executable(valuable_test ${CMAKE_CURRENT_LIST_DIR}/valuable/tests/main.cpp)
add_test(valuable_test valuable_test)
target_link_libraries(valuable_test PRIVATE GTest::GTest GTest::Main valuable)


# Create an install target
install(TARGETS valuable
        EXPORT valuable-config
        INCLUDES DESTINATION include
        )

install(EXPORT valuable-config
        FILE valuable-config.cmake
        NAMESPACE valuable::
        DESTINATION share/valuable/cmake)

install(DIRECTORY ${PROJECT_SOURCE_DIR}/valuable/include/
        DESTINATION include/valuable
        FILES_MATCHING PATTERN "*.hpp")

install(FILES LICENSE DESTINATION . RENAME valuable-LICENSE.txt)
install(FILES README.md DESTINATION . RENAME valuable-Readme.md)