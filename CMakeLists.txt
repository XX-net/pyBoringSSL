cmake_minimum_required(VERSION 3.18)


# if CMAKE_BUILD_TYPE undefined, we set it to Release
if(NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE "Release")
    set(CMAKE_BUILD_TYPE "Release" CACHE STRING "Choose Release or Debug" FORCE)
    SET(CMAKE_CONFIGURATION_TYPES ${CMAKE_BUILD_TYPE} CACHE STRING "" FORCE)
endif()


project(pyBoringSSL)

# To avoid Error C2220 in Windows.
# https://learn.microsoft.com/en-us/cpp/error-messages/compiler-errors-1/compiler-error-c2220?view=msvc-170
if (MSVC)
    # disable warnings as errors
    add_compile_options(/W3)
endif()

if (${CMAKE_SYSTEM_NAME} MATCHES "Darwin")
    set(CMAKE_MACOSX_RPATH 1)
endif()

add_subdirectory(boringssl)

add_subdirectory(brotli)

add_subdirectory(cert_decompress)
add_dependencies(cert_decompress ssl)

add_subdirectory(getpeercert)
add_dependencies(getpeercert ssl)

add_subdirectory(cffi_boringssl)
add_dependencies(cffi_boringssl ssl)
