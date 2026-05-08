# LIBRARY PROJECT - minisat

# === Includes ===
include(GNUInstallDirs)
include(CMakePackageConfigHelpers)

# === Create Library ===
add_library(minisat "core/Solver.cc" "utils/Options.cc" "utils/System.cc")
target_include_directories(minisat PRIVATE ".")

# === Link Libraries ===
find_package(ZLIB CONFIG)
target_link_libraries(minisat PRIVATE ZLIB::ZLIBSTATIC)

# === Feature Enables ===
if (MS_MSVC_STATIC_RUNTIME)
    set_property(TARGET minisat PROPERTY MSVC_RUNTIME_LIBRARY "MultiThreaded$<$<CONFIG:Debug>:Debug>")
endif()
set_property(TARGET minisat PROPERTY CXX_STANDARD 20)
enable_ipo(minisat)
set_arch(minisat)

# === Install ===
# Install binaries and headers
install(TARGETS ${PROJECT_NAME}
    EXPORT ${PROJECT_NAME}Config
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR} # static library files
    INCLUDES DESTINATION ${CMAKE_INSTALL_INCLUDEDIR} # includes
)

# Export includes
install(DIRECTORY "../include"
        DESTINATION ${CMAKE_INSTALL_PREFIX})
install(DIRECTORY "../src"
        DESTINATION ${CMAKE_INSTALL_PREFIX}
        FILES_MATCHING
        PATTERN "*.h")

# Configure the main config.cmake file
configure_package_config_file(
    ${CMAKE_SOURCE_DIR}/cmake/${PROJECT_NAME}Config.cmake.in
    ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}Config.cmake
    INSTALL_DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/${PROJECT_NAME}
)

# Install the export set and configuration files
install(EXPORT ${PROJECT_NAME}Config
    FILE ${PROJECT_NAME}Targets.cmake
    NAMESPACE ${PROJECT_NAME}::
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/${PROJECT_NAME}
)

install(
    FILES
        ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}Config.cmake
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/${PROJECT_NAME}
)