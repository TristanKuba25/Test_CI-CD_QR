cmake_minimum_required(VERSION 3.18)
project(Test_CI-CD_QR VERSION 0.1.0 LANGUAGES CXX)


set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
include(GNUInstallDirs)


include(${CMAKE_BINARY_DIR}/conanbuildinfo.cmake OPTIONAL)
if (COMMAND conan_basic_setup)         
    conan_basic_setup(TARGETS KEEP_RPATHS)
endif()


find_package(OpenCV REQUIRED COMPONENTS core imgproc highgui)
find_package(zxing-cpp REQUIRED CONFIG)
find_package(tflite REQUIRED CONFIG)

file(GLOB SRC_FILES CONFIGURE_DEPENDS src/*.cpp)
add_executable(${PROJECT_NAME} ${SRC_FILES})

target_link_libraries(${PROJECT_NAME}
        PRIVATE
        OpenCV::OpenCV         
        ZXing::ZXing
        tflite::tflite
        dl pthread)

# (dé-commenter si l’on veut charger dynamiquement la delegate VX NPU)
# target_link_libraries(${PROJECT_NAME} PRIVATE vx_delegate)


install(TARGETS ${PROJECT_NAME}
        RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR})

install(DIRECTORY models/
        DESTINATION ${CMAKE_INSTALL_DATADIR}/${PROJECT_NAME}/models)

include(CPack)
set(CPACK_GENERATOR "DEB")
set(CPACK_PACKAGE_NAME       "${PROJECT_NAME}")
set(CPACK_PACKAGE_VERSION    "${PROJECT_VERSION}")
set(CPACK_PACKAGE_CONTACT    "tristan.sol@kubapay.com")
set(CPACK_DEBIAN_PACKAGE_ARCHITECTURE "arm64")
set(CPACK_DEBIAN_PACKAGE_SECTION "utils")
set(CPACK_DEBIAN_PACKAGE_DEPENDS "libc6 (>= 2.29)")
