cmake_minimum_required(VERSION 2.8.3)
project(odva_ethernetip)

find_package(console_bridge REQUIRED)
find_package(Boost 1.47 REQUIRED COMPONENTS system)
find_package(GTest REQUIRED)


include_directories(
  include
  ${Boost_INCLUDE_DIRS}
  ${console_bridge_INCLUDE_DIRS}
  ${GTEST_INCLUDE_DIRS}
)

## Declare a cpp library for just Ethernet/IP generic stuff
add_library(${PROJECT_NAME}
  src/connection.cpp
  src/copy_serializable.cpp
  src/cpf_item.cpp
  src/cpf_packet.cpp
  src/encap_header.cpp
  src/encap_packet.cpp
  src/identity_item_data.cpp
  src/io_scanner.cpp
  src/message_router_response.cpp
  src/path.cpp
  src/rr_data.cpp
  src/session.cpp
)
target_link_libraries(${PROJECT_NAME}
  ${Boost_LIBRARIES}
  ${console_bridge_LIBRARIES}
)

set(${PROJECT_NAME}_TESTS
    test/serialization/buffer_writer_test.cpp
    test/serialization/buffer_reader_test.cpp
    test/serialization/stream_writer_test.cpp
    test/serialization/stream_reader_test.cpp
    test/serialization/serializable_buffer_test.cpp
    test/serialization/serializable_primitive_test.cpp
    test/test_socket_test.cpp
    test/encap_header_test.cpp
    test/encap_packet_test.cpp
    test/cpf_item_test.cpp
    test/cpf_packet_test.cpp
    test/sequenced_address_item_test.cpp
    test/identity_item_data_test.cpp
    test/path_test.cpp
    test/rr_data_test.cpp
    test/message_router_request_test.cpp
    test/message_router_response_test.cpp
    test/rr_data_request_test.cpp
    test/rr_data_response_test.cpp
    test/forward_open_request_test.cpp
    test/forward_open_success_test.cpp
    test/forward_close_request_test.cpp
    test/forward_close_success_test.cpp
    test/session_test.cpp
)

add_executable(${PROJECT_NAME}-test test/test_main.cpp ${${PROJECT_NAME}_TESTS})
target_link_libraries(${PROJECT_NAME}-test 
                          ${Boost_LIBRARIES} 
                          ${PROJECT_NAME} 
                          ${GTEST_LIBRARIES} 
                          gtest 
                          pthread
)

