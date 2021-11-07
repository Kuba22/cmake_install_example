cmake_minimum_required(VERSION 3.0.2)
project(mytestlib)

# find_package(OpenCV REQUIRED)

add_library(${PROJECT_NAME} SHARED mytestlib.cpp)
target_include_directories(${PROJECT_NAME} PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>  
    $<INSTALL_INTERFACE:${CMAKE_INSTALL_PREFIX}/include> )
# target_link_libraries(${PROJECT_NAME} ${OpenCV_LIBRARIES})

add_executable(example main.cpp)
target_link_libraries(example mytestlib)

install(
    TARGETS ${PROJECT_NAME}
    EXPORT mytestlibTargets
    RUNTIME DESTINATION bin
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
    PUBLIC_HEADER DESTINATION ${CMAKE_INSTALL_PREFIX}/include
)

install(EXPORT mytestlibTargets 
    FILE mytestlibTargets.cmake
    DESTINATION lib/cmake/mytestlib)

include(CMakePackageConfigHelpers)

set(INCLUDE_INSTALL_DIR ${CMAKE_INSTALL_PREFIX}/include/mytestlib)
configure_package_config_file(
    ${CMAKE_CURRENT_SOURCE_DIR}/mytestlibConfig.cmake.in
    ${CMAKE_CURRENT_BINARY_DIR}/mytestlibConfig.cmake
    INSTALL_DESTINATION lib/cmake/mytestlib
    PATH_VARS INCLUDE_INSTALL_DIR
    NO_SET_AND_CHECK_MACRO
    NO_CHECK_REQUIRED_COMPONENTS_MACRO
)

write_basic_package_version_file(
  ${CMAKE_CURRENT_BINARY_DIR}/mytestlibConfigVersion.cmake
  VERSION ${mytestlib_VERSION_MAJOR}.${mytestlib_VERSION_MINOR}
  COMPATIBILITY AnyNewerVersion
)

install(FILES
    ${CMAKE_CURRENT_BINARY_DIR}/mytestlibConfig.cmake
    ${CMAKE_CURRENT_BINARY_DIR}/mytestlibConfigVersion.cmake
    DESTINATION lib/cmake/mytestlib
)

export(EXPORT mytestlibTargets
    FILE ${CMAKE_CURRENT_BINARY_DIR}/mytestlibTargets.cmake
)