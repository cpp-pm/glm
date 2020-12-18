cmake_minimum_required(VERSION 3.2 FATAL_ERROR)
cmake_policy(VERSION 3.2)

set(GLM_VERSION "0.9.9")
project(glm VERSION ${GLM_VERSION} LANGUAGES CXX)

include(GNUInstallDirs)
include(CMakePackageConfigHelpers)

add_library(glm INTERFACE)
target_include_directories(
  glm INTERFACE $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
                $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>)

set(PROJECT_CONFIG "${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}Config.cmake")
set(VERSION_CONFIG "${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}ConfigVersion.cmake")

write_basic_package_version_file(
  ${VERSION_CONFIG}
  VERSION ${GLM_VERSION}
  COMPATIBILITY AnyNewerVersion)

configure_package_config_file(
  "./cmake/glmConfig.cmake.in" ${PROJECT_CONFIG}
  INSTALL_DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/${PROJECT_NAME})

install(TARGETS glm EXPORT ${PROJECT_NAME}Targets)

# Headers
install(
  DIRECTORY ${CMAKE_SOURCE_DIR}/glm
  DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
  FILES_MATCHING
  PATTERN "*.hpp*"
  PATTERN "*.inl*"
  PATTERN "*.h*")

# Config
install(FILES ${PROJECT_CONFIG} ${VERSION_CONFIG}
        DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/glm)
install(EXPORT glmTargets DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/glm)

# Install pkg-config file
configure_file("./cmake/glm.pc.in" "glm.pc" @ONLY)
install(FILES "${CMAKE_CURRENT_BINARY_DIR}/glm.pc" DESTINATION "${CMAKE_INSTALL_LIBDIR}/pkgconfig")

