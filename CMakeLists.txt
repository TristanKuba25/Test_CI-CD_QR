cmake_minimum_required(VERSION 3.18)
project(test-ci-cd-qr VERSION 0.1.0 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
include(GNUInstallDirs)

find_package(OpenCV     REQUIRED COMPONENTS core imgproc highgui)
find_package(zxing-cpp  REQUIRED CONFIG)
find_package(tflite     REQUIRED CONFIG)

file(GLOB SRC CONFIGURE_DEPENDS src/*.cpp)
add_executable(${PROJECT_NAME} ${SRC})
target_link_libraries(${PROJECT_NAME}
        PRIVATE OpenCV::OpenCV ZXing::ZXing tflite::tflite dl pthread)

install(TARGETS ${PROJECT_NAME} RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR})
install(DIRECTORY models/ DESTINATION ${CMAKE_INSTALL_DATADIR}/${PROJECT_NAME}/models)

include(CPack)
set(CPACK_GENERATOR                       "DEB")
set(CPACK_PACKAGE_NAME                    "test-ci-cd-qr")
set(CPACK_PACKAGE_VERSION                 "${PROJECT_VERSION}")
set(CPACK_PACKAGE_CONTACT                 "tristan.sol@kubapay.com")
set(CPACK_DEBIAN_PACKAGE_ARCHITECTURE     "arm64")
set(CPACK_DEBIAN_PACKAGE_SECTION          "utils")
set(CPACK_DEBIAN_PACKAGE_DEPENDS          "libc6 (>= 2.29)")
