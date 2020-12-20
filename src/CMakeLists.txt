# MyLib Library
set(LIBRARY_SOURCES     "my_lib.cc")
set(LIBRARY_INCLUDES    "${PROJECT_SOURCE_DIR}/include/")
set(LIBRARY_HEADERS    "${PROJECT_SOURCE_DIR}/include/my_lib.h") # Für Visual-Studio Projetordner "Header Files"

# Add Sources to the library
add_library(${LIBRARY_NAME} OBJECT ${LIBRARY_SOURCES} ${LIBRARY_HEADERS})

# Tell library where to find necessary header files to build library
target_include_directories(${LIBRARY_NAME} PUBLIC ${LIBRARY_INCLUDES})

# Link library to LinAlg-Library and Loguru Logging library
target_link_libraries(${LIBRARY_NAME} PUBLIC ${LIBRARY_LINALG} ${LIBRARY_LOG})

# Add custom compile features
target_compile_features(${LIBRARY_NAME} PUBLIC cxx_std_17)
