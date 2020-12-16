set(TEST_SOURCES "main.cc")

add_executable(${TEST_MAIN} ${TEST_SOURCES})

# Zu testende Library und gtest linken
if(WIN32)
    # Unit testing immer ohne Optimierung und im DEBUG-Modus:
    set(TEST_FLAGS "-Od")
    target_link_libraries(${TEST_MAIN} PRIVATE ${LIBRARY_NAME} gtest)
endif()

if(UNIX)
    # Unit testing immer ohne Optimierung und im DEBUG-Modus:
    set(TEST_FLAGS "-O0;-g")
    target_link_libraries(${TEST_MAIN} PRIVATE ${LIBRARY_NAME} gtest pthread)
endif()

# Compile Flags setzen
target_compile_options(${TEST_MAIN} PUBLIC ${TEST_FLAGS})
# Compile features setzen (C++ Version)
target_compile_features(${TEST_MAIN} PUBLIC cxx_std_17)

add_test(NAME ${LIBRARY_NAME}.${TEST_MAIN} COMMAND ${TEST_MAIN})

# UNIX: sudo apt-get install lcov
# UNIX: sudo apt-get install gcovr
if(CODE_COVERAGE_AVAILABLE)
    set(COVERAGE_MAIN "${PROJECT}_coverage")
    set(COVERAGE_LCOV_EXCLUDES
        "${PROJECT_SOURCE_DIR}/external/*"
        "${PROJECT_SOURCE_DIR}/app/*"
        "${CONAN_GTEST_ROOT}/*"
        "${CONAN_BOOST_ROOT}/*")
    setup_target_for_coverage_lcov(
        NAME ${COVERAGE_MAIN}
        EXECUTABLE ctest
        DEPENDENCIES ${TEST_MAIN})
endif()