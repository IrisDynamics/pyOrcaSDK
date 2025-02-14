cmake_minimum_required(VERSION 3.17...3.27)

# Scikit-build-core sets these values for you, or you can just hard-code the
# name and version.
project(
  ${SKBUILD_PROJECT_NAME}
  VERSION ${SKBUILD_PROJECT_VERSION}
  DESCRIPTION "C++ bindings for Iris Dynamics orcaSDK"
  LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 14)

include(FetchContent)

FetchContent_Declare(
  orcaSDK
  GIT_REPOSITORY https://github.com/IrisDynamics/orcaSDK.git
  GIT_TAG origin/linux-comport-path
)

FetchContent_MakeAvailable(orcaSDK)

add_subdirectory(extern/pybind11 pybind11)

pybind11_add_module(_pyOrcaSDK
    bindings/pyOrcaSDK.cpp
)

target_link_libraries(_pyOrcaSDK PRIVATE orcaSDK::core)

install(TARGETS _pyOrcaSDK LIBRARY DESTINATION pyOrcaSDK)