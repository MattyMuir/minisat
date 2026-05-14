# EXECUTABLE PROJECT - example

# === Create Executable ===
add_executable(example "example.cc")

# === Libraries ===
find_package(ZLIB CONFIG)
target_link_libraries(example PRIVATE ZLIB::ZLIBSTATIC)

target_link_libraries(example PRIVATE minisat)
target_include_directories(example PRIVATE "../include/")

# === Feature Enables ===
if (MS_MSVC_STATIC_RUNTIME)
	set_property(TARGET example PROPERTY MSVC_RUNTIME_LIBRARY "MultiThreaded$<$<CONFIG:Debug>:Debug>")
endif()
set_property(TARGET example PROPERTY CXX_STANDARD 20)
enable_ipo(example)
set_arch(example)