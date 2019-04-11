# Distributed under the OSI-approved BSD 3-Clause License.  See accompanying
# file Copyright.txt or https://cmake.org/licensing for details.

cmake_minimum_required(VERSION 3.12.3)

# We name the project and the target for the ExternalProject_Add() call
# to something that will highlight to the user what we are working on if
# something goes wrong and an error message is produced.

project(soloud-populate NONE)

include(ExternalProject)
ExternalProject_Add(soloud-populate
                     "UPDATE_DISCONNECTED" "False" "GIT_REPOSITORY" "https://github.com/HuxyUK/soloud.git" "GIT_TAG" "3fd7a4751cae4da38a2e50000eeecad9be7ce543"
                    SOURCE_DIR          "C:/Users/Szymon/Desktop/Project/ex-machina-planned-obsolescence/cmake-build-debug/_deps/soloud-src"
                    BINARY_DIR          "C:/Users/Szymon/Desktop/Project/ex-machina-planned-obsolescence/cmake-build-debug/_deps/soloud-build"
                    CONFIGURE_COMMAND   ""
                    BUILD_COMMAND       ""
                    INSTALL_COMMAND     ""
                    TEST_COMMAND        ""
)
