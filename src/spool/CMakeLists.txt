add_library(polaris_spool STATIC Spool.cc
) target_include_directories(polaris_spool PRIVATE ${CMAKE_SOURCE_DIR}/src
) target_link_libraries(polaris_spool PRIVATE polaris_protos SQLite::SQLite3 spdlog::spdlog
)
