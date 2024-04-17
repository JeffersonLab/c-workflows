cmake_minimum_required(VERSION 3.23)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

file (READ ${CMAKE_SOURCE_DIR}/VERSION VERSION)

string(STRIP ${VERSION} VERSION)

message("PROJECT_VERSION = ${VERSION}")

find_package(fmt CONFIG REQUIRED)

project(Hello LANGUAGES CXX VERSION ${VERSION})

set (SOURCE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/src)
set (INCLUDE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/include)

set(LIB_HEADER_FILES
    ${INCLUDE_DIR}/hello.h
)

set(LIB_SOURCE_FILES
    ${SOURCE_DIR}/hello.cpp
)

set(APP_SOURCE_FILES
    ${SOURCE_DIR}/main.cpp
)

add_library(hello ${LIB_SOURCE_FILES})
target_include_directories(hello PUBLIC "${INCLUDE_DIR}")
target_link_libraries(hello PRIVATE fmt::fmt)

add_executable(Hi ${APP_SOURCE_FILES})
target_link_libraries(Hi PRIVATE hello)

# Test Config
enable_testing()

add_executable(cpp_test tests/test.cpp)
target_link_libraries(cpp_test PRIVATE hello)

add_test(
  NAME cpp_test
  COMMAND $<TARGET_FILE:cpp_test>
)