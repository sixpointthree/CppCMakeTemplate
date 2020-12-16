if (USE_BENCHMARKS)
    set(BENCHMARK_SOURCES "main.cc")
    set(BENCHMARK_MAIN "${PROJECT}_benchmarks")
    
    if(CMAKE_CXX_COMPILER_ID MATCHES "MSVC")
        # Benchmarking immer mit den höchsten Optimierung:
        set(BENCHMARK_FLAGS "/Ox")
    elseif(CMAKE_CXX_COMPILER_ID MATCHES "GNU|Clang")
        # Benchmarking immer mit den höchsten Optimierung:
        set(BENCHMARK_FLAGS "-O3")
    else()
        # Falls unbekannter Compiler, keine Flags setzen
        set(BENCHMARK_FLAGS "")
    endif()

    add_executable(${BENCHMARK_MAIN} ${BENCHMARK_SOURCES})
    target_compile_options(${BENCHMARK_MAIN} PUBLIC ${BENCHMARK_FLAGS})
    target_compile_features(${BENCHMARK_MAIN} PUBLIC cxx_std_17)
    # Libs linken, die gebenchmarked werden sollen
    target_link_libraries(${BENCHMARK_MAIN} PUBLIC ${LIBRARY_NAME})
    target_link_libraries(${BENCHMARK_MAIN} PUBLIC ${LIBRARY_BENCH})
endif()