add_library(polaris_config STATIC client_config.cpp client_id.cpp
) target_include_directories(polaris_config PRIVATE ${CMAKE_SOURCE_DIR}/src
) target_link_libraries(polaris_config PRIVATE spdlog::spdlog tomlplusplus::tomlplusplus yaml-cpp::yaml-cpp
)
