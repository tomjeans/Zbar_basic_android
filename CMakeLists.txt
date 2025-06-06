# Set the minimum version of CMake required  
cmake_minimum_required(VERSION 3.4.1)

# Set the project name and version  
project(zbarjni VERSION 1.1)

# Enable C++11 support  
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)
set(JNI_SOURCE ${CMAKE_SOURCE_DIR}/libs/libiconv-1.14)
# Include directories  
include_directories(${CMAKE_SOURCE_DIR}
        ${CMAKE_SOURCE_DIR}/include
        ${JNI_SOURCE}/libiconv-1.14/libcharset
        ${JNI_SOURCE}/libiconv-1.14/include
        ${JNI_SOURCE}/libiconv-1.14/libcharset/include
        ${CMAKE_SOURCE_DIR}/zbar)

# libiconv library  
add_library(libiconv SHARED
        ${JNI_SOURCE}/libiconv-1.14/lib/iconv.c
        ${JNI_SOURCE}/libiconv-1.14/libcharset/lib/localcharset.c
        ${JNI_SOURCE}/libiconv-1.14/lib/relocatable.c)

target_compile_options(libiconv PRIVATE
        -Wno-multichar
        -D_ANDROID
        -DLIBDIR="c"
        -DBUILDING_LIBICONV
        -DBUILDING_LIBCHARSET
        -DIN_LIBRARY)

# zbar library  
add_library(zbarjni SHARED
        #${JNI_SOURCE}/zbarjni.c
        ${CMAKE_SOURCE_DIR}/zbar/img_scanner.c
        ${CMAKE_SOURCE_DIR}/zbar/sqcode.h
        ${CMAKE_SOURCE_DIR}/zbar/sqcode.c
        ${CMAKE_SOURCE_DIR}/zbar/decoder.c
        ${CMAKE_SOURCE_DIR}/zbar/image.c
        ${CMAKE_SOURCE_DIR}/zbar/symbol.c
        ${CMAKE_SOURCE_DIR}/zbar/convert.c
        ${CMAKE_SOURCE_DIR}/zbar/config.c
        ${CMAKE_SOURCE_DIR}/zbar/scanner.c
        ${CMAKE_SOURCE_DIR}/zbar/error.c
        ${CMAKE_SOURCE_DIR}/zbar/refcnt.c
        ${CMAKE_SOURCE_DIR}/zbar/video.c
        ${CMAKE_SOURCE_DIR}/zbar/video/null.c
        ${CMAKE_SOURCE_DIR}/zbar/decoder/sq_finder.c
        ${CMAKE_SOURCE_DIR}/zbar/decoder/code128.c
        ${CMAKE_SOURCE_DIR}/zbar/decoder/code39.c
        ${CMAKE_SOURCE_DIR}/zbar/decoder/code93.c
        ${CMAKE_SOURCE_DIR}/zbar/decoder/codabar.c
        ${CMAKE_SOURCE_DIR}/zbar/decoder/databar.c
        ${CMAKE_SOURCE_DIR}/zbar/decoder/ean.c
        ${CMAKE_SOURCE_DIR}/zbar/decoder/i25.c
        ${CMAKE_SOURCE_DIR}/zbar/decoder/qr_finder.c
        ${CMAKE_SOURCE_DIR}/zbar/qrcode/bch15_5.c
        ${CMAKE_SOURCE_DIR}/zbar/qrcode/binarize.c
        ${CMAKE_SOURCE_DIR}/zbar/qrcode/isaac.c
        ${CMAKE_SOURCE_DIR}/zbar/qrcode/qrdec.c
        ${CMAKE_SOURCE_DIR}/zbar/qrcode/qrdectxt.c
        ${CMAKE_SOURCE_DIR}/zbar/qrcode/rs.c
        ${CMAKE_SOURCE_DIR}/zbar/qrcode/util.c)

target_link_libraries(zbarjni PRIVATE libiconv log)

# Link against Android log and charset libraries  
#target_link_libraries(zbar log charset)

# Enable verbose build output (optional)  
set(CMAKE_VERBOSE_MAKEFILE ON)

# Set the default build type (optional)  
set(CMAKE_BUILD_TYPE Release)
