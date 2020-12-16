## LINALG -- HEADER ONLY ohne CMakeLists.txt
# Ordnerpfad speichern
set(LIBRARY_INCLUDES "linalg/")

# Header-only library einbinden mit "INTERFACE"
add_library(${LIBRARY_LINALG} INTERFACE)

# Der Library sagen, in welchem Ordner diese Header liegen (Ordnerpfad)
target_include_directories(${LIBRARY_LINALG} INTERFACE ${LIBRARY_INCLUDES})



## LOGURU -- HEADER & CPPs ohne CMakeLists.txt
# Ordnerpfad speichern
# ALTERNATIVE: set(LOGURU_SOURCES "loguru.cpp")
file(GLOB LOGURU_SOURCES "loguru/*.cpp")

# STATIC oder SHARED Library
add_library(${LIBRARY_LOG} ${LOGURU_SOURCES})

# Der Library sagen, in welchem Ordner diese Header liegen (Ordnerpfad)
target_include_directories(${LIBRARY_LOG} PUBLIC loguru)
if(UNIX)
    target_link_libraries(${LIBRARY_LOG} PUBLIC pthread dl)
endif()



## CELERO -- HEADER & CPPs mit CMakeLists.txt
if(USE_BENCHMARKS)
    # CMakeLists.txt von Celero-Lib hinzufügen
    add_subdirectory(Celero)
    # Alias-Namen setzen, dass er die Bibliothek celero findet
    add_library(${LIBRARY_BENCH} ALIAS celero)
endif()