set(CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/cmake")

# Copy resources to the build directory
function(copy_resource_to_build_dir file)
    if (WIN32)
        set(CLI_COPY_COMMAND "copy")
    else()
        set(CLI_COPY_COMMAND "cp")
    endif()
    
    add_custom_command(
        OUTPUT "${file}"
        MAIN_DEPENDENCY "${file}"
        COMMAND "${CLI_COPY_COMMAND}" ARGS "${CMAKE_SOURCE_DIR}/Compute/${file}" "${file}"
    )
endfunction()

# Find the dependencies
find_package("GLFW")
find_package("GLEW")
find_package("OpenGL")

copy_resource_to_build_dir("ComputeShader.glsl")
copy_resource_to_build_dir("Fragment.glsl")
copy_resource_to_build_dir("Vertex.glsl")

add_executable(
    "Compute" 
    
    "main.cpp" 
    "main.h"
    
    "ComputeShader.glsl"
    "Fragment.glsl"
    "Vertex.glsl"
)

target_include_directories("Compute" PRIVATE "${GLFW_INCLUDE_DIR}" "${OPENGL_INCLUDE_DIR}" "${GLEW_INCLUDE_DIRS}")

target_link_libraries("Compute" "${GLFW_LIBRARY}" "${OPENGL_LIBRARIES}" "${GLEW_LIBRARIES}")
if (UNIX)
    target_link_libraries("Compute" "pthread" "stdc++fs")
endif()