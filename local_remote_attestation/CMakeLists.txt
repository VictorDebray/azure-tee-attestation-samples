# Copyright (c) Open Enclave SDK contributors.
# Licensed under the MIT License.

cmake_minimum_required(VERSION 3.11)

if (LVI_MITIGATION MATCHES ControlFlow)
  # Configure the cmake to use customized compilation toolchain.
  # This package has to be added before `project()`.
  find_package(OpenEnclave-LVI-Mitigation CONFIG REQUIRED)
endif ()

project("Remote Attestation Sample" LANGUAGES C CXX)

find_package(OpenEnclave CONFIG REQUIRED)

set(CMAKE_CXX_STANDARD 11)
set(OE_CRYPTO_LIB
    mbedtls
    CACHE STRING "Crypto library used by enclaves.")

set(OE_INCLUDE_DIR "/opt/openenclave/include")

add_subdirectory(common)
add_subdirectory(enclave_a)
add_subdirectory(enclave_b)
add_subdirectory(host)

add_custom_target(sign ALL DEPENDS enclave_a_signed enclave_b_signed)

if ((NOT DEFINED ENV{OE_SIMULATION}) OR (NOT $ENV{OE_SIMULATION}))
  add_custom_target(run
    DEPENDS remote_attestation_host sign
    COMMAND remote_attestation_host ${CMAKE_BINARY_DIR}/enclave_a/enclave_a.signed ${CMAKE_BINARY_DIR}/enclave_b/enclave_b.signed ${CMAKE_SOURCE_DIR}/lorem_ipsum)
endif ()
