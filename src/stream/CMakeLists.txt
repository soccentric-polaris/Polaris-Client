add_library(polaris_stream STATIC StreamClient.cc
) target_include_directories(polaris_stream PRIVATE ${CMAKE_SOURCE_DIR}/src
) target_link_libraries(polaris_stream PRIVATE polaris_protos ${POLARIS_GRPC_LIBRARIES} spdlog::spdlog
)
