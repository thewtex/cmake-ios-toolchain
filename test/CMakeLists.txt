cmake_minimum_required(VERSION 3.4)
project(cmake-ios-toolchain-tests)

enable_testing()

# Add a test with the given test name to run with the given toolchain file.
# The test name should correspond to a directory of the same name.
function(ios_toolchain_test test_name toolchain_file)
  get_filename_component(toolchain_id ${toolchain_file} NAME)
  string(REPLACE ".toolchain.cmake" "" toolchain_id "${toolchain_id}")
  set(id_test_name "${toolchain_id}_${test_name}")
  set(test_dir ${PROJECT_BINARY_DIR}/Testing/${id_test_name})
  add_test(NAME ${id_test_name}_Clean
    COMMAND ${CMAKE_COMMAND} -E remove_directory ${test_dir}
    )
  add_test(NAME ${id_test_name}_MakeDirectory
    COMMAND ${CMAKE_COMMAND} -E make_directory ${test_dir}
    )
  set_tests_properties(${id_test_name}_MakeDirectory
    PROPERTIES DEPENDS ${id_test_name}_Clean)
  add_test(NAME ${id_test_name}_Configure
    COMMAND ${CMAKE_COMMAND}
      -G${CMAKE_GENERATOR}
      -DCMAKE_TOOLCHAIN_FILE=${toolchain_file}
      ${PROJECT_SOURCE_DIR}/${test_name}
    WORKING_DIRECTORY ${test_dir}
    )
  set_tests_properties(${id_test_name}_Configure
    PROPERTIES DEPENDS ${id_test_name}_MakeDirectory)
  add_test(NAME ${id_test_name}_Build
    COMMAND ${CMAKE_COMMAND} --build . --clean-first
    WORKING_DIRECTORY ${test_dir}
    )
  set_tests_properties(${id_test_name}_Build
    PROPERTIES DEPENDS ${id_test_name}_Configure)
endfunction()

set(toolchain_file ${PROJECT_SOURCE_DIR}/../ios.simulator.toolchain.cmake)

ios_toolchain_test(Build ${toolchain_file})
