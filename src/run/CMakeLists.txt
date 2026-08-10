add_library(polaris_run STATIC RunOrchestrator.cc
) target_include_directories(polaris_run PRIVATE ${CMAKE_SOURCE_DIR}/src ${CMAKE_SOURCE_DIR}/../Polaris-HAL/src
) target_link_libraries(polaris_run PRIVATE polaris_hal_bridge polaris_protos spdlog::spdlog
)
