cmake_minimum_required(VERSION 3.10)

set(PROJECT "CppTemplate")

project("${PROJECT}_Project" VERSION 0.12.3 LANGUAGES CXX)

# Global CMake variables are set here
set(CMAKE_CXX_STANDARD              17)
set(CMAKE_CXX_STANDARD_REQUIRED     ON)
set(CMAKE_CXX_EXTENSIONS            OFF)

# Project, Library and Test Names
set(LIBRARY_NAME    "${PROJECT}_MyLib")
set(LIBRARY_LINALG  "${PROJECT}_LinAlg")
set(LIBRARY_LOG     "${PROJECT}_loguru")
# celoro muss genau wie der Projektname in der CMakeLists.txt der Lib heißen
# Es sei denn man gibt ihm in der CMakeLists.txt einen Alias Namen
#set(LIBRARY_BENCH   celero)
set(LIBRARY_BENCH   "${PROJECT}_celero")
set(MAIN            "${PROJECT}_Executable")
set(TEST_MAIN       "${PROJECT}_unit_tests")

# Options for generating/building
# None
option(USE_GCOV "Create a GCov-enabled build." OFF)
option(USE_BENCHMARKS "Create a benchmark-enabled build." OFF)

# Configuration Files
configure_file("${PROJECT_SOURCE_DIR}/include/ProjectConfig.h.in"
               "${PROJECT_BINARY_DIR}/ProjectConfig.h" @ONLY)

# Include own cmake modules
set(CMAKE_MODULE_PATH "${PROJECT_SOURCE_DIR}/cmake/")
#  Include cmake documentation module
include(Documentation)
#  Include cmake ctest module
include(CTest)
#  Include code coverage
set(CODE_COVERAGE_AVAILABLE OFF)
if (USE_GCOV)
    if(CMAKE_CXX_COMPILER_ID MATCHES "GNU|Clang")
        set(CODE_COVERAGE_AVAILABLE ON)
        include(CodeCoverage)
    else()
        message("You must use a GNU or Clang Compiler for CodeCoverage!")
    endif()
endif()

# Add conan packages
# Bevor Build: conan install ../12_FinalTemplate/conanfile.txt
include(${CMAKE_BINARY_DIR}/conanbuildinfo.cmake)
conan_basic_setup()

# Add externals
add_subdirectory(external)

# Add libraries
add_subdirectory(src)

# Add executables
add_subdirectory(app)

# Add unit tests
add_subdirectory(tests)

# Add benchmarking
add_subdirectory(benchmarks)