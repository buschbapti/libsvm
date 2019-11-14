cmake_minimum_required(VERSION 3.9)

# Options. Turn on with 'cmake -Druntests=ON'.
option(runtests "Build all tests." OFF) # Makes boolean 'test' available.

project(libsvm)

# Default to C99
if(NOT CMAKE_C_STANDARD)
  set(CMAKE_C_STANDARD 99)
endif()

# Default to C++14
if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 14)
endif()

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

include_directories(
  include
)

set(CORE_SOURCES
	src/svm.cpp
)

add_library(${PROJECT_NAME} SHARED 
  ${CORE_SOURCES}
)

install(DIRECTORY include/
  DESTINATION include
)

install(TARGETS ${PROJECT_NAME}
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
  RUNTIME DESTINATION bin
)

add_executable(svm-predict src/svm-predict.c)
target_link_libraries(svm-predict
  ${PROJECT_NAME}
)

add_executable(svm-scale src/svm-scale.c)
target_link_libraries(svm-scale
  ${PROJECT_NAME}
)

add_executable(svm-train src/svm-train.c)
target_link_libraries(svm-train
  ${PROJECT_NAME}
)
