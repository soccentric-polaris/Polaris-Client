add_library(polaris_hal_service STATIC HalServiceImpl.cc
) target_include_directories(polaris_hal_service PRIVATE ${CMAKE_SOURCE_DIR}/src ${CMAKE_SOURCE_DIR}/../Polaris-HAL/src
) target_link_libraries(polaris_hal_service PRIVATE polaris_registry polaris_hal_bridge polaris_protos ${POLARIS_GRPC_LIBRARIES} protobuf::libprotobuf spdlog::spdlog
)
