add_library(polaris_janitor STATIC
    Janitor.cc
)

target_include_directories(polaris_janitor PRIVATE
    ${CMAKE_SOURCE_DIR}/src
)

target_link_libraries(polaris_janitor PRIVATE
    polaris_protos
    spdlog::spdlog
)
