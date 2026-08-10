add_library(polaris_log STATIC
    LogCollector.cc
)

target_include_directories(polaris_log PRIVATE
    ${CMAKE_SOURCE_DIR}/src
)

target_link_libraries(polaris_log PRIVATE
    polaris_protos
    spdlog::spdlog
)
