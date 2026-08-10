add_library(polaris_diag STATIC DiagSocket.cc
) target_include_directories(polaris_diag PRIVATE ${CMAKE_SOURCE_DIR}/src
) target_link_libraries(polaris_diag PRIVATE spdlog::spdlog
)
