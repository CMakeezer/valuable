cmake_minimum_required(VERSION 3.0)
project(valuable VERSION 2.2.0 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 14)

option(BUILD_TESTS "Build tests" OFF)

add_library(valuable INTERFACE)
add_library(valuable::valuable ALIAS valuable)

target_include_directories(valuable
  INTERFACE
    $<BUILD_INTERFACE:${CMAKE_CURRENT_LIST_DIR}/valuable>)

# Create an test target

if(BUILD_TESTS)
    find_package(GTest)
    enable_testing()
    add_executable(valuable_test ${CMAKE_CURRENT_LIST_DIR}/valuable/tests/main.cpp)
    add_test(valuable_test valuable_test)
    target_link_libraries(valuable_test PRIVATE GTest::GTest GTest::Main valuable)
    message(iNFO "CMAKE_CURRENT_LIST_DIR = ${CMAKE_CURRENT_LIST_DIR}")
    target_include_directories(valuable_test PRIVATE ${CMAKE_CURRENT_LIST_DIR}/valuable/include)
endif()


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