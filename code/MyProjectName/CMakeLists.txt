

SET(MyProjectName_Headers
    # your header files here    
    example.hpp
    )

SET(MyProjectName_Impl
    # your implementation files here  
    example.cpp
    )

ADD_LIBRARY(MyProjectName_Objects OBJECT 
    ${MyProjectName_Headers}  
    ${MyProjectName_Impl}
    )


TARGET_COMPILE_FEATURES(MyProjectName_Objects
    PRIVATE cxx_std_20
    )


target_include_directories(MyProjectName_Objects
    PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/..>
    $<INSTALL_INTERFACE:include>
)

ADD_LIBRARY(MyProjectName $<TARGET_OBJECTS:MyProjectName_Objects>)

ADD_LIBRARY(MyProjectName::MyProjectName ALIAS MyProjectName)



SET_TARGET_PROPERTIES( MyProjectName
    PROPERTIES
    CXX_EXTENSIONS NO
    CXX_STANDARD 20
    CXX_STANDARD_REQUIRED YES
    COMPILE_OPTIONS "-Wall -Wextra -Werror"
    PUBLIC_HEADER "${MyProjectName_headers}"
    )

INCLUDE(GNUInstallDirs)

INSTALL(TARGETS MyProjectName MyProjectName_Objects
    EXPORT MyProjectNameTargets
    RUNTIME DESTINATION ${CMAKE_INSTAL_BINDIR}
    LIBRARY DESTINATION ${CMAKE_INSTAL_LIBDIR}
    ARCHIVE DESTINATION ${CMAKE_INSTAL_LIBDIR}
    INCLUDES DESTINATION ${CMAKE_INSTAL_INCLUDEDIR}/third_party
    PUBLIC_HEADER DESTINATION ${CMAKE_INSTAL_INCLUDEDIR}/MyProjectName
    )

INSTALL( EXPORT MyProjectNameTargets
    FILE MyProjectNameTargets.cmake
    NAMESPACE MyProjectName::
    DESTINATION ${CMAKE_INSTAL_LIBDIR}/cmake/MyProjectName)
    
enable_testing()

# tests

include(GoogleTest)
include(CTest)

ADD_EXECUTABLE(example.t example.t.cpp)
TARGET_LINK_LIBRARIES(example.t MyProjectName::MyProjectName GTest::gtest_main)
gtest_discover_tests(example.t)