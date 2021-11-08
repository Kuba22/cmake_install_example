cmake_minimum_required(VERSION 3.0.2)
project(mytestlib VERSION 0.1)

# find_package(OpenCV REQUIRED)

add_library(${PROJECT_NAME} SHARED mytestlib.cpp)
target_include_directories(${PROJECT_NAME} PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include/mytestlib>  
    $<INSTALL_INTERFACE:${CMAKE_INSTALL_PREFIX}/include/mytestlib> )
# target_link_libraries(${PROJECT_NAME} ${OpenCV_LIBRARIES})

add_executable(example main.cpp)
target_link_libraries(example mytestlib)

install(
    TARGETS ${PROJECT_NAME}
    EXPORT mytestlibTargets
    RUNTIME DESTINATION ${CMAKE_INSTALL_PREFIX}/bin
    LIBRARY DESTINATION ${CMAKE_INSTALL_PREFIX}/lib
    ARCHIVE DESTINATION ${CMAKE_INSTALL_PREFIX}/lib
)

install(EXPORT mytestlibTargets 
    DESTINATION ${CMAKE_INSTALL_PREFIX}/lib/cmake/mytestlib
    NAMESPACE mytestlib::)

install(DIRECTORY ${CMAKE_CURRENT_LIST_DIR}/include/mytestlib DESTINATION ${CMAKE_INSTALL_PREFIX}/include)

include(CMakePackageConfigHelpers)

configure_package_config_file(
    ${PROJECT_SOURCE_DIR}/mytestlibConfig.cmake.in
    ${PROJECT_BINARY_DIR}/mytestlibConfig.cmake
    INSTALL_DESTINATION ${CMAKE_INSTALL_PREFIX}/lib/cmake/mytestlib
)

write_basic_package_version_file(
  mytestlibConfigVersion.cmake
  VERSION ${PROJECT_VERSION}
  COMPATIBILITY AnyNewerVersion
)

install(FILES
    ${CMAKE_CURRENT_BINARY_DIR}/mytestlibConfig.cmake
    ${CMAKE_CURRENT_BINARY_DIR}/mytestlibConfigVersion.cmake
    DESTINATION ${CMAKE_INSTALL_PREFIX}/lib/cmake/mytestlib
)

export(EXPORT mytestlibTargets
    FILE ${CMAKE_CURRENT_BINARY_DIR}/mytestlibTargets.cmake
)