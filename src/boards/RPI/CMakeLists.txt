project(RPI)
cmake_minimum_required(VERSION 3.6)
enable_language(ASM)

#---------------------------------------------------------------------------------------
# Target
#---------------------------------------------------------------------------------------

list(APPEND ${PROJECT_NAME}_SOURCES
    "${CMAKE_CURRENT_SOURCE_DIR}/empty.c"
)

add_library(${PROJECT_NAME} OBJECT EXCLUDE_FROM_ALL ${${PROJECT_NAME}_SOURCES})

target_compile_definitions(${PROJECT_NAME} PUBLIC -DUSE_HAL_DRIVER -DRPI)

# Add define if debbuger support is enabled
target_compile_definitions(${PROJECT_NAME} PUBLIC $<$<BOOL:${USE_DEBUGGER}>:USE_DEBUGGER>)

# Add define if radio debug pins support is enabled
target_compile_definitions(${PROJECT_NAME} PUBLIC $<$<BOOL:${USE_RADIO_DEBUG}>:USE_RADIO_DEBUG>)

target_include_directories(${PROJECT_NAME} PUBLIC
    ${CMAKE_CURRENT_SOURCE_DIR}
    ${CMAKE_CURRENT_SOURCE_DIR}/cmsis
    $<TARGET_PROPERTY:board,INTERFACE_INCLUDE_DIRECTORIES>
    $<TARGET_PROPERTY:system,INTERFACE_INCLUDE_DIRECTORIES>
    $<TARGET_PROPERTY:radio,INTERFACE_INCLUDE_DIRECTORIES>
    $<TARGET_PROPERTY:peripherals,INTERFACE_INCLUDE_DIRECTORIES>
)

set_property(TARGET ${PROJECT_NAME} PROPERTY C_STANDARD 11)
