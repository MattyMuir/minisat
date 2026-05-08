# EXECUTABLE PROJECT - tests

# === Create Executable ===
add_executable(tests "tests.cpp")

# === Libraries ===
find_package(ZLIB CONFIG)
target_link_libraries(tests PRIVATE ZLIB::ZLIBSTATIC)

target_link_libraries(tests PRIVATE minisat)
target_include_directories(tests PRIVATE "../include/")
target_include_directories(tests PRIVATE "../src")

# === Feature Enables ===
if (MS_MSVC_STATIC_RUNTIME)
	set_property(TARGET tests PROPERTY MSVC_RUNTIME_LIBRARY "MultiThreaded$<$<CONFIG:Debug>:Debug>")
endif()
set_property(TARGET tests PROPERTY CXX_STANDARD 20)
enable_ipo(tests)
set_arch(tests)