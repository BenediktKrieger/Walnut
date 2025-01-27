cmake_minimum_required(VERSION 3.7)
project(Walnut VERSION 1.0.0)


add_definitions(-D_CRT_SECURE_NO_WARNINGS)
IF(APPLE)
    add_definitions(-Wno-nullability-completeness)
    add_definitions(-Wno-deprecated)
ENDIF()
IF(WIN32)
    add_compile_definitions(WL_PLATFORM_WINDOWS)
ENDIF()
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

IF(MSVC)
	set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /std:c++17")
ENDIF()

file(GLOB SOURCES
    Walnut/src/Walnut/*.h
    Walnut/src/Walnut/*.hpp
    Walnut/src/Walnut/*.cpp
    Walnut/src/Walnut/ImGui/*.cpp
    WalnutApp/src/*.h
    WalnutApp/src/*.hpp
    WalnutApp/src/*.cpp
    vendor/imgui/*.cpp
    vendor/imgui/*.h
)

add_executable(${CMAKE_PROJECT_NAME} ${SOURCES})

#add vulkan
find_package(Vulkan REQUIRED)
IF (Vulkan_FOUND)
    target_link_libraries(${CMAKE_PROJECT_NAME} ${Vulkan_LIBRARIES})
    target_include_directories(${CMAKE_PROJECT_NAME} PUBLIC ${Vulkan_INCLUDE_DIR})
ELSE()
    message(ERROR "Vulkan SDK has to be installed")
ENDIF()


#add glfw
set(GLFW_BUILD_DOCS OFF CACHE BOOL "" FORCE)
set(GLFW_BUILD_TESTS OFF CACHE BOOL "" FORCE)
set(GLFW_BUILD_EXAMPLES OFF CACHE BOOL "" FORCE)
add_subdirectory(${PROJECT_SOURCE_DIR}/vendor/glfw)
target_link_libraries(${CMAKE_PROJECT_NAME} glfw)

#add glm
add_subdirectory(${PROJECT_SOURCE_DIR}/vendor/glm)
target_link_libraries(${CMAKE_PROJECT_NAME} glm)

#add imgui, stb
include_directories(${PROJECT_SOURCE_DIR}/vendor/imgui)
include_directories(${PROJECT_SOURCE_DIR}/vendor/imgui/backends)
include_directories(${PROJECT_SOURCE_DIR}/vendor/stb)
include_directories(${PROJECT_SOURCE_DIR}/Walnut/src)

