cmake_minimum_required(VERSION 3.5)
project(odva_ethernetip)

find_package(ament_cmake REQUIRED)
find_package(console_bridge REQUIRED)
find_package(Boost 1.47 REQUIRED COMPONENTS system)
find_package(GTest REQUIRED)

# message(ERROR "test print")
# message(ERROR "${console_bridge_LIBRARIES}")
# message(ERROR "${console_bridge_INCLUDE_DIRS}")
# message(ERROR "${console_bridge_SRC}")
# message(ERROR "${Boost_LIBRARIES}")
# message(ERROR "${Boost_INCLUDE_DIR}")

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
# target_link_libraries(${PROJECT_NAME}
#   PRIVATE
#     ${Boost_LIBRARIES}
#     ${console_bridge_LIBRARIES}
# )
target_include_directories(${PROJECT_NAME} 
  PUBLIC 
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>  
    $<INSTALL_INTERFACE:include>
  PRIVATE
    ${console_bridge_INCLUDE_DIRS}
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
  PRIVATE
    ${Boost_LIBRARIES} 
    ${PROJECT_NAME} 
    ${GTEST_LIBRARIES} 
    gtest
    pthread
)

ament_target_dependencies(
  ${PROJECT_NAME} 
    console_bridge
    Boost
)

ament_export_targets(export_odva_ethernetip HAS_LIBRARY_TARGET)

ament_export_dependencies(
    console_bridge
    Boost
)

install(
  DIRECTORY include/
  DESTINATION include
)

install(
  TARGETS odva_ethernetip
  EXPORT export_odva_ethernetip
  LIBRARY DESTINATION lib
  ARCHIVE DESTINATION lib
  RUNTIME DESTINATION bin
  INCLUDES DESTINATION include
)

ament_package()
