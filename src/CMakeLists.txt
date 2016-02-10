cmake_minimum_required(VERSION 2.8)
include(noweb.cmake)
include(UseLatex)
set(dbc_C
    contract.c)
set(dbc_H
    contract.h
    unsafe.h)
set(dbc_TEX
    contract.tex
    unsafe.tex)

tangle("${dbc_H}")
tangle("${dbc_C}")
weave("${dbc_TEX}")
add_custom_target(tangle DEPENDS ${dbc_H} ${dbc_C})
add_custom_target(weave DEPENDS ${dbc_TEX})

add_library(dbc SHARED ${dbc_C})
add_dependencies(dbc tangle)
target_link_libraries(dbc m)

add_latex_document(libcontract.tex
                   FORCE_PDF
                   TARGET_NAME libcontract)
add_dependencies(libcontract_pdf weave)
