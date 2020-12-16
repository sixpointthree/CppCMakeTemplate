# MyLib Library
set(LIBRARY_SOURCES     "my_lib.cc")
set(LIBRARY_INCLUDES    "${PROJECT_SOURCE_DIR}/include/")

# Add Sources to the library
add_library(${LIBRARY_NAME} OBJECT ${LIBRARY_SOURCES})

# Tell library where to find necessary header files to build library
target_include_directories(${LIBRARY_NAME} PUBLIC ${LIBRARY_INCLUDES})

# Link library to LinAlg-Library and Loguru Logging library
target_link_libraries(${LIBRARY_NAME} PUBLIC ${LIBRARY_LINALG} ${LIBRARY_LOG})

# Add custom compile features
target_compile_features(${LIBRARY_NAME} PUBLIC cxx_std_17)
