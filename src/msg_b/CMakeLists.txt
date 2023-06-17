generate_cpp_protos(msg_b.proto)

add_library(msg_b ${PROTO_HDRS} ${PROTO_SRCS})
target_include_directories(msg_b PUBLIC ${CMAKE_BINARY_DIR})
target_link_libraries(msg_b msg_a protobuf::libprotobuf)
