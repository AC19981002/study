# The lowest cmake version required
cmake_minimum_required(VERSION 3.20)

set(CMAKE_CXX_STANDARD 14)
set(PROJECT_NAME study)

project (${PROJECT_NAME})

#find the all source code as DIR_SRCS
aux_source_directory(src DIR_SRCS)

# add compilanle file to project (like main file...)
add_executable(${PROJECT_NAME} ${DIR_SRCS})

# 设置MSVC编译编码
# visual studio默认编译文件的编码是utf-8 with bom，
# 在没有中文的情况下，直接编译是没有问题的，然而当源文
# 件含有中文时（比如中文注释），则可能会出现异常，报一
# 些莫名其妙的语法错误。解决办法是通过CMake语句通知MSVC编译
# 时采用utf-8编码
add_compile_options("$<$<CXX_COMPILER_ID:MSVC>:/source-charset:utf-8>")

# set workspace path
set_property(TARGET ${PROJECT_NAME} PROPERTY VS_DEBUGGER_WORKING_DIRECTORY 
    ${CMAKE_SOURCE_DIR}/resources
)

# copy resources to workspace
add_custom_command(TARGET ${PROJECT_NAME} POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E copy_directory
    ${CMAKE_SOURCE_DIR}/resources
    $<TARGET_FILE_DIR:${PROJECT_NAME}>/resources
)



