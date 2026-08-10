add_library(polaris_hal_bridge STATIC ResultSerializer.cc
) target_include_directories(polaris_hal_bridge PRIVATE ${CMAKE_SOURCE_DIR}/src ${CMAKE_SOURCE_DIR}/../Polaris-HAL/src
) target_link_libraries(polaris_hal_bridge PRIVATE polaris_protos spdlog::spdlog
)
