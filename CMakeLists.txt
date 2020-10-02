cmake_minimum_required(VERSION 3.10)

# set the project name
project(sbusAnalyzer)

include_directories(sbusAnalyzer_src "./source")
file(GLOB sbusAnalyzer_src
	 CONFIGURE_DEPENDS
     "source/*.cpp"
)

if("${CMAKE_GENERATOR_PLATFORM}" STREQUAL "Win32")
	find_library(SDKAnalyzerLib Analyzer PATHS AnalyzerSDK/lib REQUIRED )
elseif("${CMAKE_GENERATOR_PLATFORM}" STREQUAL "x64")
	find_library(SDKAnalyzerLib Analyzer64 PATHS AnalyzerSDK/lib REQUIRED )
else()
	message(FATAL_ERROR "Invalid arch. Please set arch to x64 or Win32")
endif()

add_library(sbusAnalyzer SHARED ${sbusAnalyzer_src})

link_directories(AnalyzerSDK/lib)
target_link_libraries(sbusAnalyzer PUBLIC ${SDKAnalyzerLib})

target_include_directories(sbusAnalyzer PUBLIC AnalyzerSDK/include)