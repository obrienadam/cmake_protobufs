generate_cpp_protos(msg_a.proto)

add_library(msg_a ${PROTO_HDRS} ${PROTO_SRCS})
target_link_libraries(msg_a protobuf::libprotobuf)
target_include_directories(msg_a PUBLIC ${CMAKE_BINARY_DIR})
