find_package(GTest REQUIRED)

include_directories(${GTEST_INCLUDE_DIRS} ${CMAKE_SOURCE_DIR}/src)
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_INCLUDE_CURRENT_DIR on)

if (TARGET NerdMatrix)
  set(TEST_LIBS ${GTEST_BOTH_LIBRARIES} pthread NerdMatrix)

  add_executable(test_nerdmatrix test_nerdmatrix.cc)
  target_link_libraries(test_nerdmatrix ${TEST_LIBS})
  add_test(TestNerdMatrix test_nerdmatrix)

  add_executable(test_nerdmatrix_factory test_nerdmatrix_factory.cc)
  target_link_libraries(test_nerdmatrix_factory ${TEST_LIBS})
  add_test(TestNerdMatrixFactory test_nerdmatrix_factory)

  add_executable(test_nerdmatrix_elementwise_ops test_nerdmatrix_elementwise_ops.cc)
  target_link_libraries(test_nerdmatrix_elementwise_ops ${TEST_LIBS})
  add_test(TestNerdMatrixElementwiseOps test_nerdmatrix_elementwise_ops)

  add_executable(test_nerdmatrix_matmul test_nerdmatrix_matmul.cc)
  target_link_libraries(test_nerdmatrix_matmul ${TEST_LIBS})
  add_test(TestNerdMatrixMatMul test_nerdmatrix_matmul)

  add_executable(test_nerdmatrix_concat test_nerdmatrix_concat.cc)
  target_link_libraries(test_nerdmatrix_concat ${TEST_LIBS})
  add_test(TestNerdMatrixConcat test_nerdmatrix_concat)
endif ()

# benchmark
find_library(BENCHMARK_LIBRARIES benchmark)
if (TARGET NerdMatrix AND BENCHMARK_LIBRARIES)
  message(STATUS "Using Google Benchmark ${BENCHMARK_LIBRARIES}")
  set(BENCHMARK_LIBS ${BENCHMARK_LIBRARIES} pthread NerdMatrix)

  add_executable(benchmark_nerdmatrix  benchmark/benchmark_nerdmatrix.cc)
  target_compile_options(benchmark_nerdmatrix PRIVATE -O3)
  target_link_libraries(benchmark_nerdmatrix ${BENCHMARK_LIBS})
  add_test(BenchmarkNerdMatrix benchmark_nerdmatrix)

  add_executable(benchmark_element_wise_ops  benchmark/benchmark_element_wise_ops.cc)
  target_compile_options(benchmark_element_wise_ops PRIVATE -O3)
  target_link_libraries(benchmark_element_wise_ops ${BENCHMARK_LIBS})
  add_test(BenchmarkElementwiseOps benchmark_element_wise_ops)

  add_executable(benchmark_matmul  benchmark/benchmark_matmul.cc)
  target_compile_options(benchmark_matmul PRIVATE -O3)
  target_link_libraries(benchmark_matmul ${BENCHMARK_LIBS})
  add_test(BenchmarkNerdMatMul benchmark_matmul)
endif ()
