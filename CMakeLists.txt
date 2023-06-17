project(proto)
cmake_minimum_required(VERSION 3.18)

find_package(Protobuf REQUIRED)

function(generate_cpp_protos)
    file(RELATIVE_PATH PROTO_PATH ${CMAKE_SOURCE_DIR} ${CMAKE_CURRENT_SOURCE_DIR})
    set(PROTO_OUTPUT_PATH ${CMAKE_BINARY_DIR})

    set(PROTO_FILES)

    foreach(ARG ${ARGN})
        list(APPEND PROTO_FILES ${PROTO_PATH}/${ARG})
    endforeach()

    foreach(ARG ${ARGN})
        string(REGEX REPLACE "\.proto$" ".pb.h" PROTO_HDR ${ARG})
        list(APPEND PROTO_HDRS ${CMAKE_CURRENT_BINARY_DIR}/${PROTO_HDR})
        string(REGEX REPLACE "\.proto$" ".pb.cc" PROTO_SRC ${ARG})
        list(APPEND PROTO_SRCS ${CMAKE_CURRENT_BINARY_DIR}/${PROTO_SRC})
    endforeach()

    set(PROTO_HDRS ${PROTO_HDRS} PARENT_SCOPE)
    set(PROTO_SRCS ${PROTO_SRCS} PARENT_SCOPE)

    add_custom_command(
        OUTPUT ${PROTO_HDRS} ${PROTO_SRCS}
        COMMAND protobuf::protoc
        ARGS "--cpp_out=${CMAKE_BINARY_DIR}"
        "--proto_path=${CMAKE_SOURCE_DIR}" ${PROTO_FILES}
    )
endfunction()

add_subdirectory(src)
