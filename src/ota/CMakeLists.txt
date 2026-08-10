add_library(polaris_ota STATIC
    OtaClient.cc
)

target_include_directories(polaris_ota PRIVATE
    ${CMAKE_SOURCE_DIR}/src
)

target_link_libraries(polaris_ota PRIVATE
    polaris_protos
    ${POLARIS_GRPC_LIBRARIES}
    spdlog::spdlog
)
