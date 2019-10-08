file(GLOB ROOT_SOURCE *.cpp)
file(GLOB ROOT_INLINE *.inl)
file(GLOB ROOT_HEADER *.hpp)
file(GLOB ROOT_TEXT ../*.txt)
file(GLOB ROOT_MD ../*.md)
file(GLOB ROOT_NAT ../util/glm.natvis)

file(GLOB_RECURSE CORE_SOURCE ./detail/*.cpp)
file(GLOB_RECURSE CORE_INLINE ./detail/*.inl)
file(GLOB_RECURSE CORE_HEADER ./detail/*.hpp)

file(GLOB_RECURSE EXT_SOURCE ./ext/*.cpp)
file(GLOB_RECURSE EXT_INLINE ./ext/*.inl)
file(GLOB_RECURSE EXT_HEADER ./ext/*.hpp)

file(GLOB_RECURSE GTC_SOURCE ./gtc/*.cpp)
file(GLOB_RECURSE GTC_INLINE ./gtc/*.inl)
file(GLOB_RECURSE GTC_HEADER ./gtc/*.hpp)

file(GLOB_RECURSE GTX_SOURCE ./gtx/*.cpp)
file(GLOB_RECURSE GTX_INLINE ./gtx/*.inl)
file(GLOB_RECURSE GTX_HEADER ./gtx/*.hpp)

file(GLOB_RECURSE SIMD_SOURCE ./simd/*.cpp)
file(GLOB_RECURSE SIMD_INLINE ./simd/*.inl)
file(GLOB_RECURSE SIMD_HEADER ./simd/*.h)

source_group("Text Files" FILES ${ROOT_TEXT} ${ROOT_MD})
source_group("Core Files" FILES ${CORE_SOURCE})
source_group("Core Files" FILES ${CORE_INLINE})
source_group("Core Files" FILES ${CORE_HEADER})
source_group("EXT Files" FILES ${EXT_SOURCE})
source_group("EXT Files" FILES ${EXT_INLINE})
source_group("EXT Files" FILES ${EXT_HEADER})
source_group("GTC Files" FILES ${GTC_SOURCE})
source_group("GTC Files" FILES ${GTC_INLINE})
source_group("GTC Files" FILES ${GTC_HEADER})
source_group("GTX Files" FILES ${GTX_SOURCE})
source_group("GTX Files" FILES ${GTX_INLINE})
source_group("GTX Files" FILES ${GTX_HEADER})
source_group("SIMD Files" FILES ${SIMD_SOURCE})
source_group("SIMD Files" FILES ${SIMD_INLINE})
source_group("SIMD Files" FILES ${SIMD_HEADER})

set(SOURCES ${ROOT_SOURCE} ${CORE_SOURCE} ${EXT_SOURCE} ${GTC_SOURCE} ${GTX_SOURCE} ${SIMD_SOURCE})

if (GLM_BUILD_SHARED_LIBS)
    add_library(glm_shared SHARED ${SOURCES})

    set_target_properties(glm_shared PROPERTIES
                          ARCHIVE_OUTPUT_NAME "glm"
                          DEBUG_POSTFIX d)

    target_include_directories(glm_shared PUBLIC $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/..>
                                                 $<INSTALL_INTERFACE:include/glm>)
    install(TARGETS glm_shared
            EXPORT glm-targets
            ARCHIVE DESTINATION lib
            LIBRARY DESTINATION lib
            RUNTIME DESTINATION bin)
endif ()

if (GLM_BUILD_STATIC_LIBS)
    add_library(glm_static STATIC ${SOURCES})

    set_target_properties(glm_static PROPERTIES
                          RUNTIME_OUTPUT_NAME "glm"
                          LIBRARY_OUTPUT_NAME "glm"
                          DEBUG_POSTFIX d)

    target_include_directories(glm_static PUBLIC $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/..>
                                                 $<INSTALL_INTERFACE:include/glm>)
    install(TARGETS glm_static
            EXPORT glm-targets
            ARCHIVE DESTINATION lib
            LIBRARY DESTINATION lib
            RUNTIME DESTINATION bin)
endif ()

install(FILES ${ROOT_HEADER} ${ROOT_INLINE} DESTINATION include/glm)
install(FILES ${CORE_HEADER} ${CORE_INLINE} DESTINATION include/glm/detail)
install(FILES ${EXT_HEADER} ${EXT_INLINE} DESTINATION include/glm/ext)
install(FILES ${GTC_HEADER} ${GTC_INLINE} DESTINATION include/glm/gtc)
install(FILES ${GTX_HEADER} ${GTX_INLINE} DESTINATION include/glm/gtx)
install(FILES ${SIMD_HEADER} ${SIMD_INLINE} DESTINATION include/glm/simd)

install(EXPORT glm-targets
        NAMESPACE glm::
        FILE glm-config.cmake
        DESTINATION lib/cmake/glm)
