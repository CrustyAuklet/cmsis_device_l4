cmake_minimum_required(VERSION 3.15)
project(STM32L4_device
        VERSION 1.6.0
        DESCRIPTION "STM32L4 CMSIS device as a CMake package"
        LANGUAGES C ASM)

include_guard(GLOBAL)

if(DEFINED cmsis_MCU)
    message(STATUS "STM32L4::DEVICE : Configuring for MCU == ${cmsis_MCU}")
else()
    message(FATAL_ERROR "STM32L4::DEVICE : cmsis_MCU is not defined!")
endif()

add_library(STM32L4_DEVICE INTERFACE)
add_library(STM32L4::DEVICE ALIAS STM32L4_DEVICE)
target_include_directories(STM32L4_DEVICE INTERFACE ${CMAKE_CURRENT_LIST_DIR}/Include)

string(TOLOWER ${cmsis_MCU} STM32_DEVICE_NAME)
target_compile_definitions(STM32L4_DEVICE INTERFACE ${cmsis_MCU})

target_sources(STM32L4_DEVICE INTERFACE
    Include/stm32l4xx.h
    Source/Templates/gcc/startup_${STM32_DEVICE_NAME}.s
    Source/Templates/system_stm32l4xx.c)
