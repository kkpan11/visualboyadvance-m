add_library(vbam-components-filters OBJECT)

unset(extra_src)
if(WINXP)
    list(APPEND extra_src
        ${CMAKE_SOURCE_DIR}/third_party/quake3-sqrt/quake3-sqrt.h
    )
    target_include_directories(vbam-components-filters PRIVATE ${CMAKE_SOURCE_DIR}/third_party/quake3-sqrt)
endif()

target_sources(vbam-components-filters
    PRIVATE
    internal/2xSaI.cpp
    internal/admame.cpp
    internal/bilinear.cpp
    internal/hq2x.cpp
    internal/hq2x.h
    internal/interp.h
    internal/lq2x.h
    internal/pixel.cpp
    internal/scanline.cpp
    internal/simpleFilter.cpp
    internal/xBRZ/xbrz.cpp
    internal/xBRZ/xbrz.h
    internal/xBRZ/xbrz_config.h
    internal/xBRZ/xbrz_tools.h
    internal/xbrzfilter.cpp
    ${extra_src}

    PUBLIC
    filters.h
)

if(ENABLE_ASM_SCALERS)
    target_sources(vbam-components-filters
        PRIVATE
        internal/hq/asm/hq3x_16.asm
        internal/hq/asm/hq3x_32.asm
        internal/hq/asm/hq4x_16.asm
        internal/hq/asm/hq4x_32.asm
        internal/hq/asm/hq3x32.cpp
        internal/hq/asm/macros.mac
    )
    if(ENABLE_MMX)
        set(SRC_FILTERS ${SRC_FILTERS} internal/2xSaImmx.asm)
    endif()
else()
    target_sources(vbam-components-filters
        PRIVATE
        internal/hq/c/hq3x_pattern.h
        internal/hq/c/hq4x_pattern.h
        internal/hq/c/hq_base.h
        internal/hq/c/hq_implementation.cpp
        internal/hq/c/hq_shared.h
    )
endif()
