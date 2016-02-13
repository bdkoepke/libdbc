cmake_minimum_required(VERSION 2.8)
include(noweb.cmake)
include(UseLatex)
set(hoare_C
    contract.c)
set(hoare_H
    contract.h
    unsafe.h)
set(hoare_TEX
    contract.tex
    unsafe.tex)

tangle("${hoare_H}")
tangle("${hoare_C}")
weave("${hoare_TEX}")
add_custom_target(tangle DEPENDS ${hoare_H} ${hoare_C})
add_custom_target(weave DEPENDS ${hoare_TEX})

add_library(hoare SHARED ${hoare_C})
add_dependencies(hoare tangle)
target_link_libraries(hoare m)

add_latex_document(libhoare.tex
                   INPUTS preamble.tex motivation.tex
                   FORCE_PDF
                   TARGET_NAME libhoare)
add_dependencies(libhoare_pdf weave)
