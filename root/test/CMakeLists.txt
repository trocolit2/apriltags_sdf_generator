#####################
## TEST MAIN CMAKE ##
#####################

find_package(Boost COMPONENTS unit_test_framework REQUIRED)
include_directories (${Boost_INCLUDE_DIRS})

#I like to keep test files in a separate source directory called test
file(GLOB TEST_FILES RELATIVE ${CMAKE_CURRENT_SOURCE_DIR} *test.cpp)

# make all the tests bins
foreach(testSrc ${TEST_FILES})
        #Extract the filename without an extension (NAME_WE)
        get_filename_component(testName ${testSrc} NAME_WE)

        #Add compile target
        add_executable(${testName} ${testSrc})

        #link to Boost libraries AND your targets and dependencies
        target_link_libraries(${testName} ${Boost_LIBRARIES} ${OpenCV_LIBS} xml_genlib)

        #I like to move testing binaries into a testBin directory
        set_target_properties(${testName} PROPERTIES 
                 RUNTIME_OUTPUT_DIRECTORY ${bin_path}/testbin)

        #Finally add it to test execution - 
        #Notice the WORKING_DIRECTORY and COMMAND
        add_test(NAME ${testName} 
                WORKING_DIRECTORY ${bin_path}/testbin 
                COMMAND ${bin_path}/testbin/${testName})
endforeach(testSrc)
                                                             
