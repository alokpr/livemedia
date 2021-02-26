cmake_minimum_required(VERSION 3.10)
project(livemedia VERSION "2021.2.11" LANGUAGES CXX)

find_package(OpenSSL REQUIRED)

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

install(DIRECTORY ${INSTALL_DIR}/bin/ DESTINATION ${CMAKE_INSTALL_BINDIR})
install(DIRECTORY ${INSTALL_DIR}/include/ DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})
install(DIRECTORY ${INSTALL_DIR}/lib/ DESTINATION ${CMAKE_INSTALL_LIBDIR})
