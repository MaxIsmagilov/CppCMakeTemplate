
cmake_minimum_required(VERSION 3.25)

PROJECT( MyProjectName
    VERSION 0.0.0
    LANGUAGES C CXX
    )

ADD_SUBDIRECTORY(MyProjectName)

ADD_SUBDIRECTORY(third_party)

include(FetchContent)
FetchContent_Declare(
  googletest
  URL https://github.com/google/googletest/archive/03597a01ee50ed33e9dfd640b249b4be3799d395.zip
)
# For Windows: Prevent overriding the parent project's compiler/linker settings
set(gtest_force_shared_crt ON CACHE BOOL "" FORCE)
FetchContent_MakeAvailable(googletest)
target_compile_options(gtest PRIVATE "-Wno-error=restrict")

enable_testing()
include(GoogleTest)
include(CTest)