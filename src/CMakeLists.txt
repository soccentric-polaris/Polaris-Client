# ---------------------------------------------------------------------------
# src/CMakeLists.txt
# Adds each module as a static library then defines the main executable.
# Subdirectories are ordered leaf-first so inter-target references resolve.
# ---------------------------------------------------------------------------

add_subdirectory(config)
add_subdirectory(diag)
add_subdirectory(spool)
add_subdirectory(stream)
add_subdirectory(hal_bridge)
add_subdirectory(registry)
add_subdirectory(ota)
add_subdirectory(log)
add_subdirectory(janitor)
add_subdirectory(run)
add_subdirectory(hal_service)

# ---------------------------------------------------------------------------
# Main executable
# ---------------------------------------------------------------------------

add_executable(polaris-client
    main.cpp
    app.cpp
)

target_include_directories(polaris-client PRIVATE
    ${CMAKE_CURRENT_SOURCE_DIR}
    ${CMAKE_SOURCE_DIR}/../Polaris-HAL/src
)

target_link_libraries(polaris-client PRIVATE
    polaris_config
    polaris_diag
    polaris_hal_bridge
    polaris_hal_service
    polaris_janitor
    polaris_log
    polaris_ota
    polaris_registry
    polaris_run
    polaris_spool
    polaris_stream
    polaris_protos
    spdlog::spdlog
    tomlplusplus::tomlplusplus
    yaml-cpp::yaml-cpp
    SQLite::SQLite3
    ${POLARIS_GRPC_LIBRARIES}
    protobuf::libprotobuf
)

target_compile_definitions(polaris-client PRIVATE
    POLARIS_CLIENT_VERSION="${PROJECT_VERSION}"
)

set_compiler_warnings(polaris-client)
