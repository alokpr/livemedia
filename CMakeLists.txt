cmake_minimum_required(VERSION 3.10)
project(LiveMedia VERSION "2021.2.11" LANGUAGES CXX)

find_package(OpenSSL REQUIRED)

include(CMakePackageConfigHelpers)
include(ExternalProject)
include(GNUInstallDirs)

ExternalProject_Add(livemedia
  URL http://www.live555.com/liveMedia/public/live555-latest.tar.gz
  URL_MD5 9fa7c70480c24434a24e69684acacfb7
  BUILD_IN_SOURCE TRUE
  CONFIGURE_COMMAND ./genMakefiles linux
  BUILD_COMMAND $(MAKE) all
  INSTALL_COMMAND $(MAKE) DESTDIR=<INSTALL_DIR> PREFIX="" install
)

ExternalProject_Get_property(livemedia INSTALL_DIR)
set(INSTALL_BINDIR ${CMAKE_INSTALL_BINDIR})
set(INSTALL_DATADIR ${CMAKE_INSTALL_DATADIR}/${CMAKE_PROJECT_NAME})
set(INSTALL_INCLUDEDIR ${CMAKE_INSTALL_INCLUDEDIR})
set(INSTALL_LIBDIR ${CMAKE_INSTALL_LIBDIR})

install(DIRECTORY ${INSTALL_DIR}/bin/ DESTINATION ${INSTALL_BINDIR})
install(DIRECTORY ${INSTALL_DIR}/include/ DESTINATION ${INSTALL_INCLUDEDIR})
install(DIRECTORY ${INSTALL_DIR}/lib/ DESTINATION ${INSTALL_LIBDIR})

configure_package_config_file(LiveMediaConfig.cmake.in
  ${CMAKE_CURRENT_BINARY_DIR}/LiveMediaConfig.cmake
  INSTALL_DESTINATION ${INSTALL_DATADIR}
  PATH_VARS INSTALL_INCLUDEDIR INSTALL_LIBDIR
)

write_basic_package_version_file(
  ${CMAKE_CURRENT_BINARY_DIR}/LiveMediaConfigVersion.cmake
  COMPATIBILITY AnyNewerVersion
)

install(FILES
  ${CMAKE_CURRENT_BINARY_DIR}/LiveMediaConfig.cmake
  ${CMAKE_CURRENT_BINARY_DIR}/LiveMediaConfigVersion.cmake
  DESTINATION ${INSTALL_DATADIR}
)
