# SPDX-License-Identifier: GPL-2.0-or-later

set(INC
  .
  ../blenlib
  ../makesdna
  ../../../intern/guardedalloc
)

set(INC_SYS
)

set(SRC
  intern/cpp_types.cc
  intern/field.cc
  intern/generic_vector_array.cc
  intern/generic_virtual_array.cc
  intern/generic_virtual_vector_array.cc
  intern/multi_function.cc
  intern/multi_function_builder.cc
  intern/multi_function_params.cc
  intern/multi_function_procedure.cc
  intern/multi_function_procedure_builder.cc
  intern/multi_function_procedure_executor.cc
  intern/multi_function_procedure_optimization.cc

  FN_cpp_type.hh
  FN_cpp_type_make.hh
  FN_field.hh
  FN_field_cpp_type.hh
  FN_generic_array.hh
  FN_generic_pointer.hh
  FN_generic_span.hh
  FN_generic_value_map.hh
  FN_generic_vector_array.hh
  FN_generic_virtual_array.hh
  FN_generic_virtual_vector_array.hh
  FN_multi_function.hh
  FN_multi_function_builder.hh
  FN_multi_function_context.hh
  FN_multi_function_data_type.hh
  FN_multi_function_param_type.hh
  FN_multi_function_params.hh
  FN_multi_function_procedure.hh
  FN_multi_function_procedure_builder.hh
  FN_multi_function_procedure_executor.hh
  FN_multi_function_procedure_optimization.hh
  FN_multi_function_signature.hh
)

set(LIB
  bf_blenlib
)

if(WITH_TBB)
  add_definitions(-DWITH_TBB)
  if(WIN32)
    # TBB includes Windows.h which will define min/max macros
    # that will collide with the stl versions.
    add_definitions(-DNOMINMAX)
  endif()
  list(APPEND INC_SYS
    ${TBB_INCLUDE_DIRS}
  )

  list(APPEND LIB
    ${TBB_LIBRARIES}
  )
endif()

blender_add_lib(bf_functions "${SRC}" "${INC}" "${INC_SYS}" "${LIB}")

if(WITH_GTESTS)
  set(TEST_SRC
    tests/FN_cpp_type_test.cc
    tests/FN_field_test.cc
    tests/FN_generic_array_test.cc
    tests/FN_generic_span_test.cc
    tests/FN_generic_vector_array_test.cc
    tests/FN_multi_function_procedure_test.cc
    tests/FN_multi_function_test.cc

    tests/FN_multi_function_test_common.hh
  )
  set(TEST_LIB
    bf_functions
  )
  include(GTestTesting)
  blender_add_test_lib(bf_functions_tests "${TEST_SRC}" "${INC};${TEST_INC}" "${INC_SYS}" "${LIB};${TEST_LIB}")
endif()
