cmake_minimum_required(VERSION 3.15)
project(VulkanPlotLib)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Vulkan
find_package(Vulkan REQUIRED)
include_directories(${Vulkan_INCLUDE_DIRS})

# GLFW
add_subdirectory(external/glfw)

# ImGui
add_library(imgui STATIC
    external/imgui/imgui.cpp
    external/imgui/imgui_demo.cpp
    external/imgui/imgui_draw.cpp
    external/imgui/imgui_tables.cpp
    external/imgui/imgui_widgets.cpp
    external/imgui/backends/imgui_impl_glfw.cpp
    external/imgui/backends/imgui_impl_vulkan.cpp
)
target_include_directories(imgui PUBLIC external/imgui external/imgui/backends)
target_link_libraries(imgui glfw)

# Твоя библиотека
add_library(plotlib STATIC
    src/VulkanRenderer.cpp
    src/Plot2D.cpp
    src/Plot3D.cpp
)
target_include_directories(plotlib PUBLIC include)
target_link_libraries(plotlib imgui Vulkan::Vulkan)

# Пример
add_executable(example main.cpp)
target_link_libraries(example plotlib)

